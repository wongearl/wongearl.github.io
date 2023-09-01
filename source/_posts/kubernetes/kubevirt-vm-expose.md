---
title: kubevirt expose vm by svc
date: 2023-09-01 17:36:42
categories:
  - [kubernetes]
tags: kubevirt
---
# 背景
存在kubevit存在的三个虚机：

```
ubuntu-4tlg7   7d22h   Running   True
ubuntu-7kgrk   7d22h   Running   True
ubuntu-94kg2   7d22h   Running   True
```

网络没有做透传，pod也不是underlay网络想要通过NodePort方式暴露虚机22端口进行远程登录。

# 方法
1. 修改vm资源实例，在spec.template.metada下添加labels设置,已存在的则不用添加。例如如下：
```
  spec:
    runStrategy: RerunOnFailure
    template:
      metadata:
        creationTimestamp: null
        labels:
          kubevirtvm01: kubevirtvm01
```
2. 重启虚机

```
virtctl -n wyl-vm restart ubuntu-4tlg7
```

3. 暴露端口
```
virtctl expose vm ubuntu-4tlg7 --name ubuntu-4tlg7-ssh --port 22 --target-port 22 --type NodePort -n wyl-vm
```

4. 查看svc

```
NAME               TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
ubuntu-4tlg7-ssh   NodePort   10.96.23.17   <none>        22:32581/TCP   14m
```
使用节点ip地址加svc nodeport即可访问虚机。
