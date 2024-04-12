---
title: 禁用IE浏览器IEtoEdge BHO
date: 2024-04-11 09:05:00 +0800
categories: [OS, Windows]
tags: [ie11]
---
  
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Ext\CLSID  

将{1FD49718-1D00-4B19-AF5F-070AF6D5D54C}设置为0. 

[参考](https://www.zabkat.com/blog/remove-edge-bho.htm)