# Adding a new Block Device to CentOS

## Create and attach a new volume to a VM in AWS

Follow the instructions at [Creating EBS Volumes](../aws/ebs/ebs_create_volume.md) to create a new EBS volume and to attach it to your EC2 instance.

## List the newly attached volume 

1\. Login to your Linux machine.

2\. Run `lsblk` to display a list of attached block devices

```shell
$ lsblk
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  28G  0 disk
└─xvda1 202:1    0  28G  0 part /
xvdf    202:80   0  50G  0 disk
```

The newly attached volume should be listed as device `xvdf` (which in our case is the name the Linux kernel gave
 the attached volume). Since Linux follows the convention to provide all devices in folder `dev`, the full device name in Linux is `/dev/xvdf`.
 
## Create a Partition Table and a Partition on the Block Device 

First thing you have to do is to create a partition on the newly attached block device. Managing partitions on Linux is 
either done via __fdisk__ or __parted__. This guide will use __fdisk__ to create a partition table with one partition.

1\. Start __fdisk__ on device `/dev/xvdf`:
 
```shell
$ sudo fdisk /dev/xvdf

Welcome to fdisk (util-linux 2.30.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x50714ef9.

Command (m for help):
``` 

2\. Enter command __p__ to display all existing partitions on the device:

```shell
Command (m for help): p
Disk /dev/xvdf: 50 GiB, 53687091200 bytes, 104857600 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x50714ef9
``` 

3\. As we can see there is no partition on the newly attached device and `fdisk` prematurely added a DOS partition table
which is not what we intended. Enter command __g__ to replace the DOS partition table with a GPT partition table:

```shell
Command (m for help): g
Created a new GPT disklabel (GUID: C43E736A-0F19-453C-BC9E-BE50BEDB675D).
``` 

4\. Enter command __n__ to create a new partition spanning all storage provided by the newly attached device. 
Simply accept all default values by pressing return: 

```shell
Command (m for help): n
Partition number (1-128, default 1): 
First sector (2048-104857566, default 2048):
Last sector, +sectors or +size{K,M,G,T,P} (2048-104857566, default 104857566):

Created a new partition 1 of type 'Linux filesystem' and of size 50 GiB.
``` 

5\. Enter command __w__ to write all changes to the newly attached device and exit __fdisk__:

```shell
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
``` 

6\. Run __fdisk__ again to check all newly created partitions on the device:

```shell
$ sudo fdisk --list  /dev/xvdf
Disk /dev/xvdf: 50 GiB, 53687091200 bytes, 104857600 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: C43E736A-0F19-453C-BC9E-BE50BEDB675D

Device     Start       End   Sectors Size Type
/dev/xvdf1  2048 104857566 104855519  50G Linux filesystem
``` 

Please note that our newly created partition is listed as device `/dev/xvdf1` and has a unique disk identifier in 
UUID-format.

## Format the Partition with a Linux File System

Next thing you have to do is to create a Linux file system on the newly created partition. This is comparable to format 
a partition in Windows. In this particular guide we
will use the __ext4__ file system. An alternative file system to use would be __xfs__.

1\. Run utility __mkfs.ext4__ on the device representing the newly created partition: 

```shell
$ sudo mkfs.ext4 /dev/xvdf1
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
3276800 inodes, 13106939 blocks
655346 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=2162163712
400 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
``` 

## Mount the Partition

In order to access the newly created partition with the __ext4__ file system you are going to have to mount the partition.
Mounting always requires a source device (a block device or partition) and a target folder (mounting point) in the 
file system of your machine to used to access the filesystem content.

!!! tip "Always use UUIDs to identify volumes and partitions not device names"
    Device names of attached volumes may change or - more precisely - may be reassigned differently, 
    if new volumes are added or existing volumes are removed. This affects all devices names of partitions located
    on the attached volumes as well. Thus, it's not a good idea to use device names when mounting partitions. UUIDs
    of volumes and partitions on the other hand will never change. Thus, using UUIDs when mounting partition is definitely
    the better option!

1\. Run __blkid__ to display the UUIDs of all available partitions:

```shell
$ sudo blkid
/dev/xvda1: LABEL="/" UUID="55da5202-8008-43e8-8ade-2572319d9185" TYPE="xfs" PARTLABEL="Linux" PARTUUID="591e81f0-99a2-498d-93ec-c9ec776ecf42"
/dev/xvdf1: UUID="68ca2d3a-ac44-43f5-b64e-7c91b57a50f0" TYPE="ext4" PARTUUID="ed4fb5c3-6a61-49ea-bced-66144b16d38b"
``` 

or run __lsblk -d -fs__ to display the information we need to know to mount the partitions:

```shell
$ sudo lsblk -d -fs
NAME  FSTYPE LABEL UUID                                 MOUNTPOINT
xvda1 xfs    /     55da5202-8008-43e8-8ade-2572319d9185 /
xvdf1 ext4         68ca2d3a-ac44-43f5-b64e-7c91b57a50f0
``` 
 
2\. Create a target directory __/mnt/data0__ the partition should be mounted to:

```shell
$ sudo mkdir -p /mnt/data0
```

!!! question "Which Folder should I chose as a Mounting Point?"
    In which folder you would like to mount the newly created partition depends on your use case: 
    If you want to keep all Docker resources separate from the root partition, you will use __/var/docker/lib__ as a 
    mounting point.
    If you want to keep all user home directories off the root partition, you will use __/home__ as your mounting point.
    Using a subfolder in __/mnt__ is a good mounting point to start with if you don't know what your going to do with
    your additional storage. Remember: after you have mounted the partition in a subfolder of __/mnt__ you still can
    decide to mount specific directories on the partition at a different location!
      
3\. Add a new line to file __/etc/fstab__ which contains a list of all permanent mounts looking like this (requires root privileges!):

```text
UUID=68ca2d3a-ac44-43f5-b64e-7c91b57a50f0     /mnt/data0           ext4    defaults  0   0
```

| Field Index | Description |
| --- | --- |
| 1 | The UUID of the partition or block device to mount. |
| 2 | The mountpoint which is the directory to be used to access the filesystem content. |
| 3 | The filesystem type. |
| 4 | List of options to be used when mounting the filesystem. |
| 5 | Flag which indicates if the filesystem should be dumped. |
| 6 | Priority of the filesystem check (0: not checked, 1: root filesystem, 2: attached filesystem) | 

4\. Run __mount --all__ to mount all all entries in __/etc/fstab__. Now the volume will be mounted correctly even after a reboot of the Linux system.

```shell
$ sudo mount --all
```

4\. Run __ls -al__ on the mounting point to check if the partition has been mounted successfully:

```shell
$ ls -al /mnt/data0
total 20
drwxr-xr-x. 3 root root  4096 Apr 22 16:46 .
drwxr-xr-x. 3 root root    19 Apr 23 10:38 ..
drwx------. 2 root root 16384 Apr 22 16:46 lost+found
```
