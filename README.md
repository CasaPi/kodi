# KODI

> Kodi (previously known as XBMC), is a free, open source (GPL) multimedia player.<br>
> **-[wiki.archlinux.org](https://wiki.archlinux.org/index.php/Kodi)**

Source: [https://wiki.archlinux.org/index.php/Kodi#Raspberry_Pi](https://wiki.archlinux.org/index.php/Kodi#Raspberry_Pi)

## Installation

* Install the *kodi-rbp* package instead of *kodi* from the Arch Linux ARM repository:
  (this package ships with a systemd service to run in standalone mode)
```
$ sudo pacman -S kodi-rbp (instead of kodi, choose default:1:mesa-libgl)
```

* To get hardware acceleration working:<br>
```
$ sudo pacman -S omxplayer-git xorg-xrefresh xorg-xset
```

* To enable typing with a physical keyboard, add the udev following rule <br>
  to `/etc/udev/rules.d/raspberrypi.rules`:
```
SUBSYSTEM=="tty", KERNEL=="tty0", GROUP="tty", MODE="0666"
```

* Modify the memory reserved for GPU.
  It is 64 MB by default. This is insufficient for GPU accelerated HD video playback.<br>
  Users can increase this value via a simple edit to the gpu_mem tag in `/boot/config.txt`.<br>
  A value of **at least 128 MB is recommended**.

* Reboot.

## Run Kodi in a Window Manager

* Create a script `~/kodi.sh`:
```
#!/bin/bash
kodi-standalone
sudo chvt 2
sleep 1
sudo chvt 3
```

For more details:
> The  command  chvt  N makes /dev/ttyN the foreground terminal.  (The corresponding screen is created if it did not exist yet. To get rid of unused VTs, use deallocvt(1).)  The key combination (Ctrl-)LeftAlt-FN (with N in the range 1-12) usually has  a similar effect.<br>
> **-[Extract from the man](http://man7.org/linux/man-pages/man1/chvt.1.html)**

The purpose (above) is to change the tty in order to have a clean foreground terminal for our Kodi.<br>
If we keep the tty1, we may look some output's messages (from the boot process for instance) on the standalone kodi window.<br>
If you are curious, just remove the three last lines and execute the script.

* Finally, add execute permission and run the script:
```
$ chmod +x kodi.sh
$ ~/kodi.sh
```

## Add a Kodi Remote Control

Source: [http://forum.kodi.tv/showthread.php?tid=221700](http://forum.kodi.tv/showthread.php?tid=221700)

### Enable the service

> Go to **System->Settings->Services->Webserver** and enable the setting **Allow control of Kodi via HTTP**.<br>
> Take note of the Port number that is selected, the Username and the Password (if any).<br>
> You’ll need to input this in KodiRemote (smartphone);<br>
> Go to **System->Settings->Services->Remote control** and<br>
> enable the settings **Allow programs on this system to control XBMC**<br>
> and **Allow programs on other systems to control XBMC**.<br>
> **-http://forum.kodi.tv**

### Information
* Fisrt of all, get system information:
	* Media name (default:Kodi) ----> *System > Services > General*
	* IP address --------------------------> *System > System info*
	* Port (default: 8080)  --------------> *System > Services > Web service*
	* Username (default:kodi)
	* Password (default, no password)

* Install Kodi Remote via GoogleStore on your Androidphone and configure it with the information above.

* To make your Kodi media center detectable, install *avahi* on your system.

## Autostart Kodi

Source: [http://kodi.wiki/view/HOW-TO:Autostart_Kodi_for_Linux](http://kodi.wiki/view/HOW-TO:Autostart_Kodi_for_Linux)

## Configuration File *advancedsettings.xml*

Source: [http://kodi.wiki/view/Advancedsettings.xml](http://kodi.wiki/view/Advancedsettings.xml)

We may need to backup the kodi config file.

## Start a turned-off Kodi on a running platform using Kore

Take a look at [Keakup](https://github.com/Ventto/kakeup).

## FAQ
###Issue: "lsb_release : command not found" ?
```
$ sudo pacman -S lsb_release
```
