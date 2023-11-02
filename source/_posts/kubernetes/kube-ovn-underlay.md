---
title: kubeovn underlay 相关概念
date: 2023-11-02 10:21:20
  - [kubernetes]
tags: kubeovn
---
在计算机网络中，Provider Network（提供商网络）是一个由服务提供商或网络管理员管理的网络，它为用户提供网络连接和服务。Provider Network通常包含多个VLAN（虚拟局域网）和Subnet（子网）。

VLAN是一种逻辑上的局域网，它可以将物理网络中的设备划分为不同的逻辑组，以便进行管理和访问控制。在Provider Network中，VLAN通常用于将用户设备划分为不同的逻辑子网，以便提供更好的隔离和管理。

Subnet是IP网络的一部分，它使用子网掩码来划分网络地址空间。Subnet可以进一步划分为更小的网络段，以便更好地管理和控制网络流量。在Provider Network中，Subnet通常用于将不同的用户设备分配到不同的IP子网中，以便进行路由和访问控制。

Provider Network、VLAN和Subnet之间的关系和作用如下：

Provider Network是整个网络的框架，它提供了用户设备与外部网络或服务之间的连接。
VLAN在Provider Network中起到了逻辑隔离的作用，它将用户设备划分为不同的逻辑子网，以便更好地管理和控制网络流量。
Subnet是IP网络的一部分，它在Provider Network中定义了IP地址的分配范围和子网掩码。Subnet可以进一步划分为更小的网络段，以便更好地管理和控制网络流量。
VLAN和Subnet之间存在一定的关联。一个VLAN可以包含多个Subnet，每个Subnet都是IP网络的一部分。在Provider Network中，VLAN和Subnet的组合使用可以更好地管理网络流量和控制访问权限。
总之，Provider Network、VLAN和Subnet之间的关系是相互依存的。Provider Network提供了整个网络的框架和连接能力，VLAN起到了逻辑隔离的作用，而Subnet则定义了IP地址的分配范围和子网掩码。这些技术的组合使用可以提供更好的网络管理和控制能力。