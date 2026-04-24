---

date: 2026-04-18
title: Terraform Docker
layout: post
catagories: terraform

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

### Deploy the `Nginx Image && Countainer`


#### Terraform Block

- The `terraform {}` block contains Terraform settings, including the required providers Terraform will use to provision your infrastructure. For each provider, the `source` attribute defines an optional hostname, a namespace, and the provider type. Terraform installs providers from the [Terraform Registry](https://registry.terraform.io/) by default. In this example configuration, the `docker` provider's source is defined as `kreuzwerker/docker`, which is shorthand for `registry.terraform.io/kreuzwerker/docker`.

#### Providers

- The `provider` block configures the specified provider, in this case `docker`. A provider is a plugin that Terraform uses to create and manage your resources.

- You can use multiple provider blocks in your Terraform configuration to manage resources from different providers. You can even use different providers together. For example, you could pass the Docker image ID to a Kubernetes service.

#### Resources

- Use `resource` blocks to define components of your infrastructure. A resource might be a physical or virtual component such as a Docker container, or it can be a logical resource such as a Heroku application.

- Resource blocks have two strings before the block: the resource type and the resource name. In this example, the first resource type is `docker_image` and the name is `nginx`. The prefix of the type maps to the name of the provider. In the example configuration, Terraform manages the `docker_image` resource with the `docker` provider. Together, the resource type and resource name form a unique ID for the resource. For example, the ID for your Docker image is `docker_image.nginx`.

- Resource blocks contain arguments which you use to configure the resource. Arguments can include things like machine sizes, disk image names, or VPC IDs. Our [providers reference](https://developer.hashicorp.com/terraform/language/providers) documents the required and optional arguments for each resource. For your container, the example configuration sets the Docker image as the image source for your `docker_container` resource.
#### vim ./main.tf

```

terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
      version = "~> 3.0.1"
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
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}


```

- terraform init 时候出现了报错：

```

[devops@lb01 ~/Terraform/learn-terraform-docker-container]$ terraform init
Initializing the backend...
Initializing provider plugins...
- Finding kreuzwerker/docker versions matching "~> 3.0.1"...
╷
│ Error: Failed to query available provider packages
│
│ Could not retrieve the list of available versions for provider
│ kreuzwerker/docker: provider registry.terraform.io/kreuzwerker/docker was not
│ found in any of the search locations

```

- 经分析可能是网络问题，搜寻官方资料了解到 terraform alicloud 可能有延迟，遂在文件（~/.terraformrc) 加入多个镜像确保网络可用：

```

provider_installation {
  network_mirror {
    url = "https://mirrors.aliyun.com/terraform/"
    // Setting alicloud from Alibaba Cloud Mirror Service
    include = ["registry.terraform.io/aliyun/alicloud",
               "registry.terraform.io/hashicorp/alicloud",
              ]
  }
  direct {
    // setting other providers from Terraform Registry
    exclude = ["registry.terraform.io/aliyun/alicloud",
               "registry.terraform.io/hashicorp/alicloud",
              ]
  }
}

```

- 重新执行 terraform init 成功！

- Terraform downloads the `docker` provider and installs it in a hidden subdirectory of your current working directory, named `.terraform`. The `terraform init` command prints out which version of the provider was installed. Terraform also creates a lock file named `.terraform.lock.hcl` which specifies the exact provider versions used, so that you can control when you want to update the providers used for your project.

```

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

```

- terraform plan

```

[devops@lb01 ~/Terraform/learn-terraform-docker-container]$ terraform plan

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
      + must_run                                    = true
      + name                                        = "nginx_tutorials"
      + network_data                                = (known after apply)
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
          + external = 8000
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

```

- terraform apply 

```

docker_image.nginx: Creation complete after 5m18s [id=sha256:6e23479198b998e5e25921dff8455837c7636a67111a04a635cf1bb363d199dcnginx:latest]
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 1s [id=5ba46db57f65e503e117871455a6cf7482b58b8e67faedf5227576a64e55f512]

```

- 创建 `Nginx镜像 && 容器` 完成

- 输入本地8000端口查看nignx网页是否可访问：

```

[devops@lb01 ~/Terraform/learn-terraform-docker-container]$ docker images
                                                            i Info →   U  In Use
IMAGE          ID             DISK USAGE   CONTENT SIZE   EXTRA
nginx:latest   6e23479198b9        237MB         65.8MB    U


[devops@lb01 ~/Terraform/learn-terraform-docker-container]$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS              PORTS                  NAMES
5ba46db57f65   6e23479198b9   "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8000->80/tcp   nginx_tutorials

[devops@lb01 ~/Terraform/learn-terraform-docker-container]$ ss -lntup
Netid  State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port Process
udp    UNCONN  0       0            127.0.0.1:323         0.0.0.0:*
udp    UNCONN  0       0                [::1]:323            [::]:*
tcp    LISTEN  0       4096           0.0.0.0:8000        0.0.0.0:*
tcp    LISTEN  0       511            0.0.0.0:80          0.0.0.0:*
tcp    LISTEN  0       128            0.0.0.0:22          0.0.0.0:*
tcp    LISTEN  0       511               [::]:80             [::]:*
tcp    LISTEN  0       128               [::]:22             [::]:*
tcp    LISTEN  0       32                   *:21                *:*


[devops@lb01 ~/Terraform/learn-terraform-docker-container]$ curl localhost:8000
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

```

#### Notice!!! Format and validate the configuration

- We recommend using consistent formatting in all of your configuration files. The `terraform fmt` command automatically updates configurations in the current directory for readability and consistency.

- Format your configuration. Terraform will print out the names of the files it modified, if any. In this case, your configuration file was already formatted correctly, so Terraform won't return any file names.

```

[devops@lb01 ~/Terraform/learn-terraform-docker-container]$ terraform fmt
main.tf

```


- You can also make sure your configuration is syntactically valid and internally consistent by using the `terraform validate` command.

- Validate your configuration. The example configuration provided above is valid, so Terraform will return a success message.

```

[devops@lb01 ~/Terraform/learn-terraform-docker-container]$ terraform validate
Success! The configuration is valid.

```

- 部署成功！


#### Inspect state

- When you applied your configuration, Terraform wrote data into a file called `terraform.tfstate`. Terraform stores the IDs and properties of the resources it manages in this file, so that it can update or destroy those resources going forward.

- The Terraform state file is the only way Terraform can track which resources it manages, and often contains sensitive information, so you must store your state file securely and restrict access to only trusted team members who need to manage your infrastructure. In production, we recommend [storing your state remotely](https://developer.hashicorp.com/terraform/tutorials/cloud/cloud-migrate) with HCP Terraform or Terraform Enterprise. Terraform also supports several other [remote backends](https://developer.hashicorp.com/terraform/language/backend) you can use to store and manage your state.

- Inspect the current state using `terraform show` :

```

[devops@lb01 ~/Terraform/learn-terraform-docker-container]$ ls
main.tf  terraform.tfstate


[devops@lb01 ~/Terraform/learn-terraform-docker-container]$ terraform show
# docker_container.nginx:
resource "docker_container" "nginx" {
    attach                                      = false
    bridge                                      = null
    command                                     = [
        "nginx",
        "-g",
        "daemon off;",
    ]
    container_read_refresh_timeout_milliseconds = 15000
    cpu_set                                     = null
    cpu_shares                                  = 0
    domainname                                  = null
    entrypoint                                  = [
        "/docker-entrypoint.sh",
    ]
    env                                         = []
    hostname                                    = "5ba46db57f65"
    id                                          = "5ba46db57f65e503e117871455a6cf7482b58b8e67faedf5227576a64e55f512"
    image                                       = "sha256:6e23479198b998e5e25921dff8455837c7636a67111a04a635cf1bb363d199dc"
    init                                        = false
    ipc_mode                                    = "private"
    log_driver                                  = "json-file"
    logs                                        = false
    max_retry_count                             = 0
    memory                                      = 0
    memory_swap                                 = 0
    must_run                                    = true
    name                                        = "nginx_tutorials"
    network_data                                = [
        {
            gateway                   = "172.17.0.1"
            global_ipv6_address       = null
            global_ipv6_prefix_length = 0
            ip_address                = "172.17.0.2"
            ip_prefix_length          = 16
            ipv6_gateway              = null
            mac_address               = "f6:00:a5:5f:98:c0"
            network_name              = "bridge"
        },
    ]
    network_mode                                = "bridge"
    pid_mode                                    = null
    privileged                                  = false
    publish_all_ports                           = false
    read_only                                   = false
    remove_volumes                              = true
    restart                                     = "no"
    rm                                          = false
    runtime                                     = "runc"
    security_opts                               = []
    shm_size                                    = 64
    start                                       = true
    stdin_open                                  = false
    stop_signal                                 = "SIGQUIT"
    stop_timeout                                = 0
    tty                                         = false
    user                                        = null
    userns_mode                                 = null
    wait                                        = false
    wait_timeout                                = 60
    working_dir                                 = null

    ports {
        external = 8000
        internal = 80
        ip       = "0.0.0.0"
        protocol = "tcp"
    }
}

# docker_image.nginx:
resource "docker_image" "nginx" {
    id           = "sha256:6e23479198b998e5e25921dff8455837c7636a67111a04a635cf1bb363d199dcnginx:latest"
    image_id     = "sha256:6e23479198b998e5e25921dff8455837c7636a67111a04a635cf1bb363d199dc"
    keep_locally = false
    name         = "nginx:latest"
    repo_digest  = "nginx@sha256:6e23479198b998e5e25921dff8455837c7636a67111a04a635cf1bb363d199dc"
}

```

- When Terraform created this container, it also gathered the resource's metadata from the Docker provider and wrote the metadata to the state file. Later, you will modify your configuration to reference these values to configure other resources and output values.


#### Manually Managing State

- Terraform has a built-in command called `terraform state` for advanced state management. Use the `list` subcommand to list of the resources in your project's state.

```

[devops@lb01 ~/Terraform/learn-terraform-docker-container]$ terraform state list
docker_container.nginx
docker_image.nginx

```


### Change Infrastructure



