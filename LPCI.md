 #####System Architecture:
  [LPI](https://learning.lpi.org/en/learning-materials/101-500/101/101.2/101.2_01/) 
 - when facing issues first port of action is to check if hardware is being detected by OS
 identify connected devices:
 - ```lspci``` - shows all devices connected(Peripheral component interconnect)
 - ``` lsusb``` -shows USB devices currently connected to the machine
 - kernel modules for hardware are called drivers
 -  To list a specific devices by their address use ```lspci -s <addr> -v
    ** you can get the address from the list ```lspci``` the hex value is the address
 - to check if the kernel  module of the device is in use ``` lspci -s <address> -k ```
 - ``` lsusb -v -d <address> ``` displays more information, if you add -t you see them in hierarchical tree

         class - identifies the general category
- show all modules ```lsmod```
- kmod -  handles linux insert, remove,list
- ```modeprob``` is used to load and unload kernel modules use ```-r```  to unload the module and its related modules
- You can  load a modules with parameters using ```modinfo -p``` to change its behaviour
       - change parameters for a module using ``` /etc/modprobe.conf or individual .conf files in the directory /etc/modp          /etc/modprobe.d 
       - blacklist a problematic module from loading using ```/etc/modprobe.d/blacklist.conf```   
- Device information is stored in directories /proc  and /sys, __they are mount points not paths in the device fs__
- they are called Pseudo-filesystems
 ```/proc``` contains process and hardware resources:
     -``` /proc/cpuinfo: CPU(s) found by the OS
     -``` /pro/interrupts:  number of interrupts per IO device per CPU
     -``` /proc/ioports: registered  I/O ports regions in use
     -``` /proc/dma: registered direct memory access channels in use
     -``` /dev associated with system and storage devices
     - Hotplug - detection and configuration of a device while the system is running
     - /etc/udev/rules.d stores the rules to be used when new device 'hotplugged'
    

    ##### STORAGE DEVICES:
- referred to as block devices because data is read to and fro in blocks of buffered data with different sizes and positions
 ``` /dev/hdc and /dev/hda and /dev/hdb are reserved for the master and slave directories on the first IDE channel```

[NEXT](./boot_system.md)

