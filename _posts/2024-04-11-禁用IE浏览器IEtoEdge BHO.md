---
title: 禁用IE浏览器IEtoEdge BHO
date: 2024-04-11 09:05:00 +0800
categories: [OS, Windows]
tags: [ie11]
---

A quick google search on "IEtoEdge" didn't return any useful results, but it is quite easy to turn it off, as I discoverd after some registry tweaking. You need to locate a registry key called 

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Ext\CLSID

then find {1FD49718-1D00-4B19-AF5F-070AF6D5D54C} (the clsid of IE_TO_EDGE_BHO.DLL) and set its value to zero.

https://www.zabkat.com/blog/remove-edge-bho.htm