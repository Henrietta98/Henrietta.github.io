# Linux 建立软raid实验（raid 5）
有用的网址：https://blog.csdn.net/u012317833/article/details/14052691

简介：
RAID：（Redundant Array of indenpensive Disk） 独立磁盘冗余阵列: 磁盘阵列是把多个磁盘组成一个阵列,当作单一磁盘使用,它将数据以分段或条带(striping)的方式储存在不同的磁盘中,存取数据时,阵列中的相关磁盘一起动作,大幅减低数据的存取时间,同时有更佳的空间利用率。磁盘阵列利用的不同的技术,称为RAID level,不同的level针对不同的系统及应用,以解决数据安全的问题。简单来说，RAID把多个硬盘组合成为一个逻辑扇区，因此，操作系统只会把它当作一个硬盘。
(raid 5:分布式奇偶校验的独立磁盘结构)

RAID可分为软RAID和硬RAID，RAID软是通过软件实现多块硬盘冗余的。
