# Exam 101

* [test](#1013-change-runlevels--boot-targets-and-shutdown-or-reboot-system-weight-3)

This file lists the detailed objectives of the LPIC-1 certificate, comprised of exam 101 and exam 102. The detailed objectives can be found at https://www.lpi.org/linux-certifications/programs/lpic-1/exam-101/ and https://www.lpi.org/linux-certifications/programs/lpic-1/exam-102/

## 101.1 Determine and configure hardware settings (weight: 2) 
### Tools and utilities to list various hardware information (e.g. lsusb, lspci, etc.)
### Tools and utilities to manipulate USB devices
### Conceptual understanding of sysfs, udev, dbus
- sysfs virtual filesystem, mounted at `/sys`. Exports info about devices for use by user-space utilities
- udev is a virtual filesystem mounted at /dev. Creates dynamic device files as drivers are loaded
- D-Bus is a free and open-source inter-process communication (IPC) system, allowing multiple, concurrently-running computer programs (processes) to communicate with one another
### /sys/
### /proc/
- /proc/interrupts shows cumulative number of interrupts per CPU per device during current boot session 
- /proc/cpuinfo shows CPU info
- /proc/devices lists major numbers and device groups
- /proc/dma lists registered ISA DMA channels
- /proc/ioports shows I/O ports, that is unique locations in memory that are reserved for communication between CPU and a device
- /proc/modules shows modules loaded by the kernel
- /proc/partitions contains major and minor numbers of each partition as well as number of blocks and partition name
### /dev/
- `/dev` directory lists devices as file system
- contains files representing device access points
- devices include, terminal devices (tty), floppy (fd), hard disks (hd, sd), RAM (ram), cdrom (cd)
- Examples: `/dev/sda` designates first hard disk, `/dev/sda1` designates first partition on first hard disk
- Hard disks listed in `/dev` do not necessarily start with *sd*: they can start with *hd*, *fd*, or even some other letter-combination
- `/dev/initrd` is the initial RAM disk loaded by the bootloader. Newer kernels list RAM disks as  `/dev/ram0`, `/dev/ram1`, ...
- `file --special-files /dev/sda` shows partitions and startheads and startsectors of the partitions
### modprobe
- `modprobe` to add and remove modules from the Linux Kernel
### lsmod
- `lsmod` to show the status of modules in the Linux Kernel 
### lspci
- `lspci` to list all PCI devices
- to disable: run `lspci -k` to get a list of hardware and associated kernel modules. Then disable using computer's firmware (BIOS/EFI) or with `rmmod`. If `rmmod` fails due to dependencies, put the module into `/etc/modprobe.d/blacklist.conf` and reboot
### lsusb
- `lsusb` to list USB devices

## 101.2 Boot the system (weight: 3)
### Provide common commands to the boot loader and options to the kernel at boot time
- kernel is a set of routines, constantly in memory. Provides interface between user programs and hardware, schedules processes, manages virtual memory
- boot loader runs before OS has loaded
### Demonstrate knowledge of the boot sequence from BIOS to boot completion
- the boot sequence:  
        - power on  
        - firmware (BIOS) is loaded from EEPROM  
        - Bootloader is loaded from boot device (from Master Boot Record if boot device is a hard disk)  
        - Bootloader examines the partition table and locates the boot partition  
        - from boot partition bootloader loads the kernel or loads secondary bootloader from partition's boot sector  
        - the OS is loaded  
### Understanding of SysVinit and systemd
### Awareness of Upstart
### Check boot events in the log files
- run `dmesg` to examine kernel ring buffer
- the kernel ring buffer contains messages from kernel produced during boot time
- ring buffer messages are also passed to `syslogd` once it is running
- `syslogd` logs the messages into `/var/log/messages`
- on Ubuntu, `rsyslogd` logs the messages into `/var/log/syslog`
- besides ring buffer messages, `syslogd` also contains messages from applications like mail, cron, and auth
### dmesg
### BIOS
### bootloader
### kernel
### initramfs
### init
### SysVinit
### systemd
- `/sbin/init`: first program, a daemon, launched by kernel and stopped once the system shuts down. Loads services and other programs, and mounts partitions listed in `/etc/fstab` 
- the PID of `init` is 1 and PPID is 0  
- there are several `init` vendors: System V (or SysV), upstart, systemd  
- The System V init daemon is configured through /etc/inittab  
- `grub-install /dev/sda` will install the GRUB (GRand Unified Bootloader) legacy into the first sector (called MBR, Master Boot Record) of the first disk  
- you can also install GRUB legacy into the boot sector of a partition rather than into MBR
- `efibootmgr` to install Fedora's EFI enabled version of GRUB legacy
- GRUB 2: edit /etc/default/grub or the files in /etc/grub.d, then run `update-grub` or `grub-mkconfig > /boot/grub/grub.cfg`
- GRUB legacy options in /boot/grub/grub.conf  
        - `default = 1` tells to load OS under menu option #1 by default  
        - `timeout = 15` tells GRUB to wait 15 seconds to allow user to make OS selection at boot time   
        - `splashimage = splashimage (hd0,1)/grub/images/usplash.xpm.gz` us a splash image  
        - `title Fedora 3.4.1` the title of the OS shown in selection meny during boot  
        - `root (hd0,0)` specifies the GRUB root disk and partition  
        - `kernel /vmlinuz-2.4.9-21 ro root=/dev/hda6` loads kernel located at <GRUB-root>/vmlinuz-2.4.9-21 where <GRUB-root> was specified with the option `root`. The parameter `ro` loads the root device hda6 as read only during the boot. The root device hda6 is where Linux will mount its root once booted  
        - `initrd /initrd-3.4.1` GRUB-root relative path to the initial RAM disk that contains minimal set of drivers to boot the OS  
        - `rootnoverify` configures the root partition for GRUB, just like the root command, but does not mount the partition. This is useful for when an OS is outside of the area of the disk that GRUB can read, but setting the correct root device is still desired. Typically used to boot Windows  
        - `chainloader +1` tells GRUB to load the bootloader one sector from the start of the root partition (which should have been specified with the root option). Often used to load Windows bootloader  

## 101.3 Change runlevels / boot targets and shutdown or reboot system (weight: 3)
### Set the default runlevel or boot target
### Change between runlevels / boot targets including single user mode
### Shutdown and reboot from the command line
### Alert users before switching runlevels / boot targets or other major system events
### Properly terminate processes
### /etc/inittab
- read by sysV-compatible `init` daemon
- tells init when to start some process
- defines default runlevel
- defines what each configured runlevel does
- each line has pattern: id:runlevels:action:process
- Sample file:
```
# Level to run in
id:2:initdefault:

# System initialization before anything else.
si::sysinit:/etc/rc.d/bcheckrc

# Runlevel 0,6 is halt and reboot, 1 is maintenance mode.
l0:0:wait:/etc/rc.d/rc.halt
l1:1:wait:/etc/rc.d/rc.single
l2:2345:wait:/etc/rc.d/rc.multi
l6:6:wait:/etc/rc.d/rc.reboot

# What to do at the "3 finger salute".
ca::ctrlaltdel:/sbin/shutdown -t5 -rf now

# Runlevel 2&3: getty on console, level 3 also getty on modem port.
1:23:respawn:/sbin/getty tty1 VC linux
2:23:respawn:/sbin/getty tty2 VC linux
3:23:respawn:/sbin/getty tty3 VC linux
4:23:respawn:/sbin/getty tty4 VC linux
S2:3:respawn:/sbin/uugetty ttyS2 M19200
```

### shutdown
### init
### /etc/init.d/
A directory with System V scripts that are run at startup through symlinks in `/etc/rc\*.d` 
### telinit
### systemd
### systemctl
### /etc/systemd/
### /usr/lib/systemd/
### wall

## 102.1 Design hard disk layout (weight: 2)
### Allocate filesystems and swap space to separate partitions or disks
### Tailor the design to the intended use of the system
### Ensure the /boot partition conforms to the hardware architecture requirements for booting
### Knowledge of basic features of LVM
### / (root) filesystem
### /var filesystem
### /home filesystem
### /boot filesystem
### swap space
### mount points
### partitions 

## 102.2 Install a boot manager (weight: 2)
### Providing alternative boot locations and backup boot options
### Install and configure a boot loader such as GRUB Legacy
### Perform basic configuration changes for GRUB 2
### Interact with the boot loader
### menu.lst, grub.cfg and grub.conf
### grub-install
### grub-mkconfig
### MBR 

## 102.3 Manage shared libraries (weight: 1)
### Identify shared libraries
### Identify the typical locations of system libraries
### Load shared libraries
### ldd
### ldconfig
### /etc/ld.so.conf
### LD_LIBRARY_PATH
  
## 102.4 Use Debian package management (weight: 3)
### Install, upgrade and uninstall Debian binary packages
### Find packages containing specific files or libraries which may or may not be installed
### Obtain package information like version, content, dependencies, package integrity and installation status (whether or not the package is installed)
### /etc/apt/sources.list
### dpkg
### dpkg-reconfigure
### apt-get
### apt-cache

## 102.5 Use RPM and YUM package management (weight: 3)
### Install, re-install, upgrade and remove packages using RPM and YUM
### Obtain information on RPM packages such as version, status, dependencies, integrity and signatures
### Determine what files a package provides, as well as find which package a specific file comes from
### rpm
### rpm2cpio
### /etc/yum.conf
### /etc/yum.repos.d/
### yum
### yumdownloader 

## 103.1 Work on the command line (weight: 4)
### Use single shell commands and one line command sequences to perform basic tasks on the command line
### Use and modify the shell environment including defining, referencing and exporting environment variables
### Use and edit command history
### Invoke commands inside and outside the defined path
### bash
### echo
### env
### export
### pwd
### set
### unset
### man
### uname
### history
### .bash_history

 
## 103.2 Process text streams using filters (weight: 3)
### Send text files and output streams through text utility filters to modify the output using standard UNIX commands found in the GNU textutils package
### cat
### cut
### expand
### fmt
### head
### join
### less
### nl
### od
### paste
### pr
### sed
### sort
### split
### tail
### tr
### unexpand
### uniq
### wc

 
## 103.3 Perform basic file management (weight: 4)
### Copy, move and remove files and directories individually
### Copy multiple files and directories recursively
### Remove files and directories recursively
### Use simple and advanced wildcard specifications in commands
### Using find to locate and act on files based on type, size, or time
### Usage of tar, cpio and dd
### cp
### find
### mkdir
### mv
### ls
### rm
### rmdir
### touch
### tar
### cpio
### dd
### file
### gzip
### gunzip
### bzip2
### xz
### file globbing

## 103.4 Use streams, pipes and redirects (weight: 4)
### Redirecting standard input, standard output and standard error
### Pipe the output of one command to the input of another command
### Use the output of one command as arguments to another command
### Send output to both stdout and a file
### tee
### xargs
 
## 103.5 Create, monitor and kill processes (weight: 4)
### Run jobs in the foreground and background
### Signal a program to continue running after logout
### Monitor active processes
### Select and sort processes for display
### Send signals to processes
### &
### bg
### fg
### jobs
### kill
### nohup
### ps
### top
### free
### uptime
### pgrep
### pkill
### killall
### screen

## 103.6 Modify process execution priorities (weight: 2)
### Know the default priority of a job that is created
### Run a program with higher or lower priority than the default
### Change the priority of a running process
### Terms and Utilities:
### nice
### ps
### renice
### top 

## 103.7 Search text files using regular expressions (weight: 2)
### Create simple regular expressions containing several notational elements.  
### Use regular expression tools to perform searches through a filesystem or file content.  
### grep  
### egrep  
### fgrep  
### sed  
### regex(7)  
  
## 103.8 Perform basic file editing operations using vi (weight: 3)
### Navigate a document using vi  
### Use basic vi modes  
### Insert, edit, delete, copy and find text.  
### vi  
### /, ?  
### h,j,k,l  
### i, o, a  
### c, d, p, y, dd, yy  
### ZZ, :w!, :q!, :e!  
  
## 104.1 Create partitions and filesystems (weight: 2)
### Manage MBR partition tables
### Use various mkfs commands to create various filesystems such as:
### ext2/ext3/ext4
### XFS
### VFAT
### Awareness of ReiserFS and Btrfs
### Basic knowledge of gdisk and parted with GPT
### fdisk
### gdisk
### parted
### mkfs
### mkswap

 
## 104.2 Maintain the integrity of filesystems (weight: 2)
### Verify the integrity of filesystems
### Monitor free space and inodes
### Repair simple filesystem problems
### du
### df
### fsck
### e2fsck
### mke2fs
### debugfs
### dumpe2fs
### tune2fs
### XFS tools (such as xfs_metadump and xfs_info) 

## 104.3 Control mounting and unmounting of filesystems (weight: 3)
### Manually mount and unmount filesystems.  
### Configure filesystem mounting on bootup.  
### Configure user mountable removeable filesystems.  
### /etc/fstab  
### /media/
### mount  
### umount  
  
## 104.4 Manage disk quotas (weight: 1)
### Set up a disk quota for a filesystem
### Edit, check and generate user quota reports
### quota  
### edquota  
### repquota  
### quotaon  
  
## 104.5 Manage file permissions and ownership (weight: 3)
### Manage access permissions on regular and special files as well as directories
### Use access modes such as suid, sgid and the sticky bit to maintain security 
### Know how to change the file creation mask
### Use the group field to grant file access to group members
### chmod  
### umask  
### chown  
### chgrp  
  
## 104.6 Create and change hard and symbolic links (weight: 2)
### Create links
### Identify hard and/or soft links
### Copying versus linking files
### Use links to support system administration tasks
### ln
### ls 

## 104.7 Find system files and place files in the correct location (weight: 2)
### Understand the correct locations of files under the FHS
### Find files and commands on a Linux system
### Know the location and purpose of important file and directories as defined in the FHS
### find  
### locate  
### updatedb  
### whereis  
### which  
### type  
### /etc/updatedb.conf  
