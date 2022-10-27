# ghOSt-Userspace

To test the userspace component, you need to compile it first. Follow the instruction by google and install `bazel` to perform the building process. You may need to change `experiments/scripts/options.py`, change the global value `_FIRST_CPU` to 0 to align with your device.

```bash
bazel build -c opt ...
```

Build all targets.

## Centralized-Queue

To start the centralized queue test with RockDB, start the testing with:

```bash
sudo bazel-bin/experiments/scripts/centralized-queue.par {option}
```

Where the option could be `cfs` or `ghost` to test either ghost scheduler or the Linux's cfs scheduler. You may encounter failures, see issue #24 in `google/ghost-userspace` on how to solve it, since some of the testing hard-coded n workers and logical core bindings.

Some tests, like _shinjuku_, requires multiple logical cores to run, i.e., 8, but platforms may not be captable to provide such many cores. This could be adjusted in testing scripts, like `shinjuku.py`, to change the value of `_NUM_ROCKDB_WORKERS` and the number passed to function `GetRocksDBOptions()` so that the actual worker numbers could be changed.

Note that binding `_FIRST_CPU` to 0 is, sometimes, not ideal since we avoid doing it while many services and system processes run on this core. 1 is a good choice.
