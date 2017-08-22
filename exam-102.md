# Exam 102 

* [105.1 Customize and use the shell environment (weight: 4)](#1051-customize-and-use-the-shell-environment-weight-4)
* [105.2 Customize or write simple scripts (weight: 4)](#1052-customize-or-write-simple-scripts-weight-4)
* [105.3 SQL data management(weight 2)](#1053-sql-data-managementweight-2)
* [106.1 Install and configure X11 (weight: 2)](#1061-install-and-configure-x11-weight-2)
* [106.2 Setup a display manager (weight: 1)](#1062-setup-a-display-manager-weight-1)
* [106.3 Accessibility (weight: 1)](#1063-accessibility-weight-1)
* [107.1 Manage user and group accounts and related system files (weight: 5)](#1071-manage-user-and-group-accounts-and-related-system-files-weight-5)
* [107.2 Automate system administration tasks by scheduling jobs (weight: 4)](#1072-automate-system-administration-tasks-by-scheduling-jobs-weight-4)
* [107.3 Localisation and internationalisation (weight: 3)](#1073-localisation-and-internationalisation-weight-3)
* [108.1 Maintain system time (weight: 3)](#1081-maintain-system-time-weight-3)
* [108.2 System logging (weight: 3)](#1082-system-logging-weight-3)
* [108.3 Mail Transfer Agent (MTA) basics (weight: 3)](#1083-mail-transfer-agent-mta-basics-weight-3)
* [108.4 Manage printers and printing (weight: 2)](#1084-manage-printers-and-printing-weight-2)
* [109.1 Fundamentals of internet protocols (weight 4)](#1091-fundamentals-of-internet-protocols-weight-4)
* [109.2 Basic network configuration (weight 4)](#1092-basic-network-configuration-weight-4)
* [109.3 Basic network troubleshooting](#1093-basic-network-troubleshooting)
* [109.4 Configure client side DNS (weight: 2)](#1094-configure-client-side-dns-weight-2)
* [110.1 Perform security administration tasks (weight: 3)](#1101-perform-security-administration-tasks-weight-3)
* [110.2 Setup host security (weight: 3)](#1102-setup-host-security-weight-3)
* [110.3 Securing data with encryption](#1103-securing-data-with-encryption)


## 105.1 Customize and use the shell environment (weight: 4)
### Set environment variables (e.g. PATH) at login or when spawning a new shell
### Write Bash functions for frequently used sequences of commands
### Maintain skeleton directories for new user accounts
### Set command search path with the proper directory
### .
### source
### /etc/bash.bashrc
### /etc/profile
### env
### export
### set
### unset
### ~/.bash_profile
### ~/.bash_login
### ~/.profile
### ~/.bashrc
### ~/.bash_logout
### function
### alias
### lists

## 105.2 Customize or write simple scripts (weight: 4)
### Use standard sh syntax (loops, tests)
### Use command substitution
### Test return values for success or failure or other information provided by a command
### Perform conditional mailing to the superuser
### Correctly select the script interpreter through the shebang (#!) line
### Manage the location, ownership, execution and suid-rights of scripts
### for
### while
### test
### if
### read
### seq
### exec

## 105.3 SQL data management(weight 2)
### Use of basic SQL commands
### Perform basic data manipulation
### insert
### update
### select
### delete
### from
### where
### group by
### order by
### join

## 106.1 Install and configure X11 (weight: 2)
### Verify that the video card and monitor are supported by an X server
- run `X -version`
- Consult docs of X for supported chipsets (drivers are written for chipsets, not video cards)
- If supported, running `XFree86 -configure` or `Xorg -configure` should result in file `/root/XF86Config.new` or `/root/xorg.conf.new`
- Check for video card drivers in `/usr/X11R6/lib/modules/drivers` or `/usr/lib/xorg/modules/drivers`
    - e.g. `nv_drv.o` corresponds to an NVidia driver
### Awareness of the X font server
- font server is convenient when many computers on a network need to have same fonts
- not that common anymore
- Font server must be in X config:
```
Section "Files"
    FontPath    "unix:/7100"                            <- local font server
    FontPath    "tcp/fontserver.mydomain.com:7100"      <- remote font server
EndSection
```
- to add fonts to a font server:
    - install the font on the system
    - modify `/etc/X11/fs/config` and add the new font path to the `catalogue` variable
    - restart font sever: `/etc/init.d/xfs restart`
    - run `xset fp rehash` to refresh the fonts on the client

### Basic understanding and knowledge of the X Window configuration file
- X.org-X11 config file: `/etc/X11/xorg.conf`
- XFree86 4.x: `/etc/X11/XF86Config` or `/etc/X11/XF86Config-4`
    - earlier versions not covered by the exam
- config file is broken down in sections
    - `InputDevice` section for mice and keyboards
    - `Module` section for loading of drivers for specific features or hardware
    - `Monitor` section is for monitor; `Modeline` entry is tricky and dangerous!
    - `Device` section is for video card. `Driver` entry sets the driver
    - `Screen` section tells X how to combine video card and monitor
    - `ServerLayout` section tells X how to link all the components
- Example X config file:

```
Section "InputDevice"
    Identifier  "Keyboard0"
    Driver      "kbd"
    Option      "XkbModel"  "pc105"
    Option      "XkbLayout" "us"
    Option      "AutoRepeat"    "500 200"
EndSection

Section "InputDevice"
    Identifier  "Mouse0"
    Driver      "mouse"
    Option      "Protocol"  "IMPS/2"
    Option      "Device" "/dev/input/mice"
    Option      "Emulate3Buttons"   "no"
    Option      "ZAxisMapping"  "4 5"
EndSection

Section "Module"
    Load    "dbe"
    Load    "extmod"
    Load    "fbdevhw"
    Load    "glx"
    Load    "record"
    Load    "freetype"
    Load    "type1"
    Load    "dri"
EndSection

Section "Monitor"
    Identifier" "Monitor0"
    ModelName   "Dell 30UXWB"
    HorizSync   30.0 - 83.0
    VertRefresh 55.0 - 75.0
    Modeline "1920x1080" 138.50 1920 1968 2000 2080     1080 1083 1088 1111 
EndSection

Section "Device"
    Identifier  "Videocard0"
    Driver      "nv"
    VendorName  "nVidia"
    BoardName   "GeForce 1080X"
    VideoRam    "260000"
EndSection

Section "Screen"
    Identifier  "Screen0"
    Device      "Videocard0"
    Monitor     "Monitor0"
    DefaultDepth    24
    SubSection "Display"
        Depth   24
        Modes   "1920x1080" "1280x1024" "1024x768"
    EndSubSection
    SubSection "Display"
        Depth   8
        Modes   "1024x768" "800x600" "640x480"
    EndSubSection
EndSection

Section "ServerLayout"
    Identifier  "single monitor setup"
    Screen      "Screen0" 0 0
    InputDevice "Mouse0" "CorePointer"
    InputDevice "Keyboard0" "CoreKeyboard"
EndSection
```

### /etc/X11/xorg.conf
- config file of X.org-X11

### xhost
- command to allow X to be accessed remotely
- `xhost +somehost` will allow somehost to use current host's X server
- `xhost -somehost` will disallow somehost to use current host's X server

### DISPLAY
- This environment variable indicates to graphical applications where to display the actual graphical user interface
- The value consists of 3 parts: A host-name followed by a colon (:), a display number followed by a dot (.) and a screen number.
- running on somemachine the command `export DISPLAY=myhomemachine:0.0` will display
 graphical applications on myhomemachine that are running on somemachine

### xwininfo
- use this tool to obtain technical info about specific window

### xdpyinfo
- this tool tells you what your display is capable of

### X
- GUI system for Linux
- aka X Window System
- implementations: 
    - XFree86: popularity faded
    - X.org-X11: based on XFree86 and is most popular one
    - Accelerated-X: commercial one, with some expanded support for exotic hardware
- Create initial config with `Xorg -configure`
- Remote X sessions possible, preferably through SSH tunnel for encryption as X does not have it


## 106.2 Setup a display manager (weight: 1)

### Basic configuration of LightDM
- config file: `/etc/lightdm/lightdm.conf`
- `allow-guest=false` to disable guest logins
- `greeter-hide-users=true` to hide user list
- `greeter-show-manual-login=true` to allow manual login
- `autologin-user=username` with which username to autologin
- `autologin-user-timeout=delay` after how many seconds of inactivity after starting to autologin
- `sudo start lightdm` and `sudo stop lightdm`
- `sudo dpkg-reconfigure lightdm`

### Turn the display manager on or off
- Turn on and off by changing runlevels: `telinit 5`
- `systemctl disable gdm` in Debian 8
- Turn LightDM on/off: `sudo start lightdm` and `sudo stop lightdm`

### Change the display manager greeting
- in the config file look for option containing word `greeter`

### Awareness of XDM, KDM and GDM
- All of them are XDMCP (X Display Manager Control Protocol) servers, aka display managers
- KDM is from KDE project
- GDM is from GNOME project
- XDM is oldest at least feature-heavy display manager

### lightdm
- a daemon
- cross-desktop display manager
- runs display servers (e.g. X)
- runs greeters
- provides remote graphical login options

### /etc/lightdm/
- directory containing config for LightDM

## 106.3 Accessibility (weight: 1)

### Basic knowledge of keyboard accessibility settings (AccessX)
- AccessX seems outdated and not maintained
    - display managers typically have AccessX covered
- various options like sticky keys, repeat keys, onscreen keyboard

### Basic knowledge of visual settings and themes
- onscreen keyboard
- default fonts
- maginifier tools
- high/low contrast themes

### Basic knowledge of assistive technology (ATs)
- Linux can "speak", e.g. by reading you the text in terminal
- you can hookup Linux to a braille display
- you can adjust various visual settings, e.g. high/low contrast themes

### Sticky/Repeat Keys
- sticky keys: keep Ctrl, Shift, Alt keys pressed when pressed and released
- keyboard repeat rate: set the delay and repeat rate tailored to a user

### Slow/Bounce/Toggle Keys
- bounce keys: multiple presses count as one
- slow keys: register key strokes only after long enough press and hold

### Mouse Keys
- mouse keys: use cursor pad as mouse keys

### High Contrast/Large Print Desktop Themes
- usually located in system settings of a display manager

### Screen Reader
- program that reads the text on the screen

### Braille Display
- Translate text on screen into braille and show it in a braille display (a tacit display)

### Screen Magnifier
- make screen bigger around mouse cursor

### On-Screen Keyboard
- show a keyboard on the screen and use mouse to click on its keys

### Gestures (used at login, for example GDM)
- a mouse gesture allows you to make a clicks or start some application using mouse movents only
- e.g. dwell gesture clicks on an icon if you point mouse cursor on it long enough
- mouse gestures: move specific mouse movements to some actions (e.g. close a window)

### Orca
- Screenreader program

### GOK
- GNOME onscreen keyboard

### emacspeak
- Screenreader program

## 107.1 Manage user and group accounts and related system files (weight: 5)

### Add, modify and remove users and groups
- keep usernames 8 characters or less
- use only alphanumeric characters for usernames
- to run programs or created files with non-primary group use `newgrp`
    - e.g. `newgrp project2`
- add users using `useradd`
- change password using `passwd` utility
- modify accounts using `usermod` utility
- delete accounts using `userdel` utility
- add groups using `groupadd` utility
- modify groups using `groupmod` utility
- delete group using `groupdel` utility

### Manage user/group info in password/group databases
- password database is in `/etc/passwd`
- group database is in `/etc/group`
- primary group of a user is in `/etc/passwd`
- each username is mapped to a UID (User ID) in `/etc/passwd`
    - it is possible to have multiple usernames map to same UID
- each group is mapped to a GID (Group ID) in `/etc/group`
- Linux distributions reserve first 100 UIDs and GIDs for system use
    - root has UID 0
    - exact limits (`UID_MIN`, `UID_MAX`) are in `/etc/login.defs`

### Create and manage special purpose and limited accounts
- system accounts are created using `useradd -r systemacc1`
- create account without login by settings its shell to `/bin/false`

### /etc/passwd
- file that contains configuration of the accounts
- sample line (colon delimited): `john:x:543:200:John Smith:/home/john:/bin/bash`
    - this line means the username is `john` with UID `543` and primary GID `200`, with user comment being `John Smith` and home dir being `/home/john` and shell being `/bin/bash`
    - `x` indicates that shadow password is in use (`/etc/shadow`)
    - `543` is the UID
    - `200` is the primary GID

### /etc/shadow
- this file, present in a system that uses shadow passwords, contains encrypted passwords
- typical line (colon delimited): `john:FDFSG#$Tge34ge5:3344:0:-1:7:-1:-1`
    - most of the fields correspond to option set with `chage` utility
    - second field is the encrypted password
        - asterisk or exclamation mark indicate locked account
    - `3344` indicates days since last password change

### /etc/group
- controls group membership of users
- typical, colon delimited line: `project1:x:501:john,peter,alice`
    - indicates that group with name project1 has a shadow password (`/etc/gshadow`), has GID 501, and has memebers John, Peter, and Alice

### /etc/skel/
- directory containing files that will be copied to user's home directory upon user creation

### chage
- utility to modify settings related to account expiration

### getent
- displays entries from databases supported by the Name Service Switch libraries, which are configured in `/etc/nsswitch.conf`

### groupadd
- utility to add groups
- `groupadd [-g GID [-o]] [-r] [-f] groupname`
    - `-r` creates system group

### groupdel
- utility to delete groups
- by default, group files (`find / -gid 501`) are not deleted

### groupmod
- utility to modify groups
- `groupmod [-g GID [-o]] [-n newgroupname] currentgroupname`

### passwd
- utility to change password
- root user can change password for all users
- normal users can change only their own password
- the `-l` option locks the account by prefixing the encrypted password with exclamation mark
- the `-u` option unlocks an account
- the `-d` option removes password and makes account passwordless

### useradd
- `useradd [-d homedir] [-e expire-date] [-g default-group] [-G group[,...]] [-p pass] [-s shell] [-u UID [-o]] [-r] [-m [-k skeleton-dir]] username`
- set the shell to `/bin/false` or `/sbin/shutdown` to disallow shell use
- the `-o` option allows using existing UID
- the `-r` opion creates a system account (UID lower than `UID_MIN` as defined in `/etc/login.defs`) without homedir
- the `-m` option creates home directory based on `/etc/skel` which can be overridden with `-k` option

### userdel
- utility to delete user accounts
- `userdel -r john` to delete John's account and his files

### usermod
- utility to modify user accounts
- `-d` option changes home directory
- `-l` option changes username
- `-L` and `-U` lock and unlock the account


## 107.2 Automate system administration tasks by scheduling jobs (weight: 4)

### Manage cron and at jobs
- `cron` is a daemon that runs jobs
    - "wakes up" once a minute and runs commands 
    - a command is run only if its specified time matches current time
    - the commands are in `/var/spool/cron` file, `/etc/cron.d` directories, and `/etc/crontab` file
    - there are two types of cron jobs: system cron jobs and user cron jobs
- user cron jobs are managed with `crontab` utility
- system cron jobs are managed by editing `/etc/crontab` directly
- to run `at` jobs, `atd` daemon must be running

### Configure user access to cron and at services
- access to cron is managed through `/etc/cron.allow` and `/etc/cron.deny` files
    - if `/etc/cron.allow` is not present then everyone is allowed to run cron jobs
- access to `at` jobs is done through files `/etc/at.allow` and `/etc/at.deny`
    - if neither of these files exist, then only `root` is allowed to run `at`

### Configure anacron
- configured through file `/etc/anacrontab`
- example `/etc/anacrontab` file:
```
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr:bin
# format: period delay job-identifier command
1   5   cron.daily  run-parts /etc/cron.daily
7   10  cron.weekly run-parts /etc/cron.weekly
```
- intervals are in days

### /etc/cron.{d,daily,hourly,monthly,weekly}/
- these directories contain programs to be run by `cron` daemon as system cron jobs
- the {daily,hourly,monthly,weekly} directories suggest the frequency with which the programs should be run

### /etc/at.deny
- this file contains users that should be denied the use of `at` utility

### /etc/at.allow
- this file contains users that should be allowed the use of `at` utility

### /etc/crontab
- controls system jobs
- example:
```
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
```
- first five fields are minute, hour, day-of-month, month, day-of-week
    - multiple values can be specified as
        - asterisk which matches all possible values
        - list separated by commas (e.g. 1,10,59)
        - two values separated by a dash, representing a range, e.g. 9-17
        - a slash with some other multi-value option specifies stepped values, e.g. `*/5` in minute field to run a job every 5 minutes
- sixth field is the user that will be used to run the specified command
- seventh field is the command
- typically, `run-parts` or `cronloop` are used to run any executable script within a directory

### /etc/cron.allow
- file specifying who is allowed to have cron jobs

### /etc/cron.deny
- file specifying who is not allowed to have cron jobs

### /var/spool/cron/
- contains crontab files of users

### crontab
- utility to manage user cron jobs
- syntax: `crontab [-u user] [-l | -e | -r] [file]`
- edit user cron jobs with `crontab -e` 
- format of user crontab is same as `/etc/crontab`, except no user name is needed

### at
- utility that allows running some command once at some point in future
- syntax: 
    - `at [-V] [-q queue] [-f file] [-mMlv] timespec...`
    - `at [-V] [-q queue] [-f file] [-mMkv] [-t time]`

### atq
- query pending `at` jobs

### atrm
- remove pending `at` job

### anacron
- utility to run jobs after some interval of days passes
- useful for workstations that often are switched on and off
- not so useful on servers
- `anacron` must be called itself
    - via startup script
    - via cronjob

### /etc/anacrontab
- configuration file on `anacron`


## 107.3 Localisation and internationalisation (weight: 3)
### Configure locale settings and environment variables
- query current locale with `locale`
- query available locales with `locale -a`
- `LC_ALL` variable overrides all other `LC_*` variables
- change locale temporarily: `export LC_ALL=en_GB.UTF-8`
- change locale permanently by setting `LC_*` variables in `.bashrc` or `/etc/profile`
- `LANG=C` output is not passed through locale translations
    - helpful for debugging
    - avoids some problems when binary data is passed through pipelines

### Configure timezone settings and environment variables
- file or symlink `/etc/localtime` tells Linux the timezone
- `/usr/share/zonefiles` contains timezone files that you can symlink from `/etc/localtime`

### /etc/timezone
- a file with timezone data in textual format
    - used by Debian and derivatives
    - Fedora uses `/etc/sysconfig/clock`

### /etc/localtime
- file or symlink to a file containing current timezone info

### /usr/share/zoneinfo/
- directory with timezone files

### LC\_\*
- various environment variables that control the locale settings, like paper size, currency, etc.

### LC\_ALL
- a locale environment variable to override all other environment variables

### LANG
- a locale environment variable

### TZ
- environment variable that hold timezone info (3 possible formats)
- can be useful in the event of connecting to server in different timezone
    - set it with `export TZ=:/usr/share/zoneinfo/Europe/London`

### /usr/bin/locale
- program to query and set locale

### tzselect
- a program that helps make timezone changes

### timedatectl
- program (part of systemd) to set system clock

### date
- shows current date in various formats

### iconv
- program to convert file from one encoding to another

### UTF-8
- a language codeset
- 8-bit Unicode Transformation Format

### ISO-8859
- an early attempt to extend ASCII to handle language other than English

### ASCII
- old and primitive method of encoding language into 7 bits 
- dsigned to handle English character set only

### Unicode
- character set designed to support as many languages as possible

## 108.1 Maintain system time (weight: 3)

### Set the system date and time
- x86 and x86-64 systems running Linux have two clocks: hardware clock and software clock
- hardware clock maintains time while the system is swtiched off
- when system is booted, Linux uses hardware clock to set the software clock
- Linux uses software clock for most purposes
- Linux always sets its clock to UTC
- both hardware and software clocks are unreliable
- user `date` utility to query and set system date and time

### Set the hardware clock to the correct time in UTC
- use `hwclock` utility

### Configure the correct timezone
- use `/etc/localtime` file to set the timezone
- use `TZ` variable

### Basic NTP configuration

### Knowledge of using the pool.ntp.org service
- this is a pool of NTP servers
- NTP servers are provided by volunteers
- you do not know to which exact server you connect
- you can narrow down the pool with `0.pool.ntp.org` or `europe.pool.ntp.org`

### Awareness of the ntpq command
- interactive program to query the NTP daemon
- `ntpq -p` to show status of NTP servers that have been queried by the daemon
- asterisk indicates that this server is synced to
- plus indicates server with good times
- minus and x indicate that the server is not considered

### /usr/share/zoneinfo/
- this directory contains zone files
- one of files in this directory may be symlinked from `/etc/localtime`

### /etc/timezone
- this file contains the timezone of the software clock

### /etc/localtime
- this file contains the timezone of the software clock
- often it is a symlink to a zone file in `/usr/share/zoneinfo`

### /etc/ntp.conf
- configuration file of the NTP daemon
- most important set the NTP servers
    - NTP daemon queries each server to determine which one to use

### date
- `date` utility queries and sets the software clock
- syntax: `date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]`

### hwclock
- utility to query and set hardware clock
- `-r` option to query
- `--set --date=newdate` option to set the date
- `--systohc` to set hardware clock based on software clock
- `--hctosys` to set software clock based on hardware clock
- `--utc` or `--localtime` to treat hardware clock as UTC or localtime

### ntpd
- NTP daemon
- syncs software clock to one of the NTP servers that are set in `/etc/ntp.conf`
- works by measuring roundtrip times between server and the client

### ntpdate
- this utility is part of the NTP suite
- sets the time on the client based on some NTP server
- syntax: `ntpdate clock.example.com`

### pool.ntp.org
- subdomain of public NTP servers from volunteers
- servers are accessed in round-robin fashion so you may end up with different server upon `ntpd` restart


## 108.2 System logging (weight: 3)

### Configuration of the syslog daemon
- the daemon is configured through `syslog.conf` file

### Understanding of standard facilities, priorities and actions
- non-comment lines in `syslog.conf` have this pattern: `facility.priority action`
- facility is the type of program or tool that generated the message to be logged
    - valid values are: auth, authpriv, cron, daemon, kern, lpr, mail, mark, news, security, syslog, users, uucp, local[0-7]
        - most services not covered use daemon as facility
- priority indicates the importance of the log message
    - valid values, in escalating order of priority are: debug, info, notice, warning, warn, error, err, crit, alert, emerg, panic
    - `syslogd` will log messages only if the priority of the messages is equal to, or higher than the configured priority
- action is often a file name in the `/var/log/` directory
- facility and priority together are often referred to as selector
- examples
    - `mail.*   /var/log/mail` will log messages of all priorities from mail facility to `/var/log/mail` file
    - `*.emerg  *` will send all emerg-level messages to the consoles of all users who are logged into the computer using text-mode tools
    - `kern.crit    @logger.myhost.com` will send all critical kernel messages to logger.myhost.com
    - `kern.crit    /dev/console` will send critical kernel messages to the console

### Configuration of logrotate
- configured through `/etc/logrotate.conf` config file
- example:
```
# Rotate logs weekly
weekly
# Keep 4 weeks of old logs
rotate 4
# Create new log files after rotation
create
# Compress old log file
compress
# Refer to files for individual packages
include /etc/logrotate.d
# Set miscellaneous options
notifempty
nomail
noolddir
# Rotate `wmtp`, which is not handled by a specific program
/var/log/wmtp {
    monthly
    create 0664 root utmp
    rotate 1
}
```

### Awareness of rsyslog and syslog-ng
- `syslog-ng` is an alternative system logger, that supports more advanced filtering than `syslogd`
- recent version of Ubuntu and Fedora use `rsyslogd`

### syslog.conf
- configuration file of `syslogd`
- non-comment lines have this pattern: `facility.priority action`
- note that a log message can match multiple filters and hence `syslogd` will process it multiple times

### syslogd
- a daemon that writes incoming messages, possibly from other hosts, and writes them to the system log

### klogd
- a daemon, from same package `sysklogd` as the `syslogd` daemon
- manages logging of the kernel messages

### /var/log/
- directory containing log files

### logger
- utility to manually send log messages to the system logger for processing
- syntax: `logger [-isd] [-f file] [-p pri] [-t tag] [-u socket] [message ...]`
    - `-i` records process ID
    - `-s` sends to standard error (and the file if specified)
    - `-d` uses datagrams instead of streams 
    - `-u` allows logging to a network socket

### logrotate
- utility to perform log file rotation

### /etc/logrotate.conf
- configuration file of the `logrotate` utility

### /etc/logrotate.d/
- directory belonging to the `logrotate` utility
- contains files that configure `logrotate` to handle specific log files

### journalctl
- utility on systems that are using `systemd` to query the systemd journal

### /etc/systemd/journald.conf
- configuration file of `journald`
- example: 
```
#  This file is part of systemd.##  systemd is free software; you can redistribute 
#  it and/or modify it under the terms of the GNU Lesser General Public License as 
#  published by the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.
#
# Entries in this file show the compile time defaults.
# You can change settings by editing this file.
# Defaults can be restored by simply deleting this file.
#
# See journald.conf(5) for details.
 [Journal]
#Storage=auto
#Compress=yes
#Seal=yes
#SplitMode=uid
#SyncIntervalSec=5m
#RateLimitInterval=30s
#RateLimitBurst=1000
#SystemMaxUse=
#SystemKeepFree=
#SystemMaxFileSize=
#RuntimeMaxUse=
#RuntimeKeepFree=
#RuntimeMaxFileSize=
#MaxRetentionSec=
#MaxFileSec=1month
#ForwardToSyslog=yes
#ForwardToKMsg=no
#ForwardToConsole=no
#ForwardToWall=yes
#TTYPath=/dev/console
#MaxLevelStore=debug
#MaxLevelSyslog=debug
#MaxLevelKMsg=notice
#MaxLevelConsole=info
#MaxLevelWall=emerg
```

### /var/log/journal/
- default directory where `jounrald` stores journals, aka logs


## 108.3 Mail Transfer Agent (MTA) basics (weight: 3)
### Create e-mail aliases
### Configure e-mail forwarding
### Knowledge of commonly available MTA programs (postfix, sendmail, qmail, exim) (no configuration)
### ~/.forward
### sendmail emulation layer commands
### newaliases
### mail
### mailq
### postfix
### sendmail
### exim
### qmail
### ~/.forward
### sendmail emulation layer commands
### newaliases

## 108.4 Manage printers and printing (weight: 2)
### Basic CUPS configuration (for local and remote printers)
- Applications submit print jobs as PostScript documents
- In case of non-PostScript printers, Ghostscript program converts PostScript into appropriate format 
- CUPS, Common Unix Printing System, manages print queues
- Access CUPs daemon in browser by going to `http://localhost:631`
- print queues are usually subdirectories of `/var/spool/cups`
- CUPS daemon runs in the background watching for jobs to be submitted
- sample `/etc/cups/cupsd.conf` file:
```
<Location /printers>
Order Deny,Allow                    # allow directives modify deny directives
Deny from All                       # Refuse all connections
BrowseAllow from 127.0.0.1          # accept browsing requests from 127.0.0.1
BrowseAllow from 192.168.1.0/24
BrowseAllow from @LOCAL             # accept browsing requests from systems connected to local subnets
Allow from 127.0.0.1                # allow 127.0.0.1 to print
Allow from 192.168.1.0/24
Allow from @LOCAL
</Location>
```

### Manage user print queues
- `lpq -U bob` shows Bob's print queue
- `lprm -U bob 123` to cancel Bob's job with ID 123

### Troubleshoot general printing problems
- `cupsenable` to enable a print queue
- `cupsdisable` to disable a print queue
- `lpmove` to move a print job from one queue to another


### Add and remove jobs from configured printer queues
- `lprm` removes print jobs
- `lpc` to start, stop, and reorder jobs within queues

### CUPS configuration files, tools and utilities
- `/etc/cups/printers.conf` contains printer definitions
- `/etc/cups/cupsd.conf` contains config for the CUPS daemon

### /etc/cups/
- CUPS config dir

### lpd legacy interface (lpr, lprm, lpq)
- users submit print jobs using `lpr` program or programs that wrap it
- `lpq` displays print queue (e.g. `lpq -Php400`)
- `lprm` removes print jobs

## 109.1 Fundamentals of internet protocols (weight 4)

### Demonstrate an understanding of network masks and CIDR notation
- Network mask (aka subnet mask or netmast) is a number that identifies the portion of IP address that's a network address and the part that's computer address
    - e.g. for IP address 134.5.66.123 and network mask 255.255.0.0, the network is 134.5 and computer is 66.123
    - network mask can also be written in short form (CIDR form): 134.5.66.123 with mask 255.255.255.0 is same as 134.5.6.123/24
- CIDR (Classless Inter-Domain Routing) is a notation for representing a range of IP addresses
    - before CIDR there was classful addressing which derived the net mask from the IP address
    - CIDR represents network bit mask with a number after the slash
    - CIDR allows the mask to be any value from /0 to /32
    - e.g. the range 133.4.5.0 - 133.4.5.1 is represented as 133.4.5.0/31
    - e.g. XX.XX.XX.0 thru XX.XX.XX.9 is represented as XX.XX.XX.0/29 and XX.XX.XX.8/31 (because CIDR can express ranges only in base 2)
- Traditionally IPv4 network addresses were broken down into classes
    - each class has a private range, with routers normally dropping packets to private ranges
    - Class A (net mask of /8), with range 1.0.0.0 - 127.255.255.255, private range 10.0.0.0 - 10.255.255.255
    - Class B (net mask of /16), with range 128.0.0.0 - 191.255.255.255, private range 172.16.0.0 - 172.31.255.255
    - Class C (net mask of /24), with range 192.0.0.0 - 223.255.255.255, private range 192.168.0.0 - 192.168.255.255
    - Class D, with range 224.0.0.0 - 239.255.255.255, reserved for multicasting (sending data to multiple computers simulateneously)
    - Class E, with range 240.0.0.0 - 255.255.255.255, reserved for future use
    - Nowadays, no one does classful addressing anymore, however people do continue to use the names "Class A", "Class B", "Class C" network to indicate the network mask of size 24, 16, and 8.

### Knowledge of the differences between private and public “dotted quad” IP addresses
- dotted quad notation shows all 4 bytes of IPv4 address or network mask as decimal numbers separated by a dot (e.g. 144.22.3.6 or 255.255.0.0)
- public ip address is accessible from Internet
- private address is accessible only from withing network
- reserved ranges of ip's for private use:
    - 10.0.0.0 - 10.255.255.255:  16,777,216 hosts (class A)
    - 172.16.0.0 - 172.31.255.255: 1,048,576 hosts (class B)
    - 192.168.0.0 - 192.168.255.255: 65,536 hosts (class C)

### Knowledge about common TCP and UDP ports and services (20, 21, 22, 23, 25, 53, 80, 110, 123, 139, 143, 161, 162, 389, 443, 465, 514, 636, 993, 995)

- 20: ftp data port from which server sends data to client, in active mode
- 21: ftp server command port, where initial client request arrives
- 22: ssh, scp, sftp, port forwarding
- 23: telnet
- 25: smtp
- 53: DNS
- 80: HTTP
- 110: POP3
- 123: NTP
- 139: NetBIOS Session Service
- 143: IMAP
- 161: SNMP
- 162: SNMP Trap
- 389: LDAP
- 443: HTTPS
- 465: Authenticated SMTP over TLS/SSL (SMTPS)
- 514: Remote shell, `rsh`, `remsh`
- 636: LDAPS (LDAP over TLS/SSL)
- 993: IMAPS (IMAP over TLS/SSL)
- 995: POP3S (POP3 over TLS/SSL)

### Knowledge about the differences and major features of UDP, TCP and ICMP
- TCP
    - creates full connections
    - does error checking and correction
    - features lead to small performance penalty
    - layer 4 (application layer) protocols built on top of TCP: SMTP, HTTP, FTP
- UDP
    - simple and fast
    - does not guarantee in-order delivery
    - does not guarantee delivery
    - application layer protocols built on top of UDP: DNS, NFS, media streaming protocols
- ICMP
    - most often used to send errors between computers
    - technically, an internet layer protocol
    - relies upon IP

### Knowledge of the major differences between IPv4 and IPv6
- Converting between MAC address and IP address 
    - For IPv4, TCP/IP stack uses ARP (Address Resolution protocol
    - For IPv6, TCP-IP stack uses (NDP) Neighbor Discovery Protcol
- Expressing addresses
    - IPv4: four base-10 numbers separated by a dot (e.g. 134.43.1.23)
    - IPv6: eight groups of four hexadecimal numbers seprated by colon (e.g. fed1:0db8:85a3:08d3:1319:8a2e:0370:7334)

### Knowledge of the basic features of IPv6
- IPv6 addresses can be built in part from computer's MAC address
- Larger address space (16 bytes per address = 2^128)
- No need for NATs
- auto-configuration: no need for DHCP
- IPSec
- No broadcast, but does have multicast
- Anycast
- simpler headers -> faster parsing and routing

### /etc/services
- config file that maps port numbers to named services
- meant for programs to do `getportbyname('pop3')` type queries

### IPv4, IPv6
- IPv4
    - Internet Protocol version 4
    - deployed in 1983 at ARPANET
    - connectionless, best-effort, delivery not guaranteed
    - for packet-switched networks
    - a 32-bit address is assigned to each connected device
- IPv6
    - Internet Protocol version 6
    - replaces IPv4

### Subnetting
- process of designating some high-order bits from the host part and grouping them with the network mask to form the subnet mask
- done to have subnetworks inside main network
- all hosts on a subnetwork have same network prefix
- a /24 network can have following number of subnets for each subnet mask:
    - /24: 1 subnet
    - /25: 2 subnets
    - /26: 4 subnets
    - ...
    - in general: 2 to the power of number of subnet bits - number of network bits

### TCP, UDP, ICMP
- TCP: Transimission Control Protocol, a layer 3 (transport layer) protocol
- UDP: User Datagram Protocol, a layer 3 (transport layer) protocol
- ICMP: Internet Control Message Protocol, a simple protocol for communicating data


## 109.2 Basic network configuration (weight 4)

### Manually and automatically configure network interfaces
- manual configuration is done with `ifconfig`, `ifup`, and `ifdown` utilities
- note: Ubuntu on desktops and laptops uses `network-manager` daemon and GUI to configure the network
    - `network-manager` will only handle interfaces not listed in `/etc/network/interfaces`

### Basic TCP/IP host configuration
- Configuration through DHCP (Dynamic Host Configuration Protocol)
    - DHCP client starts at bootup and fetches config from DHCP server
    - DHCP client must renew the DHCP lease periodically
    - Fedora and Red Hat: enable DHCP in `/etc/sysconfig/network-scripts/ifcfg-<network iface name>` with `BOOTPROTO="dhcp"`
    - Ubuntu: enable DHCP in `/etc/network/interfaces` with the `dhcp` parameter in line `iface eth0 inet dhcp`
    - Run DHCP client manually for network interface `eth0` with command: `sudo dhclient eth0`
    - Look for string `DHCP` in the system log to see what DHCP client did
    - Ubuntu: DHCP leases are in `/var/lib/dhcp/dhclient.leases`
- Manual configuration
    - Config files: `/etc/sysconfig/network-scripts/ifcfg-<iface-name>` or `/etc/network/interfaces`
    - example `/etc/sysconfig/network-scripts/ifcfg-<iface-name>`
```
DEVICE="p2p1"
BOOTPROTO="static"
IPADDR="192.168.29.39"  # ip address of this system. can also be set with via ifconfig utility
NETMASK="255.255.255.0"
NETWORK="192.168.29.0"
BROADCAST="192.168.29.255"
GATEWAY="192.168.29.1"
ONBOOT="yes"
```

### Setting a default route
- a route describes which interface and gateway to take to reach a specific range of ip addresses
- a default route, is the route taken when no other route applies to the destination ip address
- use `route` utility to show the kernel routing table
- Add a default route: `route add default gw 192.168.29.1`
    - default route is shown in the `route` utility output as the line with destination `0.0.0.0`

### /etc/hostname
- file containing hostname of the computer
- the hostname is set from the value found in this file during boot time

### /etc/hosts
- file containing mapping of IP addresses to hostnames
- format: ip hostname [aliases...]
- used before a hostname is looked up with DNS
    - used during system boot when no DNS is available

### /etc/nsswitch.conf
- config file of Name Service Switch 
- Used by GNU C library to determine sources from which to obtain name-service information, in a range of categories and in what order
- The first column specifies the database
- Example:
```
passwd:         compat
group:          compat
shadow:         compat

hosts:          dns [!UNAVAIL=return] files
networks:       nis [NOTFOUND=return] files
ethers:         nis [NOTFOUND=return] files
protocols:      nis [NOTFOUND=return] files
rpc:            nis [NOTFOUND=return] files
services:       nis [NOTFOUND=return] files
```
- the line starting with "hosts" configures host lookup first through dns and then though the hosts file (e.g. `/etc/hosts`)


### ifconfig
- utility to configure a network interface
- `ifconfig eth0 192.168.42.42` to assign an ip address to eth0 interface
- `ifconfig eth0 netmask 255.255.0.0` to assign a netmask to eth0 interface 
    - for ip addresses outside the network you will need to specify a gateway
- `ifconfig -a` shows all interfaces including disabled ones
- `ifconfig eth0 down` to disable the eth0 interface
- `ifconfig eth0 up` to enable the eth0 interface
- `ifconfig eth0 broadcast 192.168.2.255` to change broadcast address of the eth0 interface
- `ifconfig eth0 mtu 1500` to set MTU to 1500 bytes, largest Ethernet supported size at the network layer

### ifup
- utility to bring a network interface up
- bring interface `eth0` up: `ifup eth0`

### ifdown
- utility to bring a network interface down
- bring interface `eth0` down: `ifdown eth0`

### ip
- utility to show / manipulate routing, devices, policy routing and tunnels

### route
- utility to manipulate IP routing table
- Add a default route: `route add default gw 192.168.29.1`


### ping
- utility to send ICMP ECHO_REQUEST to network hosts 


## 109.3 Basic network troubleshooting
### Manually and automatically configure network interfaces and routing tables to include adding, starting, stopping, restarting, deleting or reconfiguring network interfaces

### Change, view, or configure the routing table and correct an improperly set default route manually

### Debug problems associated with the network configuration


### ifconfig
- utility to configure a network interface

### ip
- utility to show / manipulate routing, devices, policy routing and tunnels

### ifup
- utility to bring a network interface up

### ifdown
- utility to bring a network interface down

### route
- utility to manipulate IP routing table

### host
- DNS lookup utility

### hostname
- utility to manipulate system's host name

### dig
- DNS lookup utility

### netstat
- utility to print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships

### ping
- utility to send ICMP ECHO_REQUEST to network hosts 

### ping6
- utility to send ICMP ECHO_REQUEST to network hosts addressed with IPv6

### traceroute
- utlity to trace path to a network host

### traceroute6
- utlity to trace path to a network host, with support for IPv6 addresses

### tracepath
- utility to trace path to a network hosts, discovering MTU along the way

### tracepath6
- utility to trace path to a network hosts, discovering MTU along the way, with support for IPv6 addresses

### netcat
- aka `nc`
- support IPv6
- utility is used for just about anything under the sun involving TCP, UDP, or UNIX-domain sockets.  
    - open TCP connections
    - send UDP packets
    - listen on arbitrary TCP and UDP ports
    - do port scanning
    - simple TCP proxies
    - shell-script based HTTP clients and servers
    - network daemon testing
    - a SOCKS or HTTP ProxyCommand for ssh(1)

## 109.4 Configure client side DNS (weight: 2)

### Query remote DNS servers
- use `dig` utility to query a remote DNS server
    - e.g. `dig somewebsite.com @some.dns.server.com` outputs ` droptips.com.       86400   IN      A       89.238.134.5`
        - 86400 is the TTL which is for how long the DNS will cache
    - e.g. `dig MX google.co.uk @ns1.google.com` to query for MX records
 
### Configure local name resolution and use remote DNS servers
- local names resolution can be done using `/etc/hosts` file
    - e.g. `127.0.0.1 mypc localhost blah`
- to use remove DNS servers, use `/etc/resolv.conf` file
    - add line `nameserver 8.8.8.8` to use DNS server with IP address 8.8.8.8

### Modify the order in which name resolution is done
- use config file `/etc/nsswitch.conf`
- query DNS servers, then files: `hosts:          dns [!UNAVAIL=return] files`
- query files, then DNS servers: `hosts:          files dns [!UNAVAIL=return]`

### /etc/hosts
- file containing mapping of IP addresses to hostnames
- format: ip hostname [aliases...]
- used before a hostname is looked up with DNS
    - used during system boot when no DNS is available
- could be used to redirect host names of ad server to localhost to avoid loading ads

### /etc/resolv.conf
- config file containing the DNS server used by the computer
- may be overwritten when network is managed by a network manager or configured via DHCP

### /etc/nsswitch.conf
- Name Service Switch config file
- Used by GNU C library to determine the sources from which to obtain name-service information in a range of categories and in what order.
- The first column specifies the database
- Example file:
```
passwd:         compat
group:          compat
shadow:         compat

hosts:          dns [!UNAVAIL=return] files
networks:       nis [NOTFOUND=return] files
ethers:         nis [NOTFOUND=return] files
protocols:      nis [NOTFOUND=return] files
rpc:            nis [NOTFOUND=return] files
services:       nis [NOTFOUND=return] files
```
- `hosts:          dns [!UNAVAIL=return] files`
    - to resolve host name consult dns first, then files like `/etc/hosts`

### host
- utility for performing simple DNS lookups

### dig
- dig = domain information groper, a flexible tool for interrogating DNS name servers

### getent
- utility to get entries from Name Service Switch libraries

## 110.1 Perform security administration tasks (weight: 3)
### Audit a system to find files with the suid/sgid bit set
### Set or change user passwords and password aging information
### Being able to use nmap and netstat to discover open ports on a system
### Set up limits on user logins, processes and memory usage
### Determine which users have logged in to the system or are currently logged in
### Basic sudo configuration and usage
### find
### passwd
### fuser
### lsof
### nmap
### chage
### netstat
### sudo
### /etc/sudoers
### su
### usermod
### ulimit
### who, w, last


## 110.2 Setup host security (weight: 3)
### Awareness of shadow passwords and how they work
### Turn off network services not in use
### Understand the role of TCP wrappers
### /etc/nologin
### /etc/passwd
### /etc/shadow
### /etc/xinetd.d/
### /etc/xinetd.conf
### /etc/inetd.d/
### /etc/inetd.conf
### /etc/inittab
### /etc/init.d/
### /etc/hosts.allow
### /etc/hosts.deny

## 110.3 Securing data with encryption
### Perform basic OpenSSH 2 client configuration and usage
### Understand the role of OpenSSH 2 server host keys
### Perform basic GnuPG configuration, usage and revocation
### Understand SSH port tunnels (including X11 tunnels)
### ssh
### ssh-keygen
### ssh-agent
### ssh-add
### ~/.ssh/id_rsa and id_rsa.pub
### ~/.ssh/id_dsa and id_dsa.pub
### /etc/ssh/ssh_host_rsa_key and ssh_host_rsa_key.pub
### /etc/ssh/ssh_host_dsa_key and ssh_host_dsa_key.pub
### ~/.ssh/authorized_keys
### ssh_known_hosts
### gpg
### ~/.gnupg/

# References

[1] http://networkengineering.stackexchange.com/questions/19840/does-cidr-really-do-away-with-ip-address-classes
[2] https://www.gitbook.com/book/jadi/lpic1/details
