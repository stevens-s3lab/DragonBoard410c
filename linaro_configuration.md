# Configuring Linaro Linux on DragonBoard 410c

## Networking

### Debian SID Developer Image

_Copied from https://github.com/fixxxxxxer/guides/blob/master/headless-wifi.md_

This guide will outline two different methods of connecting to a WiFi network with your DragonBoard 410c

- Method one: Connect to WiFi using on-board terminal when running Linaro Debian on your DragonBoard 410c
- Method two: Connect to WiFi while accessing your DragonBoard 410c through external means (serial console or ssh "headless") with nmtui
- Method three: Connect to WiFi while accessing your DragonBoard 410c through external means (serial console or ssh "headless) with nmcli

#### Method one

In this method you will be working directly on the DragonBoard 410c. By the end of this instruction you will have connected to a WiFi network using only a terminal window on your local DragonBoard 410c desktop or developer OS.

1. DragonBoard is running Linaro Debian OS (standard Linux OS)
2. Open Terminal application and execute the following command

`$ nmtui`

This command will open an interactive GUI that will allow you to maneuver your way through the menus and choose your desired WiFi network to connect to. Follow the prompt to connect to a detected WiFi signal.

> NOTE: The DragonBoard 410c is only capable of detecting and connecting to the 2.4GHz WiFi band. If your router does not offer this frequency, the GUI will not show a useable network to connect to.

#### Method two

In this method you will be accessing your DragonBoard 410c through a host machine (personal computer, running either Linux or Mac OSX). Please see [headless access](headless-access.md) instructions to learn how to connect to your DragonBoard 410c via serial console using the [Grove Seeed Sensors Mezzanine](https://www.96boards.org/product/sensors-mezzanine/).

1. You are connected to the serial console of your DragonBoard 410c using the Grove Seeed Sensors Mezzanine and a USB Type-A to microUSB cable ([instructions](headless-access.md)
2. Execute the following command:

`$ nmtui`

This command will open an interactive GUI that will allow you to maneuver your way through the menus and choose your desired WiFi network to connect to. Follow the prompt to connect to a detected WiFi signal.

#### Method three

In case you don't have a HDMI monitor around and got UART access to the board (e.g. [UART adapter board](https://www.96boards.org/products/mezzanine/uarts/) or [Sensors Mezzanine
](https://www.96boards.org/products/mezzanine/sensors-mezzanine/)), there are quite a few easy ways for you to configure a wireless connection, so you can then remotely access your board without any extra cables (besides the power adapter).

To show the overall status of NetworkManager:

```shell
root@linaro-alip:~# nmcli general status
STATE         CONNECTIVITY  WIFI-HW  WIFI     WWAN-HW  WWAN    
disconnected  none          enabled  enabled  enabled  enabled
```

To show all connections:

```shell
root@linaro-alip:~# nmcli connection show
NAME  UUID  TYPE  DEVICE
```

To show the device status (for the devices recognized by Network Manager:

```shell
root@linaro-alip:~# nmcli device status
DEVICE  TYPE      STATE         CONNECTION
wlan0   wifi      disconnected  --         
lo      loopback  unmanaged     --         
```

To view the list of available access points:

```shell
root@linaro-alip:~# nmcli dev wifi list
*  SSID        MODE   CHAN  RATE       SIGNAL  BARS  SECURITY  
   foonet      Infra  7     54 Mbit/s  70      ▂▄▆_  WPA2      
   96boards    Infra  4     54 Mbit/s  80      ▂▄▆_  WPA2      
   linaro-wifi Infra  52    54 Mbit/s  7       ▂___  WPA2      
   debian      Infra  11    54 Mbit/s  89      ▂▄▆█  WPA1 WPA2
```

To connect to a WIFI access point, first create the connection:

```shell
root@linaro-alip:~# nmcli con add con-name WiFi ifname wlan0 type wifi ssid foonet
Connection 'WiFi' (4b40221c-9af9-45ae-b5df-7d8bfe301ad5) successfully added.
```

Then set up the password for your access point (e.g. for a WPA2 AP):

```shell
root@linaro-alip:~# nmcli con modify WiFi wifi-sec.key-mgmt wpa-psk
root@linaro-alip:~# nmcli con modify WiFi wifi-sec.psk myownpassword
```

Then just enable the connection:

```shell
root@linaro-alip:~# nmcli con up WiFi
```


Congratulations! You have connected your DragonBoard 410c to a useable WiFi network using only the command line (Terminal application)
