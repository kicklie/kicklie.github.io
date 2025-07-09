---
title: 在CentOS上配置Samba
date: 2024-04-11 08:38:00 +0800
categories: [OS, CentOS]
tags: [samba]
---

前提：关闭防火墙，关闭SElinux

1. 安装samba
   yum -y samba samba-client samba-common

2. 配置
   cd /etc/samba/  
   mv smb.conf smb.conf.origin  
   vim smb.conf  
   [global]  
   workgroup = workgroup  
   server string = for public   
   security = user  
   map to guest = Bad user  
   log file = /var/log/samba/log.%m  
   max log size = 50  
   [share]  
   path = /share  
   writable = yes  
   browseable = yes  
   guest ok = yes  
   
   保存退出。

3. 创建共享目录
   mkdir /share

4. 启动Samba服务，设置开机启动
   [root@base samba]# systemctl start smb  
   [root@base samba]# systemctl enable smb  

5. 测试
   testparm  
   Load smb config files from /etc/samba/smb.conf  
   rlimit_max: increasing rlimit_max (1024) to minimum Windows limit (16384)  
   Processing section "[share]"  
   Loaded services file OK.  
   Server role: ROLE_STANDALONE  
   Press enter to see a dump of your service definitions  
   Global parameters  
   [global]  
   server string = My Samba Server  
   log file = /var/log/samba/log.%m  
   max log size = 50  
   map to guest = Bad User  
   security = USER  
   idmap config * : backend = tdb  
   [share]  
   path = /share  
   guest ok = Yes  
   read only = No  
   
   然后可以在windows下访问\\\ip，即可看到share文件夹。
   
   [参考](https://www.linuxidc.com/Linux/2017-03/141390.htm)
