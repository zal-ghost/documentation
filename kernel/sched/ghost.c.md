# kernel/sched/ghost.c

这里是主要的ghost abis。

- L3816`static inline int produce_for_task`, L3822`static inline int produce_for_agent` 是主要的发送消息的方法。

## Notes

- `struct rq` This is the main, per-CPU runqueue data structure.
