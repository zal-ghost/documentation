# kernel/sched/sched.h

这里有一些重要的ghost数据结构。

- L154`struct ghost_enclave`，组织了ghost一个enclave的数据结构，ghost调度单位就是它。
    - L155`const struct ghost_abi *abi;`指向ghost调度abi。

## Notes

- `struct task_struct`应该是线程数据结构？
