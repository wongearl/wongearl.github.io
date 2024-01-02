---
title: 使用buildx构建多架构镜像
date: 2024-01-02 15:52:06
categories:
- [kubernetes]
tags: buildx
---

# 1. 前置条件

- docker 19.03以上版本
- ubuntu 22.04
  
  # 2. 安装相关组件
  
  ## 2.1 安装docker
  
```
  sudo apt-get update
  sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

  curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

  sudo apt-key fingerprint 0EBFCD88

  sudo add-apt-repository \
    "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/ \
    $(lsb_release -cs) \
    stable"

  sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## 2.2 安装docker buildx
```

wget https://mirror.ghproxy.com//https://github.com/docker/buildx/releases/download/v0.12.0/buildx-v0.12.0.linux-amd64
mv buildx-v0.12.0.linux-amd64 docker-buildx
mv docker-buildx ~/.docker/cli-plugins/docker-buildx
chmod +x ~/.docker/cli-plugins/docker-buildx
docker buildx version

```
# 3. 安装配置模拟器
```

docker run --rm --privileged multiarch/qemu-user-static --reset -p yes

docker buildx create --name multi-platform --use --platform linux/amd64,linux/arm64 --driver docker-container --driver-opt network=host network=host --config /etc/buildkit/buildkitd.toml

```
> 如果私有仓库是http没有证书的情况下需要指定--driver-opt network=host 和 --config /etc/buildkit/buildkitd.toml 两个参数，另外需要在/etc/docker/daemon.json中添加"insecure-registries": ["image.xxxxxx.com"]
其中buildkitd.toml内容为：
```

debug = true

# insecure-entitlements allows insecure entitlements, disabled by default.

insecure-entitlements = [ "network.host", "security.insecure" ]

# optionally mirror configuration can be done by defining it as a registry.

[registry."image.xxxxxx.com"]
 http = true
 insecure = true
```