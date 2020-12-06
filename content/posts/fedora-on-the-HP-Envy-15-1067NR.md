---
title: Fedora on the HP Envy 15 1067NR
date: 2014-02-21T20:00:41+01:00
lastmod: 2014-02-21T20:00:41+01:00
author:
  - C-A de Salaberry
# authorlink: https://author.site
cover: /img/cover.jpg
categories:
  - debug
tags:
  - linux
  - fedora
  - ssd
draft: false
---

A collection of Fedora 20 optimisations for my current setup (the SSD in a OCZ Vertez 3 120Go). Notice that most of my tweaks come from fedy, an excellent open source tool that does pretty much everything needed to get a more user friendly OS.

<!--more-->

## SSD Optimisations

From [fedoraproject.org](https://ask.fedoraproject.org/en/question/41664/optimization-for-an-ssd-on-fedora-20/)


Add the following lines to **/etc/fstab**

	#SSD optimization,  /tmp to RAM 
	none             /tmp/                   tmpfs   size=10%                 0       0


=====


From [debian.org](https://wiki.debian.org/SSDOptimization)

Add the "noatime" (or "relatime") mount option in **/etc/fstab**, to disable (or significantly reduce) disk writes whenever a file is read. 


To support discards with lvm, go modify your **/etc/lvm/lvm.conf** and enable **issue_discards**:

	devices {
	...
	    # 1 enables; 0 disables.
	    #issue_discards = 0
	    issue_discards = 1
	}


=====


From [ArchLinux](https://wiki.archlinux.org/index.php/Solid_State_Drives)


Modifiy Mount flags to support **discard** and **relatime** :


```
/dev/mapper/fedora_hp--envy-root /                       ext4    defaults,relatime,discard        1 1
UUID=a8c8c6c9-7377-4ae7-860b-d2727cae704b /boot                   ext4    defaults,discard        1 2
/dev/mapper/fedora_hp--envy-home /home                   ext4    defaults,relatime,discard        1 2
/dev/mapper/fedora_hp--envy-swap swap                    swap    defaults,discard        0 0
```


=====


## Prevent sleep on lid closed


	$ vi /etc/systemd/logind.conf


change

	HandleLidSwitch=suspend


to

	HandleLidSwitch=ignore


then restart the login service:

	$ systemctl restart systemd-logind.service


Try closing the lid of the laptop and check it does not suspend.


=====


## GRUB Timeout


When your system boots, you’ll see the GRUB menu, or if you’ve enabled the menu to show by default. The GRUB menu is displayed for a predetermined number of seconds. However, on Fedora, the default timeout is only 5 seconds. So you may want to change this timeout.

* Backup the /etc/default/grub file.

```
cp  -v  /etc/default/grub  /etc/default/grub.bak
```


* As the root user, open up the /etc/default/grub file in your favorite text editor:

```
	vi   /etc/default/grub
```


* Now, edit the “GRUB_TIMEOUT” line. You will then have to regenerate the GRUB settings from the file.

* In terminal, run the following command to generate settings from /etc/default/grub file.

```
	grub2-mkconfig  -o  /boot/grub2/grub.cfg
```


* Reboot and enjoy.


=====


## Blackberry Dev

From [e-bluesoft.com](http://blog.e-bluesoft.com/?p=23):

Sometimes, after installing on Fedora, LPCExpresso does not start. The error shown is


```
java.lang.UnsatisfiedLinkError: Could not load SWT library. Reasons: 
        /usr/local/lpcxpresso_5.2.4_2122/lpcxpresso/configuration/org.eclipse.osgi/bundles/214/1/.cp/libswt-pi-gtk-4236.so: libgtk-x11-2.0.so.0: cannot open shared object file: No such file or directory
        no swt-pi-gtk in java.library.path
        /home/laji/.swt/lib/linux/x86/libswt-pi-gtk-4236.so: libgtk-x11-2.0.so.0: cannot open shared object file: No such file or directory
        Can't load library: /home/laji/.swt/lib/linux/x86/libswt-pi-gtk.so
```


To solve this dependency problem, gtk-engines for linux32 has to be installed on the system, with the command:


	$ yum install gtk2-engines.i686


Now LPCXpresso starts without any error.


=====


## Gedit
### Failed to load module 'pk-gtk-module'

From [mindthatbus.blogspot.fr](http://mindthatbus.blogspot.fr/2011/12/failed-to-load-module-pk-gtk-module.html)


When running gedit from a terminal I always got a message Failed to load module 'pk-gtk-module' but gedit seemed to start normally. to fix it on Fedora 18 with Gnome3, I did the following steps:

	$ yum remove PackageKit-gtk-module
	$ yum install PackageKit-gtk3-module


From the source:

If you're interested, these packages load fonts needed to display documents in other languages. 


### Failed to load module 'canberra-gtk-module'

From [fedoraforum.org](http://forums.fedoraforum.org/showthread.php?t=257157)

	$ yum install libcanberra-gtk2.i686

