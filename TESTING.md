# ghOSt-Userspace

To test the userspace component, you need to compile it first. Follow the instruction by google and install `bazel` to perform the building process. You may need to change `experinment/scripts/options.py`, change the global value `_FIRST_CPU` to 0 to align with your device.

```bash
bazel build -c opt ...
```

Build all targets.

## Centralized-Queue

To start the centralized queue test with RockDB, start the testing with:

```bash
sudo bazel-bin/experinments/scripts/centralized-queue.par {option}
```

Where the option could be `cfs` or `ghost` to test either ghost scheduler or the Linux's cfs scheduler.
