# Troubleshooting Linaro Linux

## No Video Output Over HDMI

### Board Does not Support Resolution + Refresh Rate Used by Monitor

On certain monitors the board does not produce video output over HDMI. This has been observed when using Linaro's Debian image. 

First, to ensure that your issue is not a hardware one make sure to boot using the Android image installed on the board's eMMC. This can be done by turning the board off, ensuring the *SD Boot* [Dip-switch](https://www.96boards.org/documentation/consumer/dragonboard/dragonboard410c/hardware-docs/hardware-user-manual.md.html#dip-switch) is set to **off** (example switch state: __0-0-0-0__), and turning it on again.

A possible solution is to instruct the kernel to use a specific video mode during boot time. This can be done by adding the following to its command line:
`video=HDMI-A-1:1920x1080@50`, where we assume _1920x1080_ and _50_ are a valid resolution and refresh rate, respectively.

#### Booting with Modified Command Line using Fastboot

Instead of modifying the boot image installed on the board, you can boot with a custom command line using fastboot.

##### Debian Developer Image on SD Card

1. Download the developer **boot** image that corresponds to the image installed on the SD card. For example, for Debian SID download [boot-linaro-sid-dragonboard-410c-1029.img.gz](http://releases.linaro.org/96boards/dragonboard410c/linaro/debian/latest/boot-sdcard-linaro-sid-dragonboard-410c-1029.img.gz). For the latest, checkout [http://releases.linaro.org/96boards/dragonboard410c/linaro/debian/latest/](http://releases.linaro.org/96boards/dragonboard410c/linaro/debian/latest/).
2. Decompress it: `gunzip boot-sdcard-linaro-sid-dragonboard-410c-1029.img.gz`
3. Boot it: `fastboot boot --cmdline "root=PARTLABEL=rootfs console=ttyMSM0,115200n8 video=HDMI-A-1:1920x1080@50" boot-sdcard-linaro-sid-dragonboard-410c-1029.img`
