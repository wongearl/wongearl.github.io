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

docker buildx create --name multi-platform --use --platform linux/amd64,linux/arm64 --driver docker-container --driver-opt network=host --config /etc/buildkit/buildkitd.toml

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
# 4. docker build命令详解
一定要注意docker build工作的路径，否则COPY命令会报：
```
ERROR: failed to solve: failed to compute cache key: failed to calculate checksum of ref w1mhljhg7ccxz3tt6y1ptzwu2::uuxnd1rvq7mmufqmomkx86n2i: "/docs/swagger-ui": not found
```
docker build命令用于根据给定的Dockerfile和上下文以构建Docker镜像。

docker build命令的使用格式：
docker build [OPTIONS] <PATH | URL | ->

1. 常用OPTIONS选项说明
--build-arg，设置构建时的环境变量
--no-cache，默认false。设置该选项，将不使用Build Cache构建镜像
--pull，默认false。设置该选项，总是尝试pull镜像的最新版本
--compress，默认false。设置该选项，将使用gzip压缩构建的上下文
--disable-content-trust，默认true。设置该选项，将对镜像进行验证
--file, -f，Dockerfile的完整路径，默认值为‘PATH/Dockerfile’
--isolation，默认--isolation="default"，即Linux命名空间；其他还有process或hyperv
--label，为生成的镜像设置metadata
--squash，默认false。设置该选项，将新构建出的多个层压缩为一个新层，但是将无法在多个镜像之间共享新层；设置该选项，实际上是创建了新image，同时保留原有image。
--tag, -t，镜像的名字及tag，通常name:tag或者name格式；可以在一次构建中为一个镜像设置多个tag
--network，默认default。设置该选项，Set the networking mode for the RUN instructions during build
--quiet, -q ，默认false。设置该选项，Suppress the build output and print image ID on success
--force-rm，默认false。设置该选项，总是删除掉中间环节的容器
--rm，默认--rm=true，即整个构建过程成功后删除中间环节的容器

2. PATH | URL | -说明
给出命令执行的上下文。
上下文可以是构建执行所在的本地路径PATH，也可以是远程URL，如Git库、tarball或文本文件等，还可以是-。
构建镜像的进程中，可以通过ADD命令将上下文中的任何文件（注意文件必须在上下文中）加入到镜像中。

可以是PATH，如本地当前PATH为.

如果是Git库，如https://github.com/docker/rootfs.git#container:docker，则隐含先执行git clone --depth 1 --recursive，到本地临时目录；然后再将该临时目录发送给构建进程。

-表示通过STDIN给出Dockerfile或上下文。

3. 示例

docker build - < Dockerfile
说明：上述构建过程只有Dockerfile，没有上下文

docker build - < context.tar.gz
说明：其中Dockerfile位于context.tar.gz包中的根路径

docker build -t champagne/myProject:latest -t champagne/myProject:v2.1 .
docker build -f dockerfiles/Dockerfile.debug -t myapp_debug .
