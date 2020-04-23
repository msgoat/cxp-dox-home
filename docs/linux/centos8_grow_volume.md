# Resize an existing Block Device on CentOS

## Motivation 

Very often you will run into capacity problems regarding the block devices you attached to your Linux system.
This article is supposed to help you with these kind of problems.

## Growing EBS Volumes in AWS

Follow the instructions at [Resizing EBS Volumes](../aws/ebs/ebs_resize_volume.md) to increase the size of an existing 
EBS volume attached to your EC2 instance.

## List the modified Block Device 

1\. Login to your Linux machine.

2\. Run `lsblk` to display a list of the attached block devices

```shell
$ lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0   28G  0 disk
└─xvda1 202:1    0   28G  0 part /
xvdf    202:80   0  100G  0 disk
└─xvdf1 202:81   0   50G  0 part /mnt/data0
```

Please note, that the size of block device `/dev/xvdf` has increased to 100 GB but the size of partition `/dev/xvdf1` ist still the same.

## Grow an existing Partition  

First thing you have to do is to increase the size of the existing partition `/dev/xvdf` up to the new size of the underlying block device.

1\. Run __growpart__ on block device `/dev/xvdf` with the number of the partition to grow (here: 1 for first partition `/dev/xvdf1`):
 
```shell
$ sudo growpart /dev/xvdf 1
CHANGED: partition=1 start=2048 old: size=104855519 end=104857567 new: size=209713119 end=209715167
``` 

## Extend the File System

Next thing you have to do is to extend the filesystem of the resized partition.

1\. Use the __df -h__ command on the resized partition to verify the size of the file system: 

```shell
[ec2-user@ip-172-31-38-101 ~]$ sudo df -h /dev/xvdf1
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvdf1       50G   53M   47G   1% /mnt/data0
```

Although the size of the partition has changed the size of the file system still remains the same.

2\. Verify which file system you use on the resized partition running __lsblk -fs__ on partition `/dev/xvdf1`:

```shell
$ lsblk -fs /dev/xvdf1
NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
xvdf1  ext4         68ca2d3a-ac44-43f5-b64e-7c91b57a50f0 /mnt/data0
└─xvdf
```

Now continue depending on the shown file system type.

### Extending `ext4` File Systems

If you are using filesystem type __ext4__ step through the following instructions:

3\. Use the __df -h__ command to verify the size of the file system for each volume: 

```shell
$ sudo resize2fs /dev/xvdf1
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/xvdf1 is mounted on /mnt/data0; on-line resizing required
old_desc_blocks = 7, new_desc_blocks = 13
The filesystem on /dev/xvdf1 is now 26214139 blocks long.
``` 

### Extending `xfs` File Systems

If you are using filesystem type __xfs__ step through the following instructions:

3\. Use the __xfs_growfs__ command passing the mounting point of the file system to grow: 

```shell
sudo xfs_growfs -d /mnt/data0
``` 

!!! tip "Install the XFS tools, if necessary"
    Install the XFS tools running `sudo yum install xfsprogs`, if command __xfs_growfs__ should not be
    available on your Linux system.


4\. Check the size of the file systems running __df -h__ again:

```shell
$ df -h /dev/xvdf1
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvdf1       99G   60M   94G   1% /mnt/data0
``` 

Make sure the new size is about 100 GB.