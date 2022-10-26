# Sched.h

98行，宏定义`__TASK_DEFERRABLE_WAKEUP`为0x1000。

592行，定义结构体`sched_ghost_entity`，具体内容为：

* `run_list`：记录运行任务列表
* `last_runnable_at`：记录上一次运行时间（kernel time）
* `dst_q`：kernel->agent消息队列
* `status_word`：？
* 一组标识：`blocked_task`，`yield_task`，`new_task`，`agent`，`bpf_cannot_load_prog`，分别表示是否阻塞，是否让出，是否是新建，是否是调度Agent，？
* `twi`标识，分别为`last_ran_cpu`，`wake_up_cpu`，`waker_cpu`，分别记录了上次在哪个CPU上运行，？，？
* `task_list`：记录所有任务的列表
* `rcu_head`：用于Read-Copy Update

其中所有`enclave`用于保证读写不越界，可以不用管。

784行`task_struct`结构体中为ghost添加了几个成员变量，主要是`struct sched_ghost_entity ghost;` 让每个task都有向ghost entity的指针。这与其它调度类的实现是类似的。
