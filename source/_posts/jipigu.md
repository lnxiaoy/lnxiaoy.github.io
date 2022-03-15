---
title: 挂机盘
date: 2018-04-28 21:50:42
tags: work
---

### 挖矿设备鸡屁股

#### HPA(2018.5.15更新)

Host Protected Area 是利用硬盘的ATA指令，在硬盘后部建立一个系统不可访问的区域。HPA在BIOS中和操作系统中均不可见。

```
25: SCSI 600.0: 10600 Disk
  [Created at block.245]
  Unique ID: ADDn.yNUrxjfhAn8
  Parent ID: MZfG.4103QAMnQF9
  SysFS ID: /class/block/sdb
  SysFS BusID: 6:0:0:0
  SysFS Device Link: /devices/pci0000:00/0000:00:14.0/usb1/1-1/1-1:1.0/host6/target6:0:0/6:0:0:0
  Hardware Class: disk
  Model: "General UDisk"
  Vendor: usb 0x048d "General"
  Device: usb 0x1234 "UDisk"
  Revision: "5.00"
  Serial ID: ""
  Driver: "usb-storage", "sd"
  Driver Modules: "usb_storage"
  Device File: /dev/sdb (/dev/sg1)
  Device Files: /dev/sdb, /dev/disk/by-id/usb-General_UDisk-0:0, /dev/disk/by-path/pci-0000:00:14.0-usb-0:1:1.0-scsi-0:0:0:0
  Device Number: block 8:16-8:31 (char 21:1)
  Speed: 480 Mbps
  Module Alias: "usb:v048Dp1234d0100dc00dsc00dp00ic08isc06ip50in00"
  Driver Info #0:
    Driver Status: uas is active
    Driver Activation Cmd: "modprobe uas"
  Driver Info #1:
    Driver Status: usb_storage is active
    Driver Activation Cmd: "modprobe usb_storage"
  Drive status: no medium
  Config Status: cfg=new, avail=yes, need=no, active=unknown
  Attached to: #16 (USB Controller)
```

目前搞不定 没有ATA的工具 访问不了HPA区域 sudo hdparm -NpABC /dev/sdb

```
Caution: invalid backup GPT header, but valid main header; regenerating
backup header from main header.

Warning! Main and backup partition tables differ! Use the 'c' and 'e' options
on the recovery & transformation menu to examine the two tables.

Warning! One or more CRCs don't match. You should repair the disk!

****************************************************************************
Caution: Found protective or hybrid MBR and corrupt GPT. Using GPT, but disk
verification and recovery are STRONGLY recommended.
****************************************************************************
GPT data structures destroyed! You may now partition the disk using fdisk or
other utilities.
The operation has completed successfully.
```

看现象为GPT分区的首部和备份的尾部不对应

```
The backup GPT table is corrupt, but the primary appears OK, so that will be used.
```

这样的u盘格式化后不能挂载

```
/dev/block/vold/public:8,1: UUID="08B4EA8AB4EA7998" LABEL="新加卷" TYPE="ntfs"
Disk identifier: D5C9DB71-8B24-481A-B143-1D127A1C3118

fsType=ntfs fsUuid=08B4EA8AB4EA7998 fsLabel=新加卷 
fsType=ntfs fsUuid=CE74F3FB74F3E45D fsLabel=新加卷 

05-02 18:46:28.661  5707  5707 I Ntfs-3g : Version 2014.2.15 integrated FUSE 27
05-02 18:46:28.661  5707  5707 I Ntfs-3g : Mounted /dev/block/sda1 (Read-Write, label "新加卷", NTFS 3.1)
05-02 18:46:28.661  5707  5707 I Ntfs-3g : Cmdline options: locale=utf8,uid=1023,gid=1023,fmask=7,dmask=7
05-02 18:46:28.661  5707  5707 I Ntfs-3g : Mount options: allow_other,nonempty,relatime,default_permissions,fsname=/dev/block/sda1,blkdev,blksize=4096
05-02 18:46:28.661  5707  5707 I Ntfs-3g : Global ownership and permissions enforced, configuration type 1
05-02 18:46:28.675  5708  5708 W sdcard  : Device explicitly disabled sdcardfs

05-02 18:46:33.714  3891  3903 V vold    : DISK gpt EDF45826-A67A-4554-8B8F-F06C0120DA42
05-02 18:46:33.714  3891  3903 V vold    : 
05-02 18:46:33.714  3891  3903 V vold    : PART 1 EBD0A0A2-B9E5-4433-87C0-68B6B72699C7 8A157F26-9404-4E37-B253-3CE44ADA474E 

05-02 18:46:58.705  3891  3933 E vold    : public:8,1 unsupported filesystem 
05-02 18:46:58.705  4257  4294 D VoldConnector: RCV <- {652 public:8,1 }
05-02 18:46:58.706  4257  4294 D VoldConnector: RCV <- {653 public:8,1 fakeUuid}
05-02 18:46:58.706  4257  4294 D VoldConnector: RCV <- {654 public:8,1 }
05-02 18:46:58.706  4257  4294 D VoldConnector: RCV <- {651 public:8,1 6}
05-02 18:46:58.707  4257  4294 D VoldConnector: RCV <- {400 9 Command failed}

05-02 19:23:16.089  3891  3903 V vold    : DISK gpt D5C9DB71-8B24-481A-B143-1D127A1C3118
05-02 19:23:16.089  3891  3903 V vold    : 
05-02 19:23:16.089  3891  3903 V vold    : PART 1 EBD0A0A2-B9E5-4433-87C0-68B6B72699C7 126F9A8F-DE78-4594-8A1A-BB0B59C988AC Basic data partition
```

#### N1 挂机盘

还有一种格式化之后也可以反复挂载，看起来GPT尾部的分区也被干掉了，但是可以使用，u盘拆开了，也看不出来什么，log也没什么异常。。。。

#### 后记

[参考链接](https://askubuntu.com/questions/633465/the-crc-for-the-main-partition-and-back-up-partition-table-are-invalid)
[参考链接](http://man.linuxde.net/hdparm)
