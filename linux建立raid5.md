# Linux 建立软raid实验（raid 5）
有用的网址：https://blog.csdn.net/u012317833/article/details/14052691

简介：
RAID：（Redundant Array of indenpensive Disk） 独立磁盘冗余阵列: 磁盘阵列是把多个磁盘组成一个阵列,当作单一磁盘使用,它将数据以分段或条带(striping)的方式储存在不同的磁盘中,存取数据时,阵列中的相关磁盘一起动作,大幅减低数据的存取时间,同时有更佳的空间利用率。磁盘阵列利用的不同的技术,称为RAID level,不同的level针对不同的系统及应用,以解决数据安全的问题。简单来说，RAID把多个硬盘组合成为一个逻辑扇区，因此，操作系统只会把它当作一个硬盘。
(raid 5:分布式奇偶校验的独立磁盘结构)

RAID可分为软RAID和硬RAID，RAID软是通过软件实现多块硬盘冗余的。
![image](https://github.com/Henrietta98/henrietta98.github.io/blob/master/raid5.jpg)
需要三块或以上硬盘，可以提供热备盘实现故障的恢复；采用奇偶效验，可靠性强，且只有同时损坏两块硬盘时数据才会完全损坏，只损坏一块硬盘时，系统会根据存储的奇偶校验位重建数据，临时提供服务；此时如果有热备盘，系统还会自动在热备盘上重建故障磁盘上的数据；

具体虚拟机操作：
(1)	添加4块硬盘   （添加后，linux重启才能识别）
![image](https://github.com/Henrietta98/henrietta98.github.io/blob/master/2.png)

fdisk -l 命令查看到添加的四块硬盘，如下图所示
![image](https://github.com/Henrietta98/henrietta98.github.io/blob/master/3.png)


(2)	分区
比如对/dev/sdb分区
fdisk /dev/sdb
Command （m for help）： n #按n创建新分区
p primary partition （1-4） #输入p 选择创建主分区
p
Partition number （1-4）： 1 #输入 1 创建第一个主分区
First cylinder （1-130, default 1）： #直接回车，选择分区开始柱面这就从 1 开始
Command （m for help）： w #然后输入w写盘
其它分区照这样做全部分出一个区出来。
 fdisk –l #显示所有分区信息
 
 
(3)	创建RAID
安装mdadm软件 yum install mdadm
mdadm –create /dev/md0 –level=5 -n3 /dev/sdb1 /dev/sdc1 /dev/sdd1
#创建RAID设备名为md0, 级别为RAID 5,采用了三块磁盘，分别是/dev/sdb1 /dev/sdc1 /dev/sdd1
mdadm –detail /dev/md0 #显示设备md0的具体信息


(4)	将/dev/md0创建文件系统
mkfs -t ext4 /dev/md0 #在centos 5.4 可能会出问题，“mkfs.ext4:No such file or directory”
原因是centos 5.4默认不支持ext4文件格式或者是ext4的支持模块默认是没有被加载的，需手动加载。
解决方法：
# modprobe ext4
# modprobe ext4dev
安装e4fsprogs   #yum install e4fsprogs*
然后，重新敲命令：mkfs -t ext4 /dev/md0


(5)	挂载/dev/md0到系统中去，我们实验是否可用
#mkdir /mdadm
#mount /dev/md0 /mdadm/
#cd /mdadm/
#ls
