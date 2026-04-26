---
layout: post
title: Docker 
date: 2026-04-21
catagories: Docker
---

### Docker Commands

- docker pull redis:8. 

```
redis:8.0.6-alpine
```

- docker images

```

[devops@lb01 ~/docker/redis]$ docker images
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
my-nodejs:v1         954fdbfce62a        194MB         48.1MB    U
nginx:latest         7f0adca1fc6c        237MB         65.8MB    U
redis:8.0.6-alpine   5f61955be8ab       86.8MB         24.9MB    U

```

- docker run -p 6000:3000 --name -d redis-example redis:8.0.6-alpine

```

[devops@lb01 ~/docker/redis]$ docker run -p 6000:3000 -d --name  redis-example redis:8.0.6-alpine
d7d16d237b5f664fa779edeb6af5b7696ef8b595b76104aa6a7475a27a4ac10c
[devops@lb01 ~/docker/redis]$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED         STATUS         PORTS                                                   NAMES
d7d16d237b5f   redis:8.0.6-alpine   "docker-entrypoint.s…"   8 seconds ago   Up 7 seconds   6379/tcp, 0.0.0.0:6000->3000/tcp, [::]:6000->3000/tcp   redis-example
[devops@lb01 ~/docker/redis]$ ss -lntup
Netid  State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port Process
udp    UNCONN  0       0            127.0.0.1:323         0.0.0.0:*
udp    UNCONN  0       0                [::1]:323            [::]:*
tcp    LISTEN  0       4096           0.0.0.0:6000        0.0.0.0:*
tcp    LISTEN  0       511            0.0.0.0:80          0.0.0.0:*
tcp    LISTEN  0       128            0.0.0.0:22          0.0.0.0:*
tcp    LISTEN  0       4096              [::]:6000           [::]:*
tcp    LISTEN  0       511               [::]:80             [::]:*
tcp    LISTEN  0       128               [::]:22             [::]:*
tcp    LISTEN  0       32                   *:21                *:*

```


- docker exec -it d7d16d237b5f /bin/bash

```

[devops@lb01 ~/docker/redis]$ docker exec -it d7d16d237b5f /bin/bash
OCI runtime exec failed: exec failed: unable to start container process: exec: "/bin/bash": stat /bin/bash: no such file or directory

# 报错后查询 Github issus ，了解到切换到 /bin/sh 后成功运行并进入到容器内
[devops@lb01 ~/docker/redis]$ docker exec -it d7d16d237b5f /bin/sh
/data # ls
/data # pwd
/data

[devops@lb01 ~/docker/redis]$ docker exec -it d7d16d237b5f /bin/sh
/data # cd /
/ # ls
bin    dev    home   media  opt    root   sbin   sys    usr
data   etc    lib    mnt    proc   run    srv    tmp    var
/ # env
HOSTNAME=d7d16d237b5f
SHLVL=1
HOME=/root
OLDPWD=/data
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/
/ #
/ # exit
[devops@lb01 ~/docker/redis]$ docker exec -it d7d16d237b5f /bin/sh
/data # curl
/bin/sh: curl: not found

```

- Difference： docker ps  /  docker  ps  -a

```

[devops@lb01 ~/docker/redis]$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS          PORTS                                                   NAMES
d7d16d237b5f   redis:8.0.6-alpine   "docker-entrypoint.s…"   31 minutes ago   Up 31 minutes   6379/tcp, 0.0.0.0:6000->3000/tcp, [::]:6000->3000/tcp   redis-example
[devops@lb01 ~/docker/redis]$ docker ps -a
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS                      PORTS                                                   NAMES
d7d16d237b5f   redis:8.0.6-alpine   "docker-entrypoint.s…"   31 minutes ago   Up 31 minutes               6379/tcp, 0.0.0.0:6000->3000/tcp, [::]:6000->3000/tcp   redis-example
37fb32f3b1f9   redis:8.0.6-alpine   "docker-entrypoint.s…"   43 minutes ago   Exited (0) 43 minutes ago                                                           redis-1
8ffc7554566c   7f0adca1fc6c         "/docker-entrypoint.…"   31 hours ago     Exited (0) 30 hours ago                                                             sre-tutorial-nginx
e329c77b6133   my-nodejs:v1         "docker-entrypoint.s…"   3 days ago       Exited (1) 30 hours ago                                                             web

```

----------------------------------------------
### Docker Deploy the Demo Project

- Workflow with Docker:

```

Push: js,Mongo DB ==> Git ==> Jenkins: Build JS APP & Create Docker Image ==> Docker Repository( Docker Hub )  

Pull: From "Docker Hub"

```

#### Developing with Containers

- need: JS and Node application, MongoDB Docker Container

- Docker Network: local:3000  ==>  js, index.html  &  Node  &  MongoDB  &  MongoDB Express


#### Step to step:

- docker pull mongo:latest

```

[devops@lb01 ~/docker/redis]$ docker pull mongo:latest
latest: Pulling from library/mongo

```

- docker pull mongo-express:1.0.2-20-alpine3.19

```

[devops@lb01 ~]$ docker pull mongo-express:1.0.2-20-alpine3.19
1.0.2-20-alpine3.19: Pulling from library/mongo-express

```

- docker network ls

```

[devops@lb01 ~/docker/redis]$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
af8fb5214c8a   bridge    bridge    local
94b007c84757   host      host      local
74952d9b5c35   none      null      local

```

- docker network create mongo

```

[devops@lb01 ~]$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
af8fb5214c8a   bridge    bridge    local
94b007c84757   host      host      local
74952d9b5c35   none      null      local
[devops@lb01 ~]$ docker network create mongo
0a4535c250ba9cd8d6cbd47618b687717c9a9ff5df0d93b81f0761ad32af63ea
[devops@lb01 ~]$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
af8fb5214c8a   bridge    bridge    local
94b007c84757   host      host      local
0a4535c250ba   mongo     bridge    local
74952d9b5c35   none      null      local

```

- docker run -p 27017:27017 --name mongo -d mongo:latest

- Configure

```

docker run -d \
-p 27017:27017 \
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
--name mongodb \
--net mongo \
mongo:latest

```



# Continues