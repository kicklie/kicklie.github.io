---
title: 在CentOS上安装VNC
date: 2024-04-10 15:38:00 +0800
categories: [OS, CentOS]
tags: [vnc]
---

yum install tigervnc-server xorg-x11-fonts-Type1 -y  
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service  
vi /etc/systemd/system/vncserver@:1.service   
Non-root user:  
ExecStart=/sbin/runuser -l username -c "/usr/bin/vncserver %i"  
PIDFile=/home/pirat9/.vnc/%H%i.pid  
root user:  
ExecStart=/sbin/runuser -l root -c "/usr/bin/vncserver %i"  
PIDFile=/root/.vnc/%H%i.pid  

###修改分辨率的方法  
ExecStart=/sbin/runuser -l my_user -c "/usr/bin/vncserver %i -geometry 1366x768"

systemctl daemon-reload  
vncpasswd  
systemctl enable vncserver@:1.service  
systemctl start vncserver@:1.service  
firewall-cmd --permanent --add-service vnc-server  
systemctl restart firewalld.service
