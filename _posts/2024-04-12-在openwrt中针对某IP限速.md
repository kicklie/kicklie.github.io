---
title: 在openwrt中针对某IP限速
date: 2024-04-12 17:20:00 +0800
categories: [OS, OpenWRT]
tags: [电视盒子, 上传]
---

**前情**：无意中发现电视偷偷上传。<br>

进入后台管理-网络-防火墙-自定义规则中，将下面一大段规则复制进去，并且参考后面的“规则参数说明”进行修改。<br>
**规则组1**：下面两行规则是根据ip限制其上行（上传）的网速<br>
iptables -t filter -I FORWARD -s 192.168.200.102 -j DROP<br>
iptables -t filter -I FORWARD -m limit -s 192.168.200.102 --limit 100/s --limit-burst 100 -j ACCEPT<br>

**规则组2**：下面两行规则是根据ip限制其下行（下载）的网速<br>
iptables -t filter -I FORWARD -d 192.168.200.102 -j DROP<br>
iptables -t filter -I FORWARD -m limit -d 192.168.200.102 --limit 100/s --limit-burst 100 -j ACCEPT<br>
#规则参数说明：<br>
#将192.168.200.102改成需要被限速的ip<br>
#100/s以及brust 100则为限制的速度（两个值可以相同），100的意思可以简单理解为每秒限制一百个数据包，每个数据包大小跟网络设置有关，需要自己尝试不同的值来测出对应的网速，上面楼主贴的图是限制200/s下的网速。<br>
#如果只是需要限制下行，则删掉规则组1；同理若只要限制上行，则删掉规则组2；若两组并存且ip都一样，则会同时限制该ip的上行与下行
#规则组可以配置多个，用来分别限制多个不同的设备。<br>  

#下面规则组是根据mac限制上行网速，也挺实用的，有需要用到的可以拿去使用（使用时记得删掉前面的#注释符号，然后将mac-source后面的mac地址换成你需要限制的设备mac地址）<br>
#iptables -t filter -I FORWARD -m mac --mac-source 3c:bd:3e:1c\:ab:63 -j DROP<br>
#iptables -t filter -I FORWARD -m mac --mac-source 3c:bd:3e:1c\:ab:63 -m limit --limit 100/s --limit-burst 100 -j ACCEPT<br>
补充个细节点：100/s以及brust 100 这两个值分别有什么用处呢？可以简单理解，第一个100就是当下载速度稳定时的网速，而第二个值则是刚开始下载时的速度，假设配置的值是50/s brust 100，那么表现就是设备下行（比如下载时）前几秒的速度可能有200KB/s，然后就降到100KB/s稳定下来，这个200KB/S就是brust 100 决定的，50则是50/s决定的，当然这个具体50/s对应的下载速度多少KB/S需要自己一点点测试。<br>

[原文](https://www.right.com.cn/forum/thread-4056260-1-1.html)

其他回答：<br>
官方的有 luci-app-nft-qos 这个限速插件，汉化包是 luci-i18n-nft-qos-zh-cn。<br>

**问题**：大佬我按你说的添加mac限速规则到防火墙自定义规则之后设备能够限速了，但是我路由器开了定时重启，到时间设备重启之后限速的设备再连上去就失去限速功能了，得手动重启一下防火墙才能恢复设备限速规则，有啥办法解决这个问题吗？<br>
**回答**：<br>
sleep 30<br>
iptables -t filter -I FORWARD -m mac --mac-source FC:7C:02:87:61:13 -j DROP<br>
iptables -t filter -I FORWARD -m mac --mac-source FC:7C:02:87:61:13 -m limit --limit 1/s --limit-burst 1 -j ACCEP<br>
