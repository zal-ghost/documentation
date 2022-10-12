# Note on Install

You need to first compile and install ghost-kernel, then install the ghost-userspace component to use ghOSt scheduler class.

## Install ghost-kernel

**Environment**: Ubuntu-20.04.5 Server LTS, Virtualbox 7.0

**Caution**: Allocate more than 50G harddrive to ensure enough space for compilation.

1. Clone the repository

```bash
git clone https://github.com/google/ghost-kernel.git
```

2. Install necessary components

```bash
sudo apt install build-essential libssl-dev libelf-dev ncurses-dev flex bison
```

3. Copy original kernel configuration

```bash
cp /boot/config-$(uname -r) .config
```

4. Enable ghOSt scheduler class

```bash
make menuconfig # then enable [ ] ghOSt scheduler class
```

5. Disable `SYSTEM_TRUSTED_KEYS` and `DEBUG_INFO_BTF`

use an editor, set `CONFIG_SYSTEM_TRUSTED_KEYS` to `""` and set `CONFIG_DEBUG_INFO_BTF` to `n` in `.config`.

6. Compile kernel

```bash
make -j$(nproc)
```

7. Install headers and components

```bash
sudo make headers_install INSTALL_HDR_PATH=/usr
sudo make modules_install
```

8. Install kernel

```bash
sudo make install
```

9. Reboot

```bash
sudo reboot
```

## Install ghost-userspace

Follow the `README.md` in google's repository would be fine. Just watch out if you are using default `gcc` on Ubuntu-20.04, it does not support `-std=c++20`, modify it in `.bazelrc` to `-std=c++2a` would work.
