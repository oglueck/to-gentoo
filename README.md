# to-gentoo

to-gentoo is a bash script that you can run on any pre-installed Linux distribution. It will replace the current system with a basic Gentoo system. If all goes well you have a fully working Gentoo system when the script finishes without even rebooting.

The basic Gentoo system is first installed in a subdirectory (/gentoo). When finished, your existing system is moved to an different subdirectory (/orig) and the Gentoo system is moved to the root filesystem.

Some files are not touched by this script however.

In fact only the following directories will be replaced with the Gentoo versions:
- etc
- lib (lib32, lib64)
- var
- usr
- sbin
- bin

Specifically the following directories will not be touched and will be kept:
- /root
- /home
- /boot
- /opt
- /mnt
- all technical directories: /dev, /proc, /sys, /tmp

To make transition a bit smoother, the following things will be kept / migrated to Gentoo:
- bootloader, the kernel and its modules in /lib/modules
- file system layout and /etc/fstab
- the root password
- users and groups with IDs above 999
- everything in /usr/local
- the hostname
- DNS config from /etc/resolv.conf
- timezone

Preconditions:
- a modern x86_64 Linux in single-user mode
- about 8GB of free space on the / filesystem
- Internet connection

How basic is the installed Gentoo system?
A current Stage-3 system with current Portage repository is installed. So it is quite uptodate and can easily be brought to a fully patched level by running: 
```
emerge -uavD world
```
Where to continue the installation?
You can simply continue working through the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Choosing_the_right_profile) where Portage is configured and a profile is chosen.

What are the essential tasks?
- You should probably compile and install a new kernel
- Create a new initramfs (aka initrd) and install grub and its config
- Install syslogd and vixie-cron and all other software you might need
- Migrate anything you need from the old system which is found in /orig
- free up space by removing the old system

When something goes wrong
In the worst case the script will fail in a state where your system has (partially) been moved to /orig or /gentoo has partially been moved to the root filesystem. In that case the system is not functional. Boot from [System Rescue CD](https://www.system-rescue-cd.org), mount the root filesystem and move the folders back in place manually. Then reboot.
