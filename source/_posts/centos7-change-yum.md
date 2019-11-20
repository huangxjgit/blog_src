---
title: Centos7修改国内yum源
date: 2019-11-19 14:08:27
tags: 改源
index_img: /img/centos7.png
categories: Linux
---
[转自]https://blog.csdn.net/kxwinxp/article/details/78578492

查看当前网络连接

```
ip address
```
看到有两个设备：

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue

2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500

这是虚拟机网卡就是ens33了，云服务器的话是enp1s0

```
// 打开有线网配置
vim /etc/sysconfig/network-scripts/ifcfg-enp1s0
// 最后一行，修改为YES
ONBOOT=YES
// 重启网络服务
systemctl restart network.service
```

# 1.更换yum源
```
# 下载wget工具
yum install -y wget
# 进入yum源配置文件所在文件夹
cd /etc/yum.repos.d/
# 备份本地yum源
mv CentOS-Base.repo CentOS-Base.repo_bak
# 获取国内yum源（阿里、163二选一）

wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
# wget -O CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo

# 清理yum缓存 
yum clean all
# 重建缓存 
yum makecache 
# 升级Linux系统
yum -y update 
```
# 2.增加epel源(可选)
```
// 安装epel源
yum install epel-release
// 修改为阿里的epel源
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

// 安装yum源优先级管理工具
yum install -y yum-priorities
// 添加优先级（数字越小优先级越高）
vim /etc/yum.repo.d/epel.repo
priority=88
// 添加优先级（这个数要小于epel里的88即可）
vim /etc/yum.repo.d/Centos-7.repo
priority=6

// 开启yum源优先级功能
vim /etc/yum/pluginconf.d/priorities.conf
// 确保文件内容包含如下：
[main]
enabled=1
```

结束


