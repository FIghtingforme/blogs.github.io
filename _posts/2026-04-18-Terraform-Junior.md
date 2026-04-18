---

date: 2026-04-18
title: Terraform
layout: post
catagories: Terraform

---



### 今天安装Terraform的时候突然安装不上的情况，于是开始排查：

```

[devops@lb01 ~/infrastructure/terraform]$ sudo dnf install terraform -y
[sudo] password for devops:
Sorry, try again.
[sudo] password for devops:
Last metadata expiration check: 2:47:45 ago on Tue 14 Apr 2026 01:05:35 PM CST.
No match for argument: terraform


```

- 猜测原因：可能与我之前在rocky上折腾yum清理repo/缓存有关系  

- 出现错误

```
ps://rpm.release.hashicorp.com/RHEL/hashicorp.repo
Adding repo from: 
Curl error (6): Couldn't resolve host name for https://rpm.release.hashicorp.com/RHEL/hashicorp.repo [Could not resolve host: rpm.release.hashicorp.com]
Error: Configuration of repo failed
```

- 网址fat-finger error，应该是`https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo`
#### 开始排查：    根据Terraform官方安装步骤来

```
# Install `yum-config-manager` to manage your repositories.
$ sudo yum install -y yum-utils


# Use `yum-config-manager` to add the official HashiCorp RHEL repository.
# 注意这一步会出现错误，重新建立缓存（sudo dnf makecache）后成功加入到本地yum
# 还有就是rocky中yum已经被dnf代替所以这条命令还需要用yum可以自行安装，我之前已经安装过yum了
$ sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo


# Install Terraform from the new repository.
$ sudo yum -y install terraform

# 输入terraform 显示则安装成功！！！
[devops@lb01 ~]$ terraform
Usage: terraform [global options] <subcommand> [args]

The available commands for execution are listed below.
The primary workflow commands are given first, followed by
less common or more advanced commands.

```


#### Iac ：用 Terraform 控制 Docker 生成 nginx镜像网站并访问


1. 新建一个空文件夹：`mkdir tf-docker-test && cd tf-docker-test`

```
[devops@lb01 ~]$ mkdir ./tf-docker-test && cd ./tf-docker-test
[devops@lb01 ~/tf-docker-test]$ ls
[devops@lb01 ~/tf-docker-test]$ pwd
/home/devops/tf-docker-test
```

2. 新建一个 `main.tf` 文件，输入以下代码（这是 Terraform 控制 Docker 的代码）：

```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "sre-tutorial-nginx"
  ports {
    internal = 80
    external = 8080
  }
}
```

#### 依次敲下三个命令来构建Docker并加载容器：
 
`terraform init`   (这一步初始化花费了蛮长时间大概5-10分钟！好在成功构建镜像和容器)

```
[devops@lb01 ~/tf-docker-test]$ terraform init
Initializing the backend...
Initializing provider plugins...
- Finding kreuzwerker/docker versions matching "~> 3.0"...
- Installing kreuzwerker/docker v3.9.0...
- Installed kreuzwerker/docker v3.9.0 (self-signed, key ID 0DCE698927DAF8EC)
Partner and community providers are signed by their developers.
If you'd like to know more about provider signing, you can read about it here:
https://developer.hashicorp.com/terraform/cli/plugins/signing
Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!
...
```


`terraform plan`

```
# 这条命令用于确认计划
[devops@lb01 ~/tf-docker-test]$ terraform plan

Terraform used the selected providers to generate the following execution plan.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach                                      = false
      + bridge                                      = (known after apply)
      + command                                     = (known after apply)
      + container_logs                              = (known after apply)
      + container_read_refresh_timeout_milliseconds = 15000
      + entrypoint                                  = (known after apply)
      + env                                         = (known after apply)
      + exit_code                                   = (known after apply)
      + hostname                                    = (known after apply)
      + id                                          = (known after apply)
      + image                                       = (known after apply)
      + init                                        = (known after apply)
      + ipc_mode                                    = (known after apply)
      + log_driver                                  = (known after apply)
      + logs                                        = false
      + memory_reservation                          = 0
      + must_run                                    = true
      + name                                        = "sre-tutorial-nginx"
      + network_data                                = (known after apply)
      + network_mode                                = "bridge"
      + read_only                                   = false
      + remove_volumes                              = true
      + restart                                     = "no"
      + rm                                          = false
      + runtime                                     = (known after apply)
      + security_opts                               = (known after apply)
      + shm_size                                    = (known after apply)
      + start                                       = true
      + stdin_open                                  = false
      + stop_signal                                 = (known after apply)
      + stop_timeout                                = (known after apply)
      + tty                                         = false
      + wait                                        = false
      + wait_timeout                                = 60

      + healthcheck (known after apply)

      + labels (known after apply)

      + ports {
          + external = 8080
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id           = (known after apply)
      + image_id     = (known after apply)
      + keep_locally = false
      + name         = "nginx:latest"
      + repo_digest  = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

───────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't
guarantee to take exactly these actions if you run "terraform apply" now.


```


`terraform apply -auto-approve`

```
# 生成镜像和容器，中途会遇到暂停输入“yes”可以获得actions
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

docker_image.nginx: Creating...
docker_image.nginx: Still creating... [00m10s elapsed]
docker_image.nginx: Still creating... [00m20s elapsed]
docker_image.nginx: Still creating... [00m30s elapsed]
docker_image.nginx: Still creating... [00m40s elapsed]
docker_image.nginx: Still creating... [00m50s elapsed]
docker_image.nginx: Still creating... [01m00s elapsed]
docker_image.nginx: Creation complete after 1m1s [id=sha256:7f0adca1fc6c29c8dc49a2e90037a10ba20dc266baaed0988e9fb4d0d8b85ba0nginx:latest]
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 0s [id=8ffc7554566c86148d1873e5c320683e79fb6eebd22cdf8283aa8212237cff46]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.


```


#### 验证：是否能在终端里看到它自动拉取镜像并启动了 Nginx,若能访问80映射的8080端口则成功建立了Docker-Nginx镜像网站

```
[devops@lb01 ~]$ curl localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy,
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>



#####################################################
# 查看 (docker images) 
[devops@lb01 ~]$ docker images
                                                            i Info →   U  In Use
IMAGE          ID             DISK USAGE   CONTENT SIZE   EXTRA
my-nodejs:v1   954fdbfce62a        194MB         48.1MB    U
nginx:latest   7f0adca1fc6c        237MB         65.8MB    U


# 查看docker containers (docker ps -a)
[devops@lb01 ~]$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                NAMES
8ffc7554566c   7f0adca1fc6c   "/docker-entrypoint.…"   17 seconds ago   Up 16 seconds   0.0.0.0:8080->80/tcp                 sre-tutorial-nginx
e329c77b6133   my-nodejs:v1   "docker-entrypoint.s…"   45 hours ago     Up 45 hours     2000/tcp, 127.0.0.1:5555->4444/tcp   web


```


#### 成功完成部署！！！

