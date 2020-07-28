### INSTALL GUIDE FOR KISSLINUX

This file is split into 5 sections detailing how to install
KISS Linux, this is my personal install process and is done
without having to use an Arch, Void or Gentoo ISO.
This guide is inspired by @mcpcpc's guide here -> [HERE] (https://github.com/mcpcpc/install)

From this point onward the `#` will represent commands that
require `sudo` or elevated access and the `$` will represent commands that require no elevated access.


* Introduction
* Partitioning
* Preparing for Chroot
* Install KISS
* Post-Install
* X and Friends


#### INTRODUCTION

This install method does not follow the conventional KISS
Linux recommended method, it is ideal for those who want to
try out KISS but aren't so set on completely leaving thier
current OS for whatever reason.

#### PARTITIONING

This step assumes that you are installing KISS Linux on an
UEFI-capable computer and that you have more than one
partition and that one of those partitions is free.

If the above is true then you can proceed, otherwise you
might wanna resize your partition. (google derr)


#### PREPPING FOR CHROOT

NB: `tmux` is an added advantage. (You might wanna install
it.)

- Start tmux (if it is installed).
  
  `$ tmux`

- Format the free partition.

    `# mkfs.ext4 /dev/sdXY`  - For your root.

    `# mkswap /dev/sdXY` - For your swap. (If you need one.)

- Mount the formatted paritions.

    `# mkdir /mnt/kiss` - KISS local mount dir.

    `# mount /dev/sdXY /mnt/kiss` - Mount drive on dir

    `$ wget https://github.com/kisslinux/repo/releases/download/1.12.0/kiss-chroot.tar.xz` - KISS Chroot

- Extract the KISS Chroot to the KISS local mount dir

    `# tar xvf kiss-chroot.tar.xz -C /mnt/kiss --strip-components 1` - Extract & Strip

    `$ wget https://raw.githubusercontent.com/kisslinux/kiss/master/contrib/kiss-chroot` - Chroot script

    `$ chmod +x kiss-chroot` - Make the chroot executable

- Chroot into the partition.

    `# ./kiss-chroot /mnt/kiss` - Chroot into kiss

- Mount your boot partition (once you enter chroot all
    commands are by default)
  
    `# mkdir -p /boot/efi` - Make boot and efi dir

    `# echo -e "/dev/sdXY\t\t/boot\t\tvfat\t\tnoauto,noatime\t1 2" >> /etc/fstab` - sdXY represents your boot partitions

    `# echo -e "/dev/sdXY\t\t/\t\text4\t\tnoatime\t\t0 1" >> /etc/fstab` - sdXY represnts your root partition

    `echo "kiss" >> /etc/hostname` - You can modify "kiss"
    to whatever you want.


#### INSTALLING KISS

- Create the needed directories for firmware.

    `# mkdir -p /usr/lib/firmware`
    
    `# mkdir -p /usr/lib/firmware/amdgpu`
    
    `# mkdir -p /usr/lib/firmware/amd`
    
    `# mkdir -p /usr/lib/firmware/amd-ucode`

- Uset and export environment variables. (optional)

    `unset CFLAGS` 
    
    `unset CXXFLAGS`

    `export KISS_PROMPT=0`

- Brace yourself.

    `# kiss b e2fsprogs dosfstools util-linux eudev libelf ncurses openssh` - These are needed

    `# kiss i e2fsprogs dosfstools util-linux eudev libelf ncurses openssh` - Install everything
