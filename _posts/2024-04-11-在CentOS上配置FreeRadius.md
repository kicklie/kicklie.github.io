---
title: 在CentOS上配置FreeRadius
date: 2024-04-11 08:41:00 +0800
categories: [OS, CentOS]
tags: [freeradius]
---

前提：已经安装好CentOS，系统能够访问互联网，且yum源已配置好。（默认情况下，yum无需配置即可使用）
1.安装freeradius  
yum list | grep radius  
yum -y install freeradius  

2.安装freeradius-utils  
yum -y install freeradius-utils  
配置文件在目录/etc/raddb/  

3.启动Radius Sever  
\#radius -X  
.................................................................  
Listening on auth address * port 1812 bound to server default  
Listening on acct address * port 1813 bound to server default  
Listening on auth address :: port 1812 bound to server default  
Listening on acct address :: port 1813 bound to server default  
Listening on auth address 127.0.0.1 port 18120 bound to server inner-tunnel  
Listening on proxy address * port 56992  
Listening on proxy address :: port 48900  
Ready to process requests  
If the output says Ready to process requests, then all is well.  

4.初步测试  
Edit the users file (in v3 this has been moved to raddb/mods-config/files/authorize), and add the following line of text at the top of the file, before anything else:  
testing Cleartext-Password := "password123"  
Start the server in debugging mode (radiusd -X), and run radtest from another terminal window:  
$ radtest testing password123 127.0.0.1 0 testing123  
If you do see an Access-Accept, then congratulations, the following authentication methods now work for the testing user:  
PAP, CHAP, MS-CHAPv1, MS-CHAPv2, PEAP, EAP-TTLS, EAP-GTC, EAP-MD5.  

5.添加客户端  
Edit the clients.conf file and add the following content:  
/添加单个子网/  
client new-1 {  
ipaddr = 10.6.161.0/24  
secret = 12345678  
}  
/添加单个主机/  
client new-2 {  
ipaddr = 192.168.2.100  
secret = testpassword  
}  

The client should also be configured to talk to the RADIUS server, by using the IP address of the machine running the RADIUS server. The client must use the same secret as configured above in the client section.

[参考](https://wiki.freeradius.org/guide/Getting%20Started)