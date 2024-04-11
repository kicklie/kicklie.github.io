---
title: Failed to start LSB
date: 2024-04-11 08:48:00 +0800
categories: [OS, CentOS]
tags: [lsb]
---

Failed to start LSB: Bring up/down networking的解决方法。  

此博客记录Centos使用过程中解决问题的思路。
1. 根据提示信息进行操作，查找错误信息。
2. 在系统日志（/var/log/messages）中查看错误信息
3. 根据错误信息进行问题定位并解决

详情：
在某次配置服务器DNS地址的过程中vi编辑文件卡死，只能关闭了putty。随后重启网卡则不能启动。使用命令systemctl status network.service查看网卡状态：
Failed to start LSB: Bring up/down networking  

查看系统日志 cat var/log/messages，日志中提示“设备MAC地址与预期不符”。

使用ip addr命令查看了本机MAC地址，并与/etc/sysconfig/network-scripts/目录中的ifcfg-XXX 中的MAC地址对比（HWADDR字段），发现网卡ens32和ifcfg-ens32中MAC地址相同，但是在此目录下发现多出了一个ifcfg-XXX文件，（ifcfg文件数量和网卡数量相同），删除该文件，重启网卡，启动成功。