# Study notes for the LPIC-1 certificate  

This file lists the detailed objectives of the LPIC-1 certificate, comprised of exam 101 and exam 102. The detailed objectives can be found at https://www.lpi.org/linux-certifications/programs/lpic-1/exam-101/ and https://www.lpi.org/linux-certifications/programs/lpic-1/exam-102/

### 101.1 Determine and configure hardware settings  

##### Enable and disable integrated peripherals.  
- peripheral: a device put and extracts information from a computer
- integrated peripherals: USB controllers, onboard LAN, floppy disk access controller, onboard serial port
- external peripherals: mouse, keyboard, microphone
- to disable: run `lspci -k` to get a list of hardware and associated kernel modules. Then disable using computer's firmware (BIOS/EFI) or with `rmmod`. If `rmmod` fails due to dependencies, put the module into `/etc/modprobe.d/blacklist.conf` and reboot

##### Configure systems with or without external peripherals such as keyboards.  
- PXE or BOOTP enable you to boot from network
- if booting without keyboard, disable halting on errors in BIOS or EFI

##### Differentiate between the various types of mass storage devices.  
- `/dev` directory lists devices as file system
- Examples: `/dev/sda` designates first hard disk, `/dev/sda1` designates first partition on first hard disk
- Hard disks listed in `/dev` do not necessarily start with *sd*: they can start with *hd*, *fd*, or even some other letter-combination
- a metadisk, aka RAID, can span multiple physical disks; the names start with *md*
- loop devices: used to mount filesystems not associated with block-devices
- `/dev/initrd` is the initial RAM disk loaded by the bootloader. Newer kernels list RAM disks as  `/dev/ram0`, `/dev/ram1`, ...
- `file --special-files /dev/sda` shows partitions and startheads and startsectors of the partitions
- `hdparm`: get/set SATA/IDE device parameters
- `hdparm -i /dev/sda` shows disk identification info which the kernel drivers (IDE, libata) have stored from boot/configuration time
- `hdparm -g /dev/sda` shows disk geometry
- `blkid`: locate/print block device attributes
- `blkid /dev/sda3` shows UUID of a disk partition

##### Set the correct hardware ID for different devices, especially the boot device.  
- each device has a major and a minor device number
- `ls -l /dev/sda` shows major and minor device number before date
- major device number identifies the driver Linux is using
- the minor number identifies device type
- disks can also have a UUID associated with it
- UUID: universally unique identifier, a 128 bit number
- `ls -l /dev/dis/by-uuid` shows disks and their UUIDs
- Normally you should not be required to change UUID, but if need be, use `tune2fs /dev/sda1 -U UUID`

##### Know the differences between coldplug and hotplug devices.  
- cold plug devices require the system to be powered off. E.g.: RAM, AGP, PCI, PATA
- hot plug devices can be plugged into a running system: USB, Firewire, some SATA and SCSI devices


##### Determine hardware resources for devices
- IRQ: an interrupt request. A signal sent to the processor that temporarily stops a running program and allows an interrupt handler to run instead
- IRQs are used by modems, NICs, keyboards, mice
- /proc/interrupts shows cumulative number of interrupts per CPU per device during current boot session 
- /proc/cpuinfo shows CPU info
- /proc/devices lists major numbers and device groups
- /proc/dma lists registered ISA DMA channels
- /proc/ioports shows I/O ports, that is unique locations in memory that are reserved for communication between CPU and a device
- /proc/modules shows modules loaded by the kernel
- /proc/partitions contains major and minor numbers of each partition as well as number of blocks and partition name
- `blockdev --getbsz /dev/sda5` to show block size of a partition


##### Tools and utilities to list various hardware information (e.g. lsusb, lspci, etc.)  
- `lsmod` to show the status of modules in the Linux Kernel 
- `lspci` to list all PCI devices
- `lsusb` to list USB devices
- `modprobe` to add and remove modules from the Linux Kernel


##### Tools and utilities to manipulate USB devices  
- `usbmgr` Daemon to load/unload USB modules
- `hotplug` is another tool to manage USB devices, and is built-in in kernels version 2.4 and higher
- hotplugged devices are detected automatically in newer kernels


