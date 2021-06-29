# Installing Linaro Linux on DragonBoard 410c

## Install Debian Developer Image on SD Card from Host

### Step 1: Connect SD Card to Host

Insert card to reader and ensure everything has been backed up.

### Step 2: Find SD Card Device

**Linux**

Run `lsblk`

The device is usually `/dev/sdN` where N is usually a-d. Example: `/dev/sdc`

**MacOS**

Run `diskutil list`

The device is `/dev/diskN` where N is usually 1-9. Example: `/dev/disk2`

_Note: The disk is the one that appeared after you connected the SD card._

### Step 3: Download Latest Debian Image

Go to [http://releases.linaro.org/96boards/dragonboard410c/linaro/debian/latest/](http://releases.linaro.org/96boards/dragonboard410c/linaro/debian/latest/) and download the zip with filename `dragonboard-410c-sdcard-developer-*.zip` 

Example: [Debian SID developer image](http://releases.linaro.org/96boards/dragonboard410c/linaro/debian/21.03/dragonboard-410c-sdcard-developer-sid-1029.zip)

### Step 4: Unzip the Image

Run `unzip dragonboard-410c-sdcard-developer-*.zip`

Example: 

```
unzip dragonboard-410c-sdcard-developer-sid-1029.zip
```

### Step 5: Write Image to SD Card

**Warning: you can lose data if you have not backed up the card or specify the wrong device name** 

Write the image to SD card using _dd_.

Run `sudo dd if=dragonboard-410c-sdcard-developer-*.img of=XXX bs=4m`, where _XXX_ stands for the device obtained in step 2.

Example: 

```
sudo dd if=dragonboard-410c-sdcard-developer-sid-1029.img of=/dev/disk2 bs=4m
```

This process may take a while.

To ensure everything has been written run : `sudo sync`


### Step 6: Unmount All SD Card Partitions from Host

**Linux**

Run `sudo umount XXX`, where XXX is the device (ex: `/dev/sdc`)


**MacOS**

Run `diskutil unmountDisk XXX`, where XXX is the device (ex: `/dev/disk2`)


### Step 7: Prepare Board

Insert the SD card in the board's reader.

Set the *SD Boot* [Dip-switch](https://www.96boards.org/documentation/consumer/dragonboard/dragonboard410c/hardware-docs/hardware-user-manual.md.html#dip-switch) to **on**. Example switch state: __0-1-0-0__


### Step 8: Boot the Board

If there is a partition named _rootfs_ on the eMMC, it may be used as `/`, so please make sure there is none.

Plugin the board to power to boot.

You should get a login prompt on the monitor. 

```
User: linaro
Pass: linaro
```


## Install Debian Developer Image on eMMC from Host


### Step 1: Download Necessary Images

You will need the following:

* the latest [bootloader image](https://releases.linaro.org/96boards/dragonboard410c/linaro/rescue/latest/) for booting Linux `dragonboard-410c-bootloader-emmc-linux-*.zip`
*  the [latest](http://releases.linaro.org/96boards/dragonboard410c/linaro/debian/latest/) rootfs (`linaro-*-developer-dragonboard-410c-*.img.gz`) and boot (`boot-linaro-*-dragonboard-410c-*.img.gz`) images for Debian 



### Step 2: Unzip the Images

Unzip the bootloader: `unzip *.zip`

Unzip the other images: `gunzip *.gz`

Afterwards, for 21.03 you should have the following files and directory:

```
dragonboard-410c-bootloader-emmc-linux-159/
boot-linaro-sid-dragonboard-410c-1029.img
linaro-sid-developer-dragonboard-410c-1029.img
```


### Step 3: Prepare Board


Set the *SD Boot* [Dip-switch](https://www.96boards.org/documentation/consumer/dragonboard/dragonboard410c/hardware-docs/hardware-user-manual.md.html#dip-switch) to **off**. Example switch state: __0-0-0-0__


While pressing Vol-, plug in the board to power to boot into fastboot.


### Step 4: Flash the Bootloader

Change to the extracted bootloader directory and run `./flashall`.

For example:

```
cd dragonboard-410c-bootloader-emmc-linux-159/
./flashall
```

### Step 5: Reboot

Reboot so you can install everything else.

```
fastboot reboot
```

Check that the device is visible against through fastboot.

```
fastboot devices
```


### Step 6: Flash Other Images

For example, for 21.03:

```
fastboot flash boot boot-linaro-sid-dragonboard-410c-1029.img
fastboot flash rootfs linaro-sid-developer-dragonboard-410c-1029.img
```

### Step 7: Reboot the Board

If there is a partition named _rootfs_ on the SD Card, it may be used as `/`, so please make sure there is none or remove the SD card. Then proceed and reboot.

```
fastboot reboot
```

You should get a login prompt on the monitor. 

```
User: linaro
Pass: linaro
```