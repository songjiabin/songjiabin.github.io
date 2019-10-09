---
title: centos7 安装 docker
tags: 
- linux
- docker
- go  
copyright: true
categories: linux
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---

![父亲, 小女孩, 爱情, 穆尔, 令人钦佩的, 城市, 间隔, 安全, 恐惧](/images/father-4525302__340.jpg)

<!-- more -->

[链接](<https://docs.docker.com/install/linux/docker-ce/centos/>)

1. 

```golang
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

2. 

```golang
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

3. 

```golang
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

4. 

```golang
yum-config-manager --enable docker-ce-nightly
```

5. 

```golang
yum-config-manager --enable docker-ce-test
```

6. 

```golang
yum-config-manager --disable docker-ce-nightly
```

7. 

```golang
yum install docker-ce docker-ce-cli containerd.io
```

8. 

```golang
systemctl start docker
```

9. 检查是否安装成功

```golang
docker version 
```