##### Conceptual understanding of sysfs, udev, hald, dbus  
- udev is a virtual filesystem mounted at /dev. Creates dynamic device files as drivers are loaded
- D-Bus is a free and open-source inter-process communication (IPC) system, allowing multiple, concurrently-running computer programs (processes) to communicate with one another
- HALD (Hardware Abstraction Layer daemon) informs user-space programs about available hardware

##### /sys
- the sysfs virtual filesystem, mounted at `/sys`. Exports info about devices for use by user-space utilities

##### /dev
- contains files representing device access points
- devices include, terminal devices (tty), floppy (fd), hard disks (hd, sd), RAM (ram), cdrom (cd)

##### lsmod

##### lsusb

##### /proc
- contains info about system resources

##### modprobe

##### lspci
- lists PCI buses and devices connected to them
  
#### 101.2 Boot the system  

##### Provide common commands to the boot loader and options to the kernel at boot time
- the kernel is a set of routines, constantly in memory, that provide interface between user programs and hardware, schedule processes, manage virtual memory
- the bootloader is a program that runs before the operating system has loaded
- `/sbin/init` is the first program, a daemon, launched by kernel and stopped once the system shuts down. It loads services and other programs, and mounts partitions listed in `/etc/fstab` 
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

##### Demonstrate knowledge of the boot sequence from BIOS to boot completion
- the boot sequence:  
        - power on  
        - firmware (BIOS) is loaded from EEPROM  
        - Bootloader is loaded from boot device (from Master Boot Record if boot device is a hard disk)  
        - Bootloader examines the partition table and locates the boot partition  
        - from boot partition bootloader loads the kernel or loads secondary bootloader from partition's boot sector  
        - the OS is loaded  

##### Check boot events in the log files
- run `dmesg` to examine kernel ring buffer
- the kernel ring buffer contains messages from kernel produced during boot time
- ring buffer messages are also passed to `syslogd` once it is running
- `syslogd` logs the messages into `/var/log/messages`
- on Ubuntu, `rsyslogd` logs the messages into `/var/log/syslog`
- besides ring buffer messages, `syslogd` also contains messages from applications like mail, cron, and auth


# STOPPED HERE

/var/log/messages  
dmesg  
BIOS  
bootloader  
kernel  
init  
  
#### 101.3 Change runlevels and shutdown or reboot system  
Set the default runlevel.  
Change between run levels including single user mode.  
Shutdown and reboot from the command line.  
Alert users before switching runlevels or other major system event.  
Properly terminate processes.  
Knowledge of basic features of systemd and Upstart  
/etc/inittab  
shutdown  
init  
##### /etc/init.d  
A directory with System V scripts that are run at startup through symlinks in `/etc/rc\*.d` 

##### telinit  
  
### 102.1 Design hard disk layout  
Allocate filesystems and swap space to separate partitions or disks.  
Tailor the design to the intended use of the system.  
Ensure the /boot partition conforms to the hardware architecture requirements for booting.  
Knowledge of basic features of LVM  
/ (root) filesystem  
/var filesystem  
/home filesystem  
swap space  
mount points  
partitions  
  
### 102.2 Install a boot manager  
Providing alternative boot locations and backup boot options.  
Install and configure a boot loader such as GRUB Legacy.  
Perform basic configuration changes for GRUB 2.  
Interact with the boot loader  
/boot/grub/menu.lst  
grub-install  
MBR  
superblock  
  
### 102.3 Manage shared libraries  
Identify shared libraries.  
Identify the typical locations of system libraries.  
Load shared libraries.  
ldd  
ldconfig  
/etc/ld.so.conf  
LD_LIBRARY_PATH  
  
### 102.4 Use Debian package management  
##### Install, upgrade and uninstall Debian binary packages.  
Find packages containing specific files or libraries which may or may not be installed.  
Obtain package information like version, content, dependencies, package integrity and installation status (whether or not the package is installed).  
/etc/apt/sources.list  
dpkg  
dpkg-reconfigure  
apt-get  
apt-cache  
aptitude  
  
#### 102.5 Use RPM and YUM package management  
Install, re-install, upgrade and remove packages using RPM and YUM.  
Obtain information on RPM packages such as version, status, dependencies, integrity and signatures.  
Determine what files a package provides, as well as find which package a specific file comes from.  
rpm  
rpm2cpio  
/etc/yum.conf  
/etc/yum.repos.d/  
yum  
yumdownloader  
  
