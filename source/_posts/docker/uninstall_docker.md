---
title: uninstall docker
date: 2023-08-23 13:53:22
categories:
  - [docker]
tags: docker
comments: true
---
要在CentOS 7上干净地卸载Docker，可以执行以下步骤：

1. 停止Docker服务：

```
sudo systemctl stop docker
```

2. 移除所有Docker容器和镜像。这将删除所有相关数据，包括容器、镜像以及存储卷等。请注意，这将不可逆转地删除数据。

```
sudo rm -rf /var/lib/docker
```

3. 卸载Docker软件包。可以使用以下命令之一，根据Docker的安装方式选择相应的命令：

- 如果Docker是通过`yum`进行安装的：

```
sudo yum remove docker-ce docker-ce-cli containerd.io
```

- 如果Docker是通过`dnf`进行安装的：

```
sudo dnf remove docker-ce docker-ce-cli containerd.io
```

- 如果Docker是通过RPM包进行手动安装的，可以使用以下命令之一：

```
sudo rpm -e docker-ce docker-ce-cli containerd.io
```

4. 删除相关配置文件：

```
sudo rm -rf /etc/docker
sudo rm -rf /etc/systemd/system/docker.service.d
```

5. 删除用户组和用户（可选）：

```
sudo groupdel docker
sudo userdel docker
```

完成以上步骤后，Docker将被完全卸载。