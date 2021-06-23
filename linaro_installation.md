# Installing Linaro Linux on DragonBoard 410c

**Other Documentation**

[Hardware Documentation](https://www.96boards.org/documentation/consumer/dragonboard/dragonboard410c/hardware-docs/index.html)

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

Example: `dragonboard-410c-sdcard-developer-sid-1029.zip`

### Step 5: Write Image to SD Card

**Warning: you can lose data if you have not backed up the card or specify the wrong device name** 

Write the image to SD card using _dd_.

Run `sudo dd if=dragonboard-410c-sdcard-developer-*.img of=XXX bs=4m`, where _XXX_ stands for the device obtained in step 2.

Example: `sudo dd if=dragonboard-410c-sdcard-developer-sid-1029.img of=/dev/disk2 bs=4m`

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

Plugin the board to power to boot.

You should get a login prompt on the monitor. 

```
User: linaro
Pass: linaro
```