#### 103.1 Work on the command line  
Use single shell commands and one line command sequences to perform basic tasks on the command line.  
Use and modify the shell environment including defining, referencing and exporting environment variables.  
Use and edit command history.  
Invoke commands inside and outside the defined path.  
.  
bash  
echo  
env  
exec  
export  
pwd  
set  
unset  
man  
uname  
history  
  
#### 103.2 Process text streams using filters  
Send text files and output streams through text utility filters to modify the output using standard UNIX commands found in the GNU textutils package.  
cat  
cut  
expand  
fmt  
head  
od  
join  
nl  
paste  
pr  
sed  
sort  
split  
tail  
tr  
unexpand  
uniq  
wc  
  
#### 103.3 Perform basic file management  
Copy, move and remove files and directories individually.  
Copy multiple files and directories recursively.  
Remove files and directories recursively.  
Use simple and advanced wildcard specifications in commands.  
Using find to locate and act on files based on type, size, or time.  
Usage of tar, cpio and dd.  
cp  
find  
mkdir  
mv  
ls  
rm  
rmdir  
touch  
tar  
cpio  
dd  
file  
gzip  
gunzip  
bzip2  
file globbing  
  
#### 103.4 Use streams, pipes and redirects  
Redirecting standard input, standard output and standard error.  
Pipe the output of one command to the input of another command.  
Use the output of one command as arguments to another command.  
Send output to both stdout and a file.  
tee  
xargs  
  
#### 103.5 Create, monitor and kill processes  
Run jobs in the foreground and background.  
Signal a program to continue running after logout.  
Monitor active processes.  
Select and sort processes for display.  
Send signals to processes.  
&  
bg  
fg  
jobs  
kill  
nohup  
ps  
top  
free  
uptime  
killall  
  
#### 103.6 Modify process execution priorities  
Know the default priority of a job that is created.  
Run a program with higher or lower priority than the default..  
Change the priority of a running process.  
nice  
ps  
renice  
top  
  
#### 103.7 Search text files using regular expressions  
Create simple regular expressions containing several notational elements.  
Use regular expression tools to perform searches through a filesystem or file content.  
grep  
egrep  
fgrep  
sed  
regex(7)  
  
#### 103.8 Perform basic file editing operations using vi  
Navigate a document using vi.  
Use basic vi modes.  
Insert, edit, delete, copy and find text.  
vi  
/, ?  
h,j,k,l  
i, o, a  
c, d, p, y, dd, yy  
ZZ, :w!, :q!, :e!  
  
#### 104.1 Create partitions and filesystems  
Use various mkfs commands to set up partitions and create various filesystems such as: ext2/ext3/ext4, xfs, reiserfs v3, vfat  
fdisk  
mkfs  
mkswap  
  
#### 104.2 Maintain the integrity of filesystems  
Verify the integrity of filesystems.  
Monitor free space and inodes.  
Repair simple filesystem problems.  
du  
df  
fsck  
e2fsck  
mke2fs  
debugfs  
dumpe2fs  
tune2fs  
xfs tools  
  
#### 104.3 Control mounting and unmounting of filesystems  
Manually mount and unmount filesystems.  
Configure filesystem mounting on bootup.  
Configure user mountable removeable filesystems.  
/etc/fstab  
/media  
mount  
umount  
  
#### 104.4 Manage disk quotas  
Set up a disk quota for a filesystem.  
Edit, check and generate user quota reports.  
quota  
edquota  
repquota  
quotaon  
  
#### 104.5 Manage file permissions and ownership  
Manage access permissions on regular and special files as well as directories.  
Use access modes such as suid, sgid and the sticky bit to maintain security.  
Know how to change the file creation mask.  
Use the group field to grant file access to group members.  
chmod  
umask  
chown  
chgrp  
  
#### 104.6 Create and change hard and symbolic links  
Create links.  
Identify hard and/or softlinks.  
Copying versus linking files.  
Use links to support system administration tasks.  
ln  
  
#### 104.7 Find system files and place files in the correct location  
Understand the correct locations of files under the FHS.  
Find files and commands on a Linux system.  
Know the location and purpose of important file and directories as defined in the FHS.  
find  
locate  
updatedb  
whereis  
which  
type  
/etc/updatedb.conf  
  
