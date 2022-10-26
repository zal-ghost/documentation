# kernel/sched/core.c

- ghost使用了kernfs，L214`static int __init ghost_setup_root(void)`至L332`static void __exit ghostfs_exit(void)`结束均与ghost使用的kernfs虚拟文件系统有关。
- L335`static DEFINE_SPINLOCK(cpu_rsvp);`是per_cpu(enclave)写操作的锁。
- L357`int ghost_add_cpus(struct ghost_enclave *e, const struct cpumask *new_cpus)`

## Notes

```c
#define per_cpu_ptr(ptr, cpu)   ({ (void)(cpu); (ptr); })
#define per_cpu(var, cpu)	(*per_cpu_ptr(&(var), cpu))
```
