# Preparation

* Init the submodules
* Apply patches where applicable
* Install requiered packages (check documentation of yocto, keystone, u-boot for this)

# Create linux image

Use the freedom-u-sdk to create a linux image by running 

```console
kas build freedom-u-sdk/scripts/kas/unmatched.yml --target demo-coreip-cli
```

from ./dist. This will take some time(easily two hours on a powerful machine).

# Build keystone

Follow the default keystone build instructions.

# Build the SPL and u-boot

```console
export OPENSBI=<path to keystone/build/sm.build/platform/generic/firmware/fw_dynamic.bin>
cd <U-Boot-dir>
make sifive_unmatched_defconfig
make FW_PAYLOAD=<path to keystone/build/sm.build/platform/generic/firmware/fw_payload.bin>
```

# Prepare the SD card

```console
xzcat demo-coreip-cli-unmatched.wic.xz | sudo dd of=/dev/mmcblk0 bs=512K iflag=fullblock oflag=direct conv=fsync status=progress
sudo dd if=spl/u-boot-spl.bin of=/dev/mmcblk0 seek=34
sudo dd if=u-boot.itb of=/dev/mmcblk0 seek=2082
```
