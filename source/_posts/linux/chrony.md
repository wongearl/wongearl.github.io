---
title: chrony参数及常用命令介绍
date: 2023-10-27 14:38:29
categories:
  - [linux]
tags: chrony
---
​
# 1. 背景
在Kubernetes集群中，如果各节点的时间不一致，可能会带来以下影响：

资源调度问题：Kubernetes使用资源调度器来分配任务和计算资源。如果各个节点的时间不一致，可能会导致调度器无法准确判断任务的截止时间，从而影响任务的执行和资源的分配。
服务一致性问题：对于有状态的服务（如数据库），如果各个节点的时间不一致，可能会导致数据一致性问题。例如，如果在某个节点上写入了一条数据，然后在另一个时间不一致的节点上读取数据，可能会读取到过期的数据。
日志分析问题：在Kubernetes集群中，所有的日志都是按照时间顺序排列的。如果各个节点的时间不一致，可能会导致日志分析出现问题，如时间戳不匹配、事件顺序错误等。
容器同步问题：Kubernetes使用容器来运行任务。如果各个节点的时间不一致，可能会导致容器同步出现问题，如容器启动和停止的时间不一致等。
网络通信问题：Kubernetes集群中的各个节点通过网络进行通信。如果节点之间的时间不一致，可能会导致网络通信出现问题，如消息延迟、丢包等。
监控和报警问题：如果各个节点的时间不一致，可能会导致监控和报警系统出现问题。例如，报警阈值设置错误、监控数据不准确等。
采用chrony进行集群节点间时间同步。

#2. chrony配置文件介绍
ubuntu系统下chrony配置文件路径为/etc/chrony/chrony.conf ，内容如下：
```
# Welcome to the chrony configuration file. See chrony.conf(5) for more
server 192.168.0.206 iburst  #选择集群中一个节点作为服务器，这在集群无法连接外网是保证所有节点时间一直，其余所有节点从该节点获取时间同步。
server ntp.aliyun.com iburst #外网ntp server
server time1.cloud.tencent.com iburst
# information about usuable directives.

# This will use (up to):
# - 4 sources from ntp.ubuntu.com which some are ipv6 enabled
# - 2 sources from 2.ubuntu.pool.ntp.org which is ipv6 enabled as well
# - 1 source from [01].ubuntu.pool.ntp.org each (ipv4 only atm)
# This means by default, up to 6 dual-stack and up to 2 additional IPv4-only
# sources will be used.
# At the same time it retains some protection against one of the entries being
# down (compare to just using one of the lines). See (LP: #1754358) for the
# discussion.
#
# About using servers from the NTP Pool Project in general see (LP: #104525).
# Approved by Ubuntu Technical Board on 2011-02-08.
# See http://www.pool.ntp.org/join.html for more information.
pool ntp.ubuntu.com        iburst maxsources 4
pool 0.ubuntu.pool.ntp.org iburst maxsources 1
pool 1.ubuntu.pool.ntp.org iburst maxsources 1
pool 2.ubuntu.pool.ntp.org iburst maxsources 2

# This directive specify the location of the file containing ID/key pairs for
# NTP authentication.
keyfile /etc/chrony/chrony.keys

# This directive specify the file into which chronyd will store the rate
# information.
driftfile /var/lib/chrony/chrony.drift

# Uncomment the following line to turn logging on.
#log tracking measurements statistics

# Log files location.
logdir /var/log/chrony

# Stop bad estimates upsetting machine clock.
maxupdateskew 100.0

# This directive enables kernel synchronisation (every 11 minutes) of the
# real-time clock. Note that it can’t be used along with the 'rtcfile' directive.
rtcsync

# Step the system clock instead of slewing it if the adjustment is larger than
# one second, but only in the first three clock updates.
makestep 1 3 #此出可以设置为：makestep 3 -1 当误差大于三秒时执行步进调整，而不用等待微调

allow all #允许所有ip访问本时间服务器

local stratum 10 #即使自己未能通过网络时间服务器同步到时间，也允许将本地时间作为标准时间授时给其它客户端
```
# 3. 常用命令

timedatectl set-time "2023-10-27 12:30:50" ：修改时间
chronyc tracking： 服务当前同步状态的快照
chronyc sources -v：查看时间同步源
chronyc makestep：立即执行步进调整
systemctl status chronyd： 查看chronyd服务状态
systemctl restart chronyd: 重启chronyd服务

​