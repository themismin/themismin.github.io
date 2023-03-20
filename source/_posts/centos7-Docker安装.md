---
title: centos7-Docker安装
tags:
  - CentOS
  - CentOS 7
  - Docker
  - docker-compose
categories:
  - IT
  - 运维
abbrlink: f872f762
date: 2022-11-17 01:17:18
---

### docker 安装
```
yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce docker-ce-cli containerd.io
systemctl start docker
systemctl enable docker
```

### docker-compose 安装
```
### 直接安装
curl -L https://github.com/docker/compose/releases/download/v2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

### 间接安装（网络问题，无法直接安装）
echo "https://github.com/docker/compose/releases/download/v2.12.0/docker-compose-$(uname -s)-$(uname -m)"

### 本地下载好，再上传
rz

### 安装
mv docker-compose-linux-x86_64 /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose -v
```

### 其他
1. 查看容器的挂载目录
```
docker inspect 6a11b0b8291a | grep Mounts -A 20
```

2. 阿里云 docker 镜像加速器
```
https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors


# 配置
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://am3rvw31.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
```

3. 启动/关闭容器
```
docker-composer up -d

docker-composer down
```