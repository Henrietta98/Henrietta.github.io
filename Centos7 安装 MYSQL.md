# Centos7 安装 MYSQL
准备文件：
1. 官网下载   mysql-8.0.16-linux-glibc2.12-x86_64.tar
2. centos7 虚拟机 （配置好图形界面以及vmvare tools）

## 安装详细步骤：
1. 在物理机上复制 mysql-8.0.16-linux-glibc2.12-x86_64.tar 文件
2. 文件粘贴到虚拟机
3. 运用查找命令找到mysql文件： ***find / -name "*.tar"***

![image](https://github.com/Henrietta98/henrietta98.github.io/blob/master/C_1.png)

4. 进入文件所在目录，解压缩包： ***tar -xvf mysql-8.0.16-linux-glibc2.12-x86_64.tar***

5. #给包重命名为mysql,并安装到/usr/local/目录下
***mv mysql-8.0.16-linux-glibc2.12-x86_64 /usr/local/mysql***

6. #创建mysql用户组

***groupadd mysql***

***useradd -g mysql mysql***

7. #更改所属的组和用户
***chown -R mysql***

***chgrp -R mysql***

8. #进入mysql目录,创建data目录
***cd /usr/local/mysql***

***mkdir data***

***cd /usr/local/mysql/bin***

***./mysqld –initialize***      初始化数据库

![image](https://github.com/Henrietta98/henrietta98.github.io/blob/master/CentOS%2064%20%E4%BD%8D-2019-06-18-20-36-55.png)

9. 在 **/etc/** 下创建 **my.cnf**

***cd /etc/***

***touch my.cnf***

***vim my.cnf***

在配置文件 **my.cnf** 添加配置

![image](https://github.com/Henrietta98/henrietta98.github.io/blob/master/C_3.png)

10. 修改config配置，修改SELINUX=disabled

***vi /etc/selinux/config***

11. 修改mysql目录权限

***chown -R mysql:mysql /usr/local/mysql***

12. 创建软连接(实现可直接命令行执行mysql)

***ln -s /usr/local/mysql/bin/mysql /usr/bin***

13. mysqld配置,拷贝启动文件到/etc/init.d/下并重命令为mysqld

***cp /usr/local/mysql/support-files/mysql.server  /etc/init.d/mysqld***

14. 增加执行权限

***chmod 755 /etc/init.d/mysqld***

15. 检查自启动项列表中没有mysqld

***chkconfig --list mysqld***

#如果没有就添加mysqld

***chkconfig --add mysqld***


16. 设置开机启动

***chkconfig mysqld on***

#启动测试
***service mysqld start***

![image](https://github.com/Henrietta98/henrietta98.github.io/blob/master/C_4.png)

成功启动

### 这是历史命令记录，以后崩了还可以借鉴一下

![image](https://github.com/Henrietta98/henrietta98.github.io/blob/master/C_5.png)
![image](https://github.com/Henrietta98/henrietta98.github.io/blob/master/C_6.png)
