---
title: "Docker简单配置"
date: 2026-03-20
categories: ["Docker"]
---

#### Docker配置命令：


```
# 1）先清理冲突包  
dnf remove y podmandocker podman runc  
  
# 2）安装仓库管理工具  
dnf y install dnfpluginscore  
  
# 3）添加 Docker 官方 RHEL 仓库  
dnf configmanager addrepo https://download.docker.com/linux/rhel/dockerce.repo  
  
# 4）安装 Docker  
dnf y install dockerce dockercecli containerd.io dockerbuildxplugin dockercomposeplugin  
  
# 5）启动并设置开机自启  
systemctl enable now docker  
  
# 6） 验证  
docker version  
docker info

这条路径和 Docker 官方给 RHEL 8/9/10 的安装方式一致；Rocky 的官方文档也给出了相同的仓库地址和安装包组合。


# 阶段 1：认识镜像  
docker search nginx  
docker pull nginx  
docker images  
  
# 阶段 2：跑第一个容器  
docker run d name mynginx p 80:80 nginx  
docker ps  
docker logs f mynginx  
docker exec it mynginx bash  
  
# 阶段 3：理解生命周期  
docker stop mynginx  
docker start mynginx  
docker rm f mynginx  
  
# 阶段 4：练习镜像管理  
docker image inspect nginx  
docker image save nginx > nginx.tar  
docker image load i nginx.tar  
docker rmi nginx

```






报错点（环境被污染）：

```
[root@lb01 ~]$ docker version
Client: Docker Engine  Community
 Version:           29.3.0
 API version:       1.54
 Go version:        go1.25.7
 Git commit:        5927d80
 Built:             Thu Mar  5 14:30:26 2026
 OS/Arch:           linux/amd64
 Context:           default
failed to connect to the docker API at unix:///run/podman/podman.sock; check if the path is correct and if the daemon is running: dial unix /run/podman/podman.sock: connect: no such file or directory

```



解决办法：

```
env | grep DOCKER_HOST
unset  DOCKER_HOST
```








报错点(网速慢)：

```
[root@lb01 ~]$ docker search nginx
Error response from daemon: Get "https://index.docker.io/v1/search?q=nginx&n=25": dial tcp 69.63.181.12:443: connect: connection refused

```

##### 解决办法：
###### 配置Docker镜像加速器：

vim /etc/docker/daemon.json

```
{
  "registrymirrors": [
    "https://docker.1panel.live",
    "https://hub.rat.dev",
    "https://dockerproxy.com",
    "https://mirror.ccs.tencentyun.com"
  ]
}
```

##### 终端重启：

```
systemctl daemonreexec  
systemctl restart docker
```

测试成功：

docker pull nginx



