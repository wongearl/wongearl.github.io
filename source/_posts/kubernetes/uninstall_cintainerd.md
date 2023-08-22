---
title: uninstall containerd
date: 2023-08-22 11:39:52
categories:
  - [kubernetes]
tags: containerd
---
要彻底清除 Kubernetes 集群节点上的 containerd，可以按照以下步骤进行：

Step 1: 停止并移除所有运行的容器
```
$ docker stop $(docker ps -aq)
$ docker rm $(docker ps -aq)
```
这将停止并删除节点上所有正在运行的容器。

Step 2: 停止并禁用 containerd 服务
```
$ systemctl stop containerd
$ systemctl disable containerd
```
这将停止并禁用 containerd 服务。

Step 3: 卸载 containerd 软件包
```
$ yum remove containerd -y
```
根据你的系统，可能需要使用适当的软件包管理器（如apt、dnf）来移除 containerd 软件包。

Step 4: 删除 containerd 配置目录
```
$ rm -rf /etc/containerd
```
这将删除 containerd 的配置文件。

Step 5: 删除 containerd 数据目录
```
$ rm -rf /var/lib/containerd
```
这将删除 containerd 的数据目录。

Step 6: 清理 containerd 相关的 iptables 规则
```
$ iptables -t nat -F
$ iptables -t mangle -F
```
这将清除与 containerd 相关的 iptables 规则。

Step 7: 检查是否有残留的 containerd 配置文件
```
$ ls /etc/systemd/system/containerd*
```
如果输出中有任何文件，则手动删除它们：
```
$ rm /etc/systemd/system/containerd*.service
```

现在，你的 Kubernetes 集群节点上的 containerd 已经彻底卸载。