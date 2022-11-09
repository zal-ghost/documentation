# Upgrade ghOSt to linux-6.0

这里记录一些non-trivial的merge conflicts。

## include/asm-generic/vmlinux.lds.h

L127左右定义了`SCHED_DATA`，对schedule class进行了排序，5.11中begin->end是低->高，
6.0中highest->lowest是高到低，顺序要反过来。
在定义了`CONFIG_SCHED_CLASS_GHOST`时将ghost的两个sched class插进去。

## kernel/sched/Makefile

这里所有option-dependent的build target在6.0全被删掉了，挪到了同目录的`build_*.c`。
根据文件注释这应该是考虑编译效率问题。
Ghost在这里添加了CFLAGS，需要保留在Makefile中；两个源文件挪到`build_policy.c`。

## include/linux/sched.h

L100左右ghost和RT分别加了一个bitmask，合并为

```c
/* RT specific auxilliary flag to mark RT lock waiters */
#define TASK_RTLOCK_WAIT		0x1000
#define __TASK_DEFERRABLE_WAKEUP	0x2000
#define TASK_STATE_MAX			0x4000
```

应该没有溢出风险。

L860左右accept both。

## include/linux/vmalloc.h

6.0给vmalloc后面都加了一个`__alloc_size(1)`标注，
ghost本身就是加了一个`vmalloc_user_node_flags`函数，给它也加上这个标注。

## kernel/entry/common.c

FIXME: L189左右对于`tick_nohz_user_enter_prepare`和`ghost_commit_greedy_txn`的顺序并不清楚。

## kernel/sched/fair.c

这里的主要修改源自kernel/sched/sched.h L2861

```c
static inline unsigned int rq_adj_nr_running(struct rq *rq)
{
	return rq->nr_running - extra_nr_running(rq);
}
```

因此需要把`rq->nr_running`替换为这个函数。于是这带来新的问题：一些新的取nr_running变量的需要按照这个更新。

更新处：L10085`should_we_balance`函数内。
L8050`detach_tasks`函数内，下文也改了。
感觉`cfs_rq`的都不需要改。

## kernel/sched/core.c

`CONFIG_SCHED_CORE`开启时重写了`pick_next_task`，ghost修改了`__pick_next_task`，
我们直接忽略`CONFIG_SCHED_CORE`开启时的`pick_next_task`。

`__setscheduler`被解构并inline掉了，改为在L7785覆盖掉之前的sched class。这样并不美观但由于需要attr，只能这样了。

L7685把两条check wrap起来，如果有ghost就不check。

L9903这里所有的+1都要反过来，因为放置顺序变了。

## kernel/sched/ghost-core.c

这一部分的修改主要源自于Linux6.0版本部分模块对于`ghost_commit_greedy_txn`函数的定义需求。
由于该函数的声明存在于kernel/sched/sched.h中，因此需要使用`EXPORT_SYMBORL`宏将其定义进行导出。
更新处：kernel/sched/ghost_core.c L724
更新方式：`EXPORT_SYMBOL(ghost_commit_greedy_txn);`
