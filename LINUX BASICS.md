- ACL support has to be  turned on
- `tune2fs -l /dev/sda5 | grep "mount options"`  allwos you to see if acls are enables for a file system
- `facl`  file access control list 
- when using acl regular posix has no way of representing them, the last letter on the permissions has to be a `+`  to show there are more acl permissions
- 
```
setfacl -m u:<user>:<rw> file
-m to modify
-x to remove
-g for group
-r for read access

getfacl <file>|directory to get the facl permissions of a file
```
<u><b>SETTING DIRECTORY ACL_</b>:</u>
```
setfacl -m d:g:<group>:r ./<directory>
```
- any file created under the directory will inherit from the directory
<u> <B>SHARED LIBRARIES</B></u>
- contain peices of code used by more than one program
- /lib- contains system libraries
- libraries end in .so extension
- `ldconfig -v ` makes the library daemon to list all available libraries
- `ldd <program>` shows all the library dependencies for a program
-  to use libraries for your own purposes use the ~/.local/lib to store those libraries
- tell the library daemon by using `sudo ldconfig -n ~/.local/lib`
- for everybody use 
- in `/etc/ld.so.conf.d create a custom config file and add your custom library path
<u><b>INSTALLING AND MANAGING SOFTWARE IN DEB LINUX</b></u>
- `dpkg`: designed to install .deb files, it didnt track dependencies
- `apt get` : designed to install your program and get and install the dependencies
- `apt` - more efficient with dep tracking and management
- `/etc/apt/sources.list`  original file for source of packages
- `/etc/apt/sources.list.d` is where you add custom list files when you install something/ or add a  new source to pull packages 
- `sudo apt search <keyword> ` allows you to search for the packae if you dont know the name
- `sudo apt list <package-name>` get more information 
- `sudo apt dist-upgrade` update the distro

**INSTALLING AND MANAGING SOFTWARE IN REDHAT BASED LINUX

- `rpm` original installation package, not great like dpkg
- `yum` yellow dog update manager - it would also automatically pull dependencies, the apt get of redhat
- `dnf` share the same confi files with yum
- `/etc/yum.repos.d` default location for source list
- `yum search`  find a package by keyname
- `yum list ` gives u version info
- `yum info ` gives you things like the description and more info about the package
- you can just replace `yum ` with `dnf` and get the same results
- to add custom repo add a .repo file to `/etc/yum.repods.d`
- use `rpm --import file.asc` to import the key for the signed packages
- `sudo yum update`  to update packages as well as kernel updates and atuo remove what is nolonger needed
- `sudo yum remove| sudo dnf remove` to remove packages, dnf will do a  better way of removing
- this creates a cache of the package in case you will need it again so you `yum erase`  to remove even the cache

*SERVICES*:
-> INIT SYSTEMS:
- the first service that gets started when you start linux
- `systemd`- binary that runs 
	- [systemd.io](https://systemd.io)
	- 
- `sysv5 init system`- system 5 came from unix system, 
- `upstart` developed by canonical
	- designed to be compatible with `sysvinit`
	- support for removable hardware
- `systemctl` shows you that you run systemd
- `ps aux`  pid 1 shows you the path of the init system running
- When the init is a binary you can do `man init` to see documentation from the vendor on which init system it is
### *SysvInit*
- calls  the `/sbin/init` binary
- `/etc/rc.d/rc.local and rc.sysinit` are  important  scripts for the init system
- modify the `rc.local` to configure what happens when the system is in a specific state
- `runlevels` detect what state the system is in
	- documented in the init table `/etc/init/rc.d`
	- `runlevel` shows if there has been changes since the system was booted and what level the system is in
	- `/etc/rc.d/r<number>.d ` files contain scripts on what to do in each run level 
#### managing services:
- to check if  a service is running  `chkconfig <service> on|off` start the service when the system is running
- `sudo service <service_name> status` to check if the service is running
- `chckconfig --list`  list of services and their run levels
- `chkconfig off `  to turn a service off on all levels
- `chckconfig  --level 235 <service> on`  turns on the service for the level2,3,5

 ## SYSTEMD
 - found in `/lib/systemd`
 - config found in `/lib/systemd/system`  part of system and `/etc/systemd/system` custom stuff that dont get overwritten during an upgrade
 - to change default target without rebooting use `sudo systemctl isolate <file.target`
 - `systemtctl get-default` gets the default mode
 - MANAGING SERVICES WITH SYSTEMD:
	 - usually system target files shows all the other target files it requires
	 - creating a service:
		 - [Unit]
		 - Description
		 - After=which target should be loaded first e.g network.target
		 - [Service]
			 - User
			 - WorkingDirectory
			 - ExecStart
			 - Restart
		- [Install]
			- WantedBy=<runlevel.target>
	-  `sudo systemctl damon reload`  after making changes to a service file
MANAGING STORAGE:**
**- Partitions:**
	- two primary efi and primary
	- gpt- creates volumes on the harddrive
	- `fdisk`- for modifying MBR
		-  -p print
		- - n create a new partition
		- - w write to disk
	- `lsblk`  lists all block devices on your machine

	- `gdisk` for gpt
		- also uses pnw
	- gnu parted: gui version of parted
 ***LINUX FILE SYSTEMS**:*
 - Extended file system: (ext)
	 - most common fs in linux systems
	 - volumes up to 1EB
	 - files up to 16TB
	 - unlimited directories
	 - journaling
		 - you dont write directly to disk, you write to another file then commit the file when the operation is complete
	- General purpose
- XFS: xtends file system
	- developed by SGI
	- up to 8EB
	- file up to 1PB
	- journaling
	- graphics/dbs
- BTRFS: B- tree FS
	- has almost all features
	- snapshots
	- journaling
	- network storage
- CREATING FS:
	-   `/usr/sbin/mkfs*` are the set of tools used to make file systems
	- swap are not physical fs
	- `sudo mkswap`  makes swap partition
	-  to change fs you have to backup your data then use the `mkfs` you can move from one ext version to another
	- changing a label `e2label <mount_point> <label_name>`, they can be changed without affecting the physical device
	- 
	MAINTAINING FS:
    - when you have a software error on the disk,e.g long boot times
    -  linux runs file system checks at boottime
    - `/etc/fstab` is where the info for the file system table is stored
    - each line for each disk in the file has two bits at the end
	    - first bit is the dump bit, backup using an old system
	    - the second one is the pass bit, it determines if a file health system check should be done, 1= high prio, 2= low prio
   -  `fsck -t <filesystem> <disk>` is used to do a file system check while the file system is online
	   -  if the disk is messed up give it the file system 
  DISK MONITORING:
  - `df -h`  view the dfisk usage in human readable form
  - `du -chd 1 <directory>`  shows you what each file and folder is usingn human readable format and also only show the usage of a directory
  - `systat` and `iotop`  are good at reporting the iops of the system
  - `iotop` shows the usages by process
  MOUNTING REMOVABLE DRIVES:
  - 
