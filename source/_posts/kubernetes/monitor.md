---
title: metrics/cadvisor,kube-state-metrics,node-exporter
date: 2023-08-18 18:30:53
tags: kubernetes
---
# kubelet的metrics/cadvisor
Kubelet是Kubernetes主节点的一个核心组件，负责管理节点上的容器，以及与主控平面进行通信。Kubelet通过提供不同的接口和嵌入式组件来收集和暴露节点和容器的监控指标。

1. Kubelet监控指标：
   - Kubelet启动的Pod数目
   - Kubelet已经完成的Pod数目
   - Kubelet当前正在运行的Pod数目
   - Kubelet拒绝启动的Pod数目
   - Kubelet处理错误的Pod数目
   - Kubelet未知状态的Pod数目
   - Kubelet容器运行时间
   - Kubelet容器CPU使用率
   - Kubelet容器内存使用率
   - Kubelet存储设备使用率
   - Kubelet网络上行流量
   - Kubelet网络下行流量
   - Kubelet容器磁盘使用量
   - Kubelet容器文件系统使用率
   - Kubelet容器日志记录量

2. cAdvisor监控指标：
   - 容器的CPU使用率
   - 容器的内存使用率
   - 容器的磁盘使用率
   - 容器的网络上行流量
   - 容器的网络下行流量
   - 容器的文件系统使用率
   - 容器的日志记录量
   - 容器的进程数
   - 容器的打开文件数
   - 容器的线程数
   - 容器的磁盘I/O使用率
   - 容器的网络延迟
   - 容器的网络吞吐量
   - 容器的内存压缩率
   - 容器的内存丢失
   - 容器的CPU限制与请求

总体来说，Kubelet和cAdvisor提供了丰富的监控指标，可以用于监视节点和容器的资源使用情况、运行状态及性能状况。这些指标对于在Kubernetes集群中管理和优化容器化应用程序的性能和可靠性非常有帮助。
# kube-state-metrics
kube-state-metrics（KSM）是一个用于将Kubernetes集群的状态信息转换为Prometheus指标的开源项目。它可以提供丰富的监控指标，用于监控Kubernetes集群中的各种资源和对象。以下是Kube-state-metrics提供的一些主要监控指标：

1. 节点指标（Node Metrics）：包括节点的CPU利用率、内存利用率、磁盘空间利用率等信息。

2. Pod指标（Pod Metrics）：包括Pod的CPU利用率、内存利用率、网络流量等信息。

3. 命名空间指标（Namespace Metrics）：包括命名空间中的Pod、Replication Controller、Deployment、DaemonSet等资源的数量和状态信息。

4. 服务指标（Service Metrics）：包括服务的连接数、请求流量、响应时间等信息。

5. 部署指标（Deployment Metrics）：包括部署的副本数量、可用副本数量、滚动更新状态等信息。

6. 容器指标（Container Metrics）：包括容器的CPU利用率、内存利用率、文件系统使用情况等信息。

7. StatefulSet指标（StatefulSet Metrics）：包括StatefulSet的副本数量、可用副本数量、当前状态等信息。

8. 守护进程指标（DaemonSet Metrics）：包括DaemonSet的副本数量、可用副本数量、当前状态等信息。

9. 任务指标（Job Metrics）：包括任务的运行状态、副本数量、成功和失败的次数等信息。

这些指标可以提供关于Kubernetes集群和其中资源的性能、状态和健康状况的详细信息。使用这些指标，可以进行实时监控、性能优化、故障排除和容量规划，以确保集群的稳定性和可靠性。
# node-exporter
Node Exporter 是一种用于 Prometheus 的开源代理，用于暴露各种系统级监控指标。它可以在 Linux 系统上工作，并提供以下类型的监控指标：

1. 系统指标：包括 CPU 使用率、内存使用率、磁盘使用率、磁盘 I/O 情况、网络流量、文件系统使用率等。这些指标可以帮助管理员了解系统的整体状态和资源利用情况。

2. 进程指标：可以获取正在运行的进程数、进程CPU和内存使用情况、进程网络连接数等信息。通过这些指标，可以监控和识别系统中资源占用较多的进程，从而及时调整和优化。

3. 网络指标：包括网络接口的带宽利用率、传输速率、丢包率和错误率等。这些指标可以帮助了解网络流量情况，监控网络性能和及时发现问题。

4. 磁盘指标：包括磁盘使用率、磁盘读写速度、磁盘IO等。这些指标可以帮助监控磁盘的健康状况、数据读写速度和IO性能。

5. 内存指标：包括内存使用量、内存交换情况、内存分页等。这些指标可以帮助了解内存的使用情况和性能。

6. CPU 指标：包括 CPU 使用率、CPU 温度、CPU 核心数等。这些指标可以帮助监控系统的负载情况和CPU性能。

7. 运行时间指标：包括系统的运行时间以及系统启动后的负载状况。这些指标可以帮助了解系统的稳定性和运行时间。

除了以上列举的指标，Node Exporter 还提供了其他许多监控指标，以及一些自定义指标的扩展方式。用户可以根据需要选择性地监控和收集这些指标，以满足对系统性能和资源利用的需求。