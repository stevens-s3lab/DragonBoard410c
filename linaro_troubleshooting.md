# Troubleshooting Linaro Linux

## No Video Output Over HDMI

### Board Does not Support Resolution + Refresh Rate Used by Monitor

On certain monitors the board does not produce video output over HDMI. This has been observed when using Linaro's Debian image. 

First, to ensure that your issue is not a hardware one make sure to boot using the Android image installed on the board's eMMC. This can be done by turning the board off, ensuring the *SD Boot* [Dip-switch](https://www.96boards.org/documentation/consumer/dragonboard/dragonboard410c/hardware-docs/hardware-user-manual.md.html#dip-switch) is set to **off** (example switch state: __0-0-0-0__), and turning it on again.

A possible solution is to instruct the kernel to use a specific video mode during boot time. This can be done by adding the following to its command line:
`video=HDMI-A-1:1920x1080@50`, where we assume _1920x1080_ and _50_ are a valid resolution and refresh rate, respectively.

__Examples are based on Debian SID Developer Image on SD Card__

#### Step 1: Download the boot image that corresponds to the image installed on the SD card. 

1. Download [boot-linaro-sid-dragonboard-410c-1029.img.gz](http://releases.linaro.org/96boards/dragonboard410c/linaro/debian/latest/boot-sdcard-linaro-sid-dragonboard-410c-1029.img.gz). For the latest, checkout [http://releases.linaro.org/96boards/dragonboard410c/linaro/debian/latest/](http://releases.linaro.org/96boards/dragonboard410c/linaro/debian/latest/).
2. Decompress it: `gunzip boot-sdcard-linaro-sid-dragonboard-410c-1029.img.gz`


#### Step 2: Modify Command Line

#### Option 1: Booting with Modified Command Line using Fastboot

Instead of modifying the boot image installed on the board, you can boot with a custom command line using fastboot.

```
fastboot boot --cmdline "root=PARTLABEL=rootfs console=ttyMSM0,115200n8 video=HDMI-A-1:1920x1080@50" boot-sdcard-linaro-sid-dragonboard-410c-1029.img
```

#### Option 2: Modify Command Line in Boot Image and Install it

Modify the boot image to change the command line passed to the kernel. 

##### Step 1

Get current command line: `abootimg -i image`. 

For example:

```
abootimg -i boot-sdcard-linaro-sid-dragonboard-410c-1029.img
```

The output will be something like the following:

```
* cmdline = root=/dev/disk/by-partlabel/rootfs rw rootwait console=tty0 console=ttyMSM0,115200n8
```


##### Step 2 

Make a copy of the original boot image, which you are going to use to modify the command line.

For example: `cp boot-sdcard-linaro-sid-dragonboard-410c-1029.img boot-sdcard-video.img`

##### Step 3

Modify command line of the new image by appending video option:

```
abootimg -u image -c "cmdline video=HDMI-A-1:1920x1080@50" boot-sdcard-video.img
```

where _cmdline_ is the value returned in step 1.

For example:

```
abootimg -u image -c "cmdline = root=/dev/disk/by-partlabel/rootfs rw rootwait console=tty0 console=ttyMSM0,115200n8 video=HDMI-A-1:1920x1080@50" boot-sdcard-video.img
```

##### Step 4

Test new image by booting it with fastboot.

For example:

```
fastboot boot boot-sdcard-video.img
```

##### Step 5

If all went well, flash boot image to SD Card using fastboot and reboot.

```
fastboot flash boot boot-sdcard-video.img
fastboot reboot
```


### Force Display Resolution and Bypass EDID

_Did not work for me_

[https://www.96boards.org/documentation/consumer/dragonboard/dragonboard410c/guides/force-display-res.md.html](https://www.96boards.org/documentation/consumer/dragonboard/dragonboard410c/guides/force-display-res.md.html)
