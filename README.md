lpic-1-study-notes  
==================  
  
Study notes for the LPIC-1 certificate  
  
#### 101.1 Determine and configure hardware settings  
  
Enable and disable integrated peripherals.  
Configure systems with or without external peripherals such as keyboards.  
Differentiate between the various types of mass storage devices.  
Set the correct hardware ID for different devices, especially the boot device.  
Know the differences between coldplug and hotplug devices.  
Determine hardware resources for devices.  
Tools and utilities to list various hardware information (e.g. lsusb, lspci, etc.)  
Tools and utilities to manipulate USB devices  
Conceptual understanding of sysfs, udev, hald, dbus  
/sys  
/proc  
/dev  
modprobe  
lsmod  
lspci  
lsusb  
  
#### 101.2 Boot the system  
Provide common commands to the boot loader and options to the kernel at boot time.  
Demonstrate knowledge of the boot sequence from BIOS to boot completion.  
Check boot events in the log files.  
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
/etc/init.d  
telinit  
  
#### 102.1 Design hard disk layout  
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
  
#### 102.2 Install a boot manager  
Providing alternative boot locations and backup boot options.  
Install and configure a boot loader such as GRUB Legacy.  
Perform basic configuration changes for GRUB 2.  
Interact with the boot loader  
/boot/grub/menu.lst  
grub-install  
MBR  
superblock  
  
#### 102.3 Manage shared libraries  
Identify shared libraries.  
Identify the typical locations of system libraries.  
Load shared libraries.  
ldd  
ldconfig  
/etc/ld.so.conf  
LD_LIBRARY_PATH  
  
#### 102.4 Use Debian package management  
Install, upgrade and uninstall Debian binary packages.  
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
  
