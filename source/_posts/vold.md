---
title: Vold
date: 2018-04-24 18:08:09
tags: work
---

### 需求

最近一直在做硬盘格式化的功能，有一天，突然有一个人找我说，我的硬盘插上去没反应了。。。，那咋办，查呀。发现硬盘不能挂载，然后接着查呀，发现硬盘挂载时候出错了。

```
04-23 11:04:27.367  3890  3932 V vold    : /dev/block/vold/public:8,2: UUID="3337771922705DC0" LABEL="h1" TYPE="hfsplus" 
04-23 11:04:27.367  3890  3932 V vold    : 
04-23 11:04:27.367  3890  3932 W vold    :  doMount, diskId is disk:8,0, mFsLabel is h1
04-23 11:04:27.367  3890  3932 W vold    : mFsLabel is  setLabel OK
04-23 11:04:27.367  3890  3932 W vold    : skipping hfsplus check
04-23 11:04:27.368  4255  4292 D VoldConnector: RCV <- {652 public:8,2 hfsplus}
04-23 11:04:27.368  3890  3932 I vold    :  deviceType is hfsplus try to mount  mDevPath /dev/block/vold/public:8,2
04-23 11:04:27.368  3890  3932 I vold    : flags 142ownerUid 1023 ownerGid 1023
04-23 11:04:27.368  4255  4292 D VoldConnector: RCV <- {653 public:8,2 3337771922705DC0}
04-23 11:04:27.368  3890  3932 E vold    : /dev/block/vold/public:8,2 failed to mount via hfs+
04-23 11:04:27.368  3890  3932 E vold    : public:8,2 failed to mount /dev/block/vold/public:8,2: No such device
```

发现文件系统是HFS+，这是啥东西呢？一顿百度之后发现是苹果电脑使用的文件系统，那就支持一下吧～

#### 准备工作

因为MBR分区表不支持2T以上的硬盘，所以目前格式化硬盘的分区表是GPT，但出于兼容性考虑，硬盘的第一个扇区仍然用作MBR，之后才是GPT头

1. EFI(Extensible Firmware Interface 可扩展固件接口)

由于BIOS发展缓慢，英特尔在安藤处理器上推出EFI技术，它突破传统16位代码的寻址能力，达到处理器的最大寻址。它利用加载EFI驱动的形式，识别及操作硬件，不同于BIOS利用挂载实模式中断的方式增加硬件功能，后者必须将一段类似于驱动的16位代码，放置在固定的0x000C0000至0x000DFFFF之间存储区中，运行这段代码的初始化部分，它将挂载实模式下约定的中断向量向其他程序提供服务

2. GPT

HFS+分区的GUID为48465300-0000-11AA-AA11-00306543ECAC

#### 开始动手

动手查找系统挂载磁盘的方法在/system/vold/下，发现PublicVolume.cpp下没有对HFS+文件系统的支持，但是却存在Hfsplus.cpp,所以，手动改一下

```
status_t checkStatus = -1;
    if (mFsType == "vfat") {
        checkStatus = vfat::Check(mDevPath);
    } else if (mFsType == "ntfs") {
        checkStatus = ntfs::Check(mDevPath.c_str());
    } else if (mFsType == "exfat") {
        checkStatus = exfat::Check(mDevPath.c_str());
    } else if (!strncmp(mFsType.c_str(), "ext", 3)) {
        // ext2/3/4 check later
        checkStatus = 0;
    } else if (mFsType == "hfs" || mFsType == "hfsplus") {
        checkStatus = hfsplus::Check(mDevPath.c_str());
    } else if (mFsType == "iso9660" || mFsType == "udf") {
        // iso needn't check
        checkStatus = iso9660::Check(mDevPath.c_str());
    }
```

在Hfsplus.cpp中，执行mount方法，会调用系统的方法，传入文件系统路径、挂载点、文件系统类型和标志等

```
rc = mount(fsPath, mountPoint, "hfsplus", flags, mountData);
```

