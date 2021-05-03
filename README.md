# Raspberry OS and RTL8188EUS 802.11n Wireless Network Adapter

Ok, RTL8188 doesn't work as WPA_supplicant from the scratch. No problem, but doesn't work at all. Here are easy steps to get it back to work. Sad, but at first you should connect it with Ethernet cable. 

## Before you will boot it

1. Deploy OS' image onto SD-card
2. Put empty file called `ssh` to `boot` partition
3. Put `wpa_supplicant.conf` file to `boot` partition
4. Connect to ethernet cable
5. Put RTL8188EUS into USB port (any)
6. Start

## SSH to your new setup

Time to fix "driver problem". Open `nano /etc/systemd/system/dbus-fi.w1.wpa_supplicant1.service` and find row `ExecStart=/sbin/wpa_supplicant -u -s -O /run/wpa_supplicant` you need to change it in a way: `ExecStart=/sbin/wpa_supplicant -u -s -O /run/wpa_supplicant -c /etc/wpa_supplicant/wpa_supplicant.conf -i wlan0 -D wext` where `/etc/wpa_supplicant/wpa_supplicant.conf` is your config file from \#3 above and `-i wlan0 -D wext` are: **i** - for interface name (normally `wlan0`) and **D** is to show which one driver it should to use.

Now check `/etc/network/interfaces` and add `wireless-power off` to tell to system not to switch power off if here is no active connection.

Now do all the things you shoud do on first start:
1. `sudo apt update`
2. `sudo apt -y --fix-missing upgrade`
3. `sudo raspi-config`
4. etc
5. Reboot

## A few notes

You can check how this "driver fix" will help you just from command line, use `/sbin/wpa_supplicant -c/etc/wpa_supplicant/wpa_supplicant.conf -iwlan0 -Dwext`
