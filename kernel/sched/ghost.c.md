# kernel/sched/ghost.c

这里是主要的ghost abis。

L3816`static inline int produce_for_task`, L3822`static inline int produce_for_agent` 是主要的发送消息的方法。

L3995-4215`task_deliver_msg_*`用来构建和发送不同类型的消息。

L5193

```c
static bool _ghost_commit_txn(int run_cpu, bool sync, int64_t rendezvous,
			      int *commit_state, bool *need_rendezvous)
```

似乎是用来提交transaction的大函数，值得关注。TODO

## Notes

- `struct rq` This is the main, per-CPU runqueue data structure.