在common/fs/namespace.c中，执行sys_mount()–>do_mount()–>do_new_mount()–>do_kern_mount()–>vfs_kern_mount(),一路从用户态查到了内核代码，在加了一堆log、printf和Kprint之后，发现代码停在了do_new_mount()里的get_fs_type中，接着查找代码，在common/fs/filesystems.c中，打印系统支持的文件系统类型

```
static struct file_system_type **find_filesystem(const char *name, unsigned len)
{
	struct file_system_type **p;
	for (p=&file_systems; *p; p=&(*p)->next) {
		printk(KERN_WARNING "%s: file_systems : %s\n",__func__, (*p)->name);
		if (strlen((*p)->name) == len &&
		    strncmp((*p)->name, name, len) == 0)
			break;
	}
	return p;
}
```

```
[  108.449365@2] find_filesystem: file_systems : sysfs
[  108.449369@2] find_filesystem: file_systems : rootfs
[  108.449373@2] find_filesystem: file_systems : ramfs
[  108.449377@2] find_filesystem: file_systems : bdev
[  108.449381@2] find_filesystem: file_systems : proc
[  108.449385@2] find_filesystem: file_systems : cgroup
[  108.449388@2] find_filesystem: file_systems : tmpfs
[  108.449392@2] find_filesystem: file_systems : binfmt_misc
[  108.449396@2] find_filesystem: file_systems : debugfs
[  108.449401@2] find_filesystem: file_systems : securityfs
[  108.449405@2] find_filesystem: file_systems : sockfs
[  108.449409@2] find_filesystem: file_systems : pipefs
[  108.449413@2] find_filesystem: file_systems : rpc_pipefs
[  108.449416@2] find_filesystem: file_systems : configfs
[  108.449420@2] find_filesystem: file_systems : devpts
[  108.449424@2] find_filesystem: file_systems : ext3
[  108.449428@2] find_filesystem: file_systems : ext2
[  108.449432@2] find_filesystem: file_systems : ext4
[  108.449436@2] find_filesystem: file_systems : cramfs
[  108.449439@2] find_filesystem: file_systems : hugetlbfs
[  108.449443@2] find_filesystem: file_systems : vfat
[  108.449447@2] find_filesystem: file_systems : msdos
[  108.449451@2] find_filesystem: file_systems : exfat
[  108.449455@2] find_filesystem: file_systems : iso9660
[  108.449459@2] find_filesystem: file_systems : ecryptfs
[  108.449463@2] find_filesystem: file_systems : nfs
[  108.449466@2] find_filesystem: file_systems : nfs4
[  108.449470@2] find_filesystem: file_systems : ntfs
[  108.449474@2] find_filesystem: file_systems : jffs2
[  108.449478@2] find_filesystem: file_systems : autofs
[  108.449482@2] find_filesystem: file_systems : fuseblk
[  108.449486@2] find_filesystem: file_systems : fuse
[  108.449490@2] find_filesystem: file_systems : fusectl
[  108.449494@2] find_filesystem: file_systems : udf
[  108.449497@2] find_filesystem: file_systems : pstore
[  108.449501@2] find_filesystem: file_systems : mqueue
[  108.449505@2] find_filesystem: file_systems : selinuxfs
[  108.449509@2] find_filesystem: file_systems : mtd_inodefs
[  108.449513@2] find_filesystem: file_systems : functionfs
```

其中，并没有hfsplus这一项。这就头大了，严重怀疑是不是内核系统编译的时候就没添加这一个编译项。
在内核编译fs的Kconfig文件中，添加default y后解决问题

```
config HFSPLUS_FS
	tristate "Apple Extended HFS file system support"
	depends on BLOCK
	select NLS
	select NLS_UTF8
    default y
	help
	  If you say Y here, you will be able to mount extended format
	  Macintosh-formatted hard drive partitions with full read-write access.

	  This file system is often called HFS+ and was introduced with
	  MacOS 8. It includes all Mac specific filesystem data such as
	  data forks and creator codes, but it also has several UNIX
	  style features such as file ownership and permissions.
```

[参考链接](https://blog.csdn.net/yxwmzouzou/article/details/7931326)
