---

date: 2026-04-15
title: Docker--- Nodejs
layout: post
catagories: Docker

---


### 目标：Docker 建立 Nodejs 镜像并跑一个 Nodejs 容器

#### Troubleshooting Record


```
[devops@lb01 ~]$ sudo docker build -t nginx-image .

[+] Building 0.1s (1/1) FINISHED                               docker:default
 => [internal] load build definition from Dockerfile                     0.1s
 => => transferring dockerfile: 2B                                       0.0s
ERROR: failed to build: failed to solve: failed to read dockerfile: open Dockerfile: no such file or directory
```
- 需要先创建一个文件/文件夹



```
[devops@lb01 ~]$ docker build -t nginx-image ./docker/nginx-image
ERROR: permission denied while trying to connect to the docker API at unix:///var/run/docker.sock
```

- 没有权限访问 socket 文件：/var/run/docker.sock
	- 检查sock文件权限：660 / 666
	- 将当前用户加入docker组：sudo usermod -aG docker $USER；然后退出重登。




```
[devops@lb01 ~/docker]$ docker build -t nginx-image .
[+] Building 0.0s (1/1) FINISHED                               docker:default
 => [internal] load build definition from Dockerfile                     0.0s
 => => transferring dockerfile: 2B                                       0.0s
ERROR: failed to build: failed to solve: failed to read dockerfile: open Dockerfile: no such file or directory
```
- 显示没有文件/文件夹
- 尝试用文件夹建立dockerfile
- 发现仍然报错，显示文件不存在搜索后发现是dockerfile没有提前建立的关系，于是先建立Dockerfile文件在项目下，再次docker build -t my-nodejs就没有报错了


docker images

- 缓存机制：再次建立镜像会更快


docker run -d -p 127.0.0.1:3000:3000 getting-started

```
[devops@lb01 ~/docker/nodejs-image]$ docker run -p 127.0.0.1:3000:2000 my-nodejs
yarn run v1.22.22
error Couldn't find a package.json file in "/app"
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```


```
[devops@lb01 ~/docker/nodejs-image]$ docker rmi my-nodejs:latest
Error response from daemon: conflict: unable to delete my-nodejs:latest (must be forced) - container 7e6522f10d60 is using its referenced image abb4f558a36f
```

- docker rmi 无效，镜像删不掉的原因：容器占据镜像的分层 / 被打了多个Tag，必须先删除容器再删除镜像。
	
- 查看 && 删除所有容器：docker ps -a 
					docker rm -f $(docker ps -aq)

- 删除镜像  && 查看：docker rmi my-nodejs
					docker images




- 创建缺少的package.json，index.js,修改dockerfile（增加鲁棒性：给yarn.lock* 防止不存在报错）

- 修改dockerfile：
```
FROM node:18-alpine
WORKDIR /app
COPY package.json yarn.lock* .
RUN yarn install --production
COPY . .
EXPOSE 2000
CMD ["yarn", "start"]

```
- 修改index.js
```
console.log("SRE tip: 2000 port started!!!")

```
- 修改package.json:
```
{
  "name": "my-nodejs-app",
  "version": "1.0.0",
  "main":  "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "4.18.2"
  }
}

```

- 重新建立 镜像&&容器：

nodejs container启动成功！！!：
```
[devops@lb01 ~/docker/nodejs-image]$ docker run -p 127.0.0.1:3000:2000 my-nodejs:v2
yarn run v1.22.22
warning package.json: No license field
$ node index.js
SRE 提示：服务已经在2000端口启动
Done in 0.07s.

```

- 显示done说明容器被实时销毁，容器生命周期=PID 1进程生命周期





#### 部署 node 的容器：

- 继续完善，让进程一直处于监听status：


```
在 3000 端口真正能访问到index.js，我们需要把 index.js 改造成一个真正的 **Web 服务器**。

#### 1. 修改本地 index.js（引入 Express）

const express = require('express');
const app = express();
const port = process.env.APP_PORT;

app.get('/', (req, res) => {
  res.send('<h1>你好，Global Remote DevOps!</h1><p>容器运行正常。</p>');
});

app.listen(port, '0.0.0.0', () => {
  console.log(`SRE 监控：服务已在容器内端口 ${port} 启动，并保持监听...`);
});


#### 2. 更新 package.json

确保你的依赖清单里有 express
### 重新构建并运行


#### 重新构建
docker build -t my-nodejs:v3 .

#### 后台运行
docker run -d --name node -e APP_PORT=2026 -p 127.0.0.1:5555:2026 my-node:latest




#### 验证与监控

1. **检查容器状态**：  
    docker ps  
    （确认状态是 Up 而不是 Exited）
    
2. **查看实时日志**：  
    docker logs -f my-web-service  
    （你应该看到“保持监听...”的提示，且不会退出）
    
3. **本地访问测试**：  
    新开一个终端，执行：  
    curl http://127.0.0.1:5555


- 清理缓存： docker system prume


```
[devops@lb01 ~/docker/nodejs-image]$ docker ps
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                                NAMES
81a46b0a9be9   my-node:latest   "docker-entrypoint.s…"   22 seconds ago   Up 22 seconds   2000/tcp, 127.0.0.1:5555->2026/tcp   node
[devops@lb01 ~/docker/nodejs-image]$ docker exec 81a46b0a9be9 netstat -lntup
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:2026            0.0.0.0:*               LISTEN      28/node
```

- 成功实现 nodejs 端口随机访问
- 进入容器：  docker exec -it 81a46b0a9be9 /bin/sh
