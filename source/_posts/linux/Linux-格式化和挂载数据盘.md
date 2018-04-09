---
title: Linux 格式化和挂载数据盘
date: 2018-04-09 09:56:32
tags:
  - linux
  - fdisk
categories:
  - linux
---

## 分区

1. 运行 `fdisk -l` 命令查看实例是否有数据盘。如果执行命令后，没有发现 `/dev/vdb` ，表示您的实例没有数据盘，无需格式化数据盘，请忽略本文后续内容。

  - 如果您的数据盘显示的是 `dev/xvd?`，表示您使用的是非 `I/O` 优化实例。
  - 其中 ? 是 `a−z` 的任一个字母。

2. 创建一个单分区数据盘，依次执行以下命令：

  - 运行 `fdisk /dev/vdb` ：对数据盘进行分区。
  - 输入 `n`  并按回车键：创建一个新分区。
  - 输入 `p`  并按回车键：选择主分区。因为创建的是一个单分区数据盘，所以只需要创建主分区。
    - 如果要创建 4 个以上的分区，您应该创建至少一个扩展分区，即选择 `e`。
  - 输入分区编号并按回车键。因为这里仅创建一个分区，可以输入 `1`。
  - 输入第一个可用的扇区编号：按回车键采用默认值 `1`。
  - 输入最后一个扇区编号：因为这里仅创建一个分区，所以按回车键采用默认值。
  - 输入 `wq` 并按回车键，开始分区。

```sh
[root@iZuf6eksh7dbr9g67v6cc7Z /]# fdisk /dev/vdb
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0xb20a5f02.

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): 
Using default response p
Partition number (1-4, default 1): 1
First sector (2048-419430399, default 2048): 
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-419430399, default 419430399): 
Using default value 419430399
Partition 1 of type Linux and of size 200 GiB is set

Command (m for help): wq
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.

```


查看新的分区：运行命令 `fdisk -l` 。如果出现以下信息，说明已经成功创建了新分区 /dev/vdb1。

```sh
[root@iZuf6eksh7dbr9g67v6cc7Z /]# fdisk -l

Disk /dev/vda: 42.9 GB, 42949672960 bytes, 83886080 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x0008d73a

   Device Boot      Start         End      Blocks   Id  System
/dev/vda1   *        2048    83884031    41940992   83  Linux

Disk /dev/vdb: 214.7 GB, 214748364800 bytes, 419430400 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0xb20a5f02

   Device Boot      Start         End      Blocks   Id  System
/dev/vdb1            2048   419430399   209714176   83  Linux

```


## 文件系统

在新分区上创建一个文件系统：运行命令 `mkfs.ext3 /dev/vdb1` 。


- 本示例要创建一个 `ext3` 文件系统。您也可以根据自己的需要，选择创建其他文件系统，例如，如果需要在 `Linux`、`Windows` 和 `Mac` 系统之间共享文件，您可以使用 `mkfs.vfat` 创建 `VFAT` 文件系统。

- 创建文件系统所需时间取决于数据盘大小

```sh
[root@iZuf6eksh7dbr9g67v6cc7Z /]# mkfs.ext3 /dev/vdb1
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
13107200 inodes, 52428544 blocks
2621427 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=4294967296
1600 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
	4096000, 7962624, 11239424, 20480000, 23887872

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done 
```


- （建议）备份 `etc/fstab`：运行命令 `cp /etc/fstab /etc/fstab.bak`

- 向 `/etc/fstab` 写入新分区信息：运行命令 `echo /dev/vdb1 /fdata ext3 defaults 0 0 >> /etc/fstab`

如果需要把数据盘单独挂载到某个文件夹，比如单独用来存放网页，请将以上命令 `/fdata` 替换成所需的挂载点路径。

- 查看 `/etc/fstab` 中的新分区信息, 运行命令 `cat /etc/fstab`


```sh
[root@iZuf6eksh7dbr9g67v6cc7Z /]# cat /etc/fstab

#
# /etc/fstab
# Created by anaconda on Sun Oct 15 15:19:00 2017
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=eb448abb-3012-4d8d-bcde-94434d586a31 /                       ext4    defaults        1 1
/dev/vdb1 /fdata ext3 defaults 0 0

```

- 挂载文件系统：运行命令 `mount /dev/vdb1 /mnt` 。


查看目前磁盘空间和使用情况：运行命令 `df -h`。
如果出现新建文件系统的信息，说明挂载成功，可以使用新的文件系统了。

```sh
[root@iZuf6eksh7dbr9g67v6cc7Z /]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        40G  6.9G   31G  19% /
devtmpfs        3.9G     0  3.9G   0% /dev
tmpfs           3.9G     0  3.9G   0% /dev/shm
tmpfs           3.9G   62M  3.8G   2% /run
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
tmpfs           783M     0  783M   0% /run/user/0
overlay          40G  6.9G   31G  19% /var/lib/docker/overlay2/7c73ef02aa155cb0106db23938309119e5ec8820e55ddccc8dc24f0bb4f0cd90/merged
shm              64M     0   64M   0% /var/lib/docker/containers/d82300097547f9657e0700a1f9bd48081beb9597e1aee00582c625f3947c66bb/shm
overlay          40G  6.9G   31G  19% /var/lib/docker/overlay2/24bd611663e7da9ee8b9628c02d40931f11fa90b78b59ef2905536e3f42692e7/merged
shm              64M     0   64M   0% /var/lib/docker/containers/bb827a281e5db495f2350318f6a04b0b3a429d0aabfbe27bb1c78557ba5f2400/shm
overlay          40G  6.9G   31G  19% /var/lib/docker/overlay2/ab34d0522164f53e0eb053346b33de271c654651308d30482713592c11657c53/merged
shm              64M     0   64M   0% /var/lib/docker/containers/4fad0235b222042d460ba7c1c0e54547ffbe4762d97a7167edcd0d40bdf85866/shm
overlay          40G  6.9G   31G  19% /var/lib/docker/overlay2/d4bc93bc057c04471e7fe5e378f9405acb9b2c93a915dc4e44bc1e0710182397/merged
shm              64M     0   64M   0% /var/lib/docker/containers/3838d0849069dcb2897925277e02531755d3a247e3e7b958a15757651afc21c7/shm
/dev/vdb1       197G   60M  187G   1% /mnt
```