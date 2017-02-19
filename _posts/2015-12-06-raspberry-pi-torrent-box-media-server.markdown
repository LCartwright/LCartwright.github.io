---
layout: post
title:  "So you want to make a torrentbox"
date:   2015-12-06
categories: raspberry-pi rpi torrent
---

# Raspberry Pi media server
If you're anything like me, you occasionaly dip into the legally ambigious world of torrenting linux distributions. If you're also anything like me; you live in a house (or flat) with more than one person using sharing an internet connection with limited bandswitch. If I were to download and share my "Distros" all day, everyone else in my house would have a hard time watching their favourite streaming site! If only there were a way to host an ultra-low power server, downloading my "Distros" during hours in which all sensible people would be asleep.

So as a learning project, I'll walk you through the steps required to solve the above problem.
Before we begin, you will need:

1. A Raspberry Pi (Model B+) with NOOBS installed
2. An External USB Storage Drive 
3. An Atheros based WiFi adapter (optional)

Following the blog post, you will end up with:

* An awesome raspberry pi
* A File server
* A remote torrent box
* A DNLA media server



## Connecting to WiFi
If you're planning on using the built in Ethernet adapter, you can skip this step. Go [here](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md) to see the original source.

With your [supported](http://elinux.org/RPi_USB_Wi-Fi_Adapters) and preferably atheros based USB WiFi adpter plugged in, enter the following command in the RPi terminal.

~~~ terminal
$ sudo iwlist wlan0 scan | grep "ESSID"
~~~

The above command scans for wireless networks in range of your device and returns a list of their SIDs. Hopefully the network you're trying to connect to should be in this list.

~~~ terminal
ESSID:"SKYE2123456"
ESSID:"BTHub4-1234"
ESSID:"BTWifi-with-FON"
ESSID:"BTWifi-X"
~~~

With the SID of your home network you want to connect to in hand, open up the wpa-supplicant file

~~~
$ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
~~~

Go to the end of the file and enter the following (making note of the SSID from earlier)

~~~
network={
    ssid="YOUR_ESSID"
    psk="YOUR_WIFI_PASSWORD"
}
~~~

Write the file (CTRL + O) and exit (CTRL + X).

Next, we'll setup the auto-reconnect script. The author of this script can be found [here](https://rpi.tnet.com/project/scripts/wifi_check).

First, lets go to an appropriate folder

~~~
cd /usr/local/bin
~~~

Download the script

~~~
$ sudo wget https://raw.github.com/dweeber/WiFi_Check/master/WiFi_Check -O /usr/local/bin/WiFi_Check
~~~

Modify the file to be executable

~~~
$ sudo chmod 0755 /usr/local/bin/WiFi_Check
~~~

Edit the Cron job table

~~~
$ crontab -e
~~~

With crontab open in edit mode, add the following line. This setups up our WiFi_Check script to run once every 5 minutes.

~~~
*/5 * * * * /usr/local/bin/WiFi_Check
~~~

At this stage you will have a stable WiFi connection, resilliant to router reboots. 

WiFi or not, it's usually a good idea to set up a static IP address for your RPi, follow (this)[https://www.modmypi.com/blog/tutorial-how-to-give-your-raspberry-pi-a-static-ip-address] guide to find out more.

## Mounting the external storage
Before we begin, update and upgrade your RPi (note -y will assume yes to all questions)

~~~
sudo apt-get update
sudo apt-get -y upgrade
~~~

TODO:

## Installing Samba (File Server)

## Installing the torrent daemon