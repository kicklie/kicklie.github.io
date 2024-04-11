---
title: 在CentOS上安装Chrome
date: 2024-04-10 10:56:00 +0800
categories: [OS, CentOS]
tags: [chrome]
---

1. Enable Google YUM repository     
   - Fedora 34/33/32/31/30  
     dnf install fedora-workstation-repositories<br>
     dnf config-manager --set-enabled google-chrome  
   
   - CentOS/RHEL 8.3/7.8  
     \# cat << EOF > /etc/yum.repos.d/google-chrome.repo  
     [google-chrome]  
     name=google-chrome  
     baseurl=http://dl.google.com/linux/chrome/rpm/stable/x86_64  
     enabled=1  
     gpgcheck=1  
     gpgkey=https://dl.google.com/linux/linux_signing_key.pub  
     EOF  

2. Install Google Chrome with YUM
   
   - Fedora 34/33/32/31/30  
     dnf install google-chrome-stable  
   - CentOS/RHEL 8.3  
     dnf install google-chrome-stable  
   - CentOS/RHEL 7.8  
     yum install google-chrome-stable    

[参考](https://www.if-not-true-then-false.com/2010/install-google-chrome-with-yum-on-fedora-red-hat-rhel/)
