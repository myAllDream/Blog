---
title: CentOS7设置阿里镜像
date: 2018-07-12 13:05:36
categories: "Linux"
tags:
---

## 1. 备份原来的yum源

```bash
sudo cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak 
```

## 2.设置aliyun的yum源

```sh
sudo wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo 
```

## 3.添加EPEL源

EPEL是由 Fedora 社区打造，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目。装上 EPEL后，可以像在 Fedora 上一样，可以通过 yum install package-name，安装更多软件。

```shell
sudo wget -P /etc/yum.repos.d/ http://mirrors.aliyun.com/repo/epel-7.repo 
```

## 4.清理缓存并生成新的缓存

```shell
sudo yum clean all  
sudo yum makecache  
```

## 5.这时候再更新系统就会看到以下mirrors.aliyun.com信息 

```sh
[root@localhost ~]# yum -y update
已加载插件：fastestmirror, refresh-packagekit, security
设置更新进程Loading mirror speeds from cached hostfile
* base: mirrors.aliyun.com
* extras: mirrors.aliyun.com
* updates: mirrors.aliyun.com
```

