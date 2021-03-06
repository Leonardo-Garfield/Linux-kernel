# Linux Kernel 0.11 on VirtualBox

There are many documents about the impmentation of Linux 0.11 running on Bochs, Qemu. I saw no one doing it on VirtualBox, which I am more familiar with, than Bochs. It can be simple and straightforward, though still took me some time to get it right.

The purpose is to get Linux 0.11 run under VirtualBox. It contains 2 files: boot.img (as virtual floppy) and root.vdi (as virtual HDD).

I use Ubuntu as host computer to run Bochs, to modify and compile the Linux 0.11 source code, to get boot.img (original file name was bootimage-0.11). Together with root.vdi (converted from root.img, original file name was hdc-0.11.img) from zip file download, I run VirtualBox with what-just-compiled boot.img as virtual floppy drive, and root.vdi as virtual HDD drive. So I can do whatever I want to do with Linux 0.11 on VirtualBox. 

## Environment
- **Ubuntu 14.04 (running Bochs with Linux 0.11 & source code)**: I create virtual machines under my MacOS X for experiments. I choose arbitrarily a virtual machine running Ubuntu 14.04 to run Bochs. Bochs supports many other platforms which you can choose. 
- **MacOS X El Captain (running VirtualBox with Linux 0.11 & source code)**: You may choose your preferred platform to run VirtualBox, not limited to MacOS.

## Prerequisite
- **Bochs 2.6.8** - [http://bochs.sourceforge.net](http://bochs.sourceforge.net)
- **linux-0.11-devel-040329.zip** - Linux 0.11 source code from  [http://www.oldlinux.org/Linux.old/bochs/linux-0.11-devel-040329.zip](http://www.oldlinux.org/Linux.old/bochs/linux-0.11-devel-040329.zip    )
- **VirtualBox 5.0.16** - [https://www.virtualbox.org](https://www.virtualbox.org)

## Preparation on host computer (Ubuntu)
- download & install **Bochs**
- download & unzip **linux_devel_040329.zip** : only 3 files are required: **bochsrc-hd.bxrc**; **bootimage-0.11**; **hdc-0.11.img**

## Steps
In this section, I use '$' as prompt when you are at host, and '#' as prompt when you are at linux 0.11 environment under Bochs.

1. Update **bochsrc-hd.bxrc** with the latest syntax requirements by **Bochs 2.6.8** (which changes quite a lot compared to the original file of bochsrc.bxrc)
2. Run **Bochs** and select the file **bochsrc-hd.bxrc** or   
`   $ bochs -f bochsrc-hd.bxrc
`
3. You will be at the directory of /usr/root after boot from Linux 0.11. Change directory to /usr/src/linux/kernel/blk_drv and modify **hd.c**, which you can find in subdirectory containing the modified **hd.c**
 (reference: [https://virtuallyfun.superglobalmegacorp.com/2010/08/13/great-resource-for-ancient-linux/](https://virtuallyfun.superglobalmegacorp.com/2010/08/13/great-resource-for-ancient-linux/)) 
4. Go back to the root directory of the source code, and compile  
`    # cd /usr/src/linux  
`

    `# make clean
` 

    `# make
`

    (Several Makefiles need to be updated if you not using the root.img here, but using the original hdc-0.11.img. Check reference [http://11210601.blog.51cto.com/11200601/1744524](http://11210601.blog.51cto.com/11200601/1744524)
)
5. "Image" file is generated after compilation. Now write it back to floppy of Bochs so we will be able to read it from host computer

    `# dd bs=8192 if=Image of=/dev/fd0
`
6. Switch back to host computer. You will find the file **bootimage-0.11** has beed updated. Save it with a new name **boot.img**, with reasons  1. for clariy in naming; 2. VirtualBox requires .img as file name extension. 

## Preparation on host running VirtualBox
- download & install **VirtualBox**

- Get 2 files ready: 

    - **boot.img** from previous step
    - **root.img** (or **hdc-0.11.img** from **linux_devel_040329.zip**)

- Convert the root.img to VirtualBox VDI format by issuing the following VirutalBox command

    `	$ VBoxManage convertfromraw --format VDI root.img root.vdi
`


## Steps
1. Launch VirtualBox
2. Select - New. Choose whatever name you like. Select "Type - Linux", and "Version - Linux 2.2"
![image](https://dl.dropboxusercontent.com/u/26460417/VirtualBox_Linux_0-1.png)
3. 64MB memory is enough as minimum requirement. You can try with more memory. 
![image](https://dl.dropboxusercontent.com/u/26460417/VirtualBox_Linux_0-2.png)
4. Select root.vdi we convert from root.img (filename in zip download was hdc-0.11.img)
![image](https://dl.dropboxusercontent.com/u/26460417/VirtualBox_Linux_0-3.png)
5. Linux 0.11 on VirtualBox is created. Don't launch it yet. We need to add Floppy drive as the boot device first. Click the "Storage" 
![image](https://dl.dropboxusercontent.com/u/26460417/VirtualBox_Linux_0-4.png)
6. Click the icon "Diamond with Plus sign"  and select "Add Floppy Controller". 
![image](https://dl.dropboxusercontent.com/u/26460417/VirtualBox_Linux_0-5.png)
7. "Controller: Floppy" is added to the Storage tree. Click the icon "Floppy with Plus sign" to "Adds Floppy Drive"
![image](https://dl.dropboxusercontent.com/u/26460417/VirtualBox_Linux_0-6.png)
8. Choose "Choose disk". And Select the file we created: boot.img (or bootimage-0.11.img)
![image](https://dl.dropboxusercontent.com/u/26460417/VirtualBox_Linux_0-7.png)
9. The screen shows the file "boot.img" as the Floppy
![image](https://dl.dropboxusercontent.com/u/26460417/VirtualBox_Linux_0-8.png)
10. After completion of the settings. Launch "Linux 0,11" on VirtualBox" 
![image](https://dl.dropboxusercontent.com/u/26460417/VirtualBox_Linux_0.png)











   

 