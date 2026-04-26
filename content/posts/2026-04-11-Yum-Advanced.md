---

date: 2026-04-11
title: yum 进阶
layout: post
catagories: system

---

## 探究yum源的混乱


**更换yum源核心逻辑：注释metalink；更换国内的baseurl**

**解决方案：源于yum错乱，为了尽快解决最好的办法就是删除原来错乱的配置重新配置官方的yum源**



### 配置yum的目的：

 #### 直接跨海访问国外会有极高的延迟和丢包率，从而使用国内的镜像加速，本质上是切换到了 **地理位置更近、带宽更高的镜像站**

 #### 高可用：国内大厂的镜像站有极高的负载均衡，保证 **7X24 小时不中断**

 #### 版本一致性：使用私有的企业内部私有源保证服务器安装的 **软件版本一致**



---


### /etc/yum.repos.d/ 文件拆解：


#### 核心系统源（BaseOS， AppStream）：

- rocky.repo

- 包含 linux内核，li，cp，ssh等操作系统生存必须的基础包

- 必须改！！！



#### 官方拓展源 ( Extras, Plus)：

- rocky-extras.repo

- 包含官方支持但不属于核心系统的插件

- 建议改



#### 第三方社区源（ EPEL ）：

- epel.repo

- 由 Federa 社区维护，提供很多红帽系官方不带的工具

- 必须改



---


### repo 文件示例：

```

[epel] 
# 仓库 ID 
name=Extra Packages for Enterprise Linux 9 
# 仓库名称 

# metalink=https://mirrors... 
# 自动寻找最快节点的地址（通常由于网络问题失效） 

baseurl=https://mirrors.aliyun.com/epel/9/Everything/x86_64/ 
# 真正的下载地址 enabled=1 # 是否启用（1为启用） 

gpgcheck=1 # 是否校验签名（为了安全，必须为1） 
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-9 
# 校验用的公钥

```


---



Yum 源案例实践：




First Step --- 删除所有乱七八糟的 .repo 文件和备份文件




```

# 1. 创建一个紧急备份目录（以防万一）
sudo mkdir -p /etc/yum.repos.d/backup_archive

# 2. 将现有所有乱七八糟的文件移走，让目录变空
sudo mv /etc/yum.repos.d/*.repo* /etc/yum.repos.d/backup_archive/

# 3. 清理缓存元数据，确保 dnf 不再记住之前的错误路径
sudo dnf clean all

```






Second Step --- 还原官方默认设置



```

# 强制重装仓库定义包，这会自动在 /etc/yum.repos.d/ 下生成官方原版的 .repo 文件
sudo dnf reinstall rocky-repos rocky-release -y

# 验证官方文件是否回来
ll /etc/yum.repos.d/
# 预期输出：rocky.repo, rocky-addons.repo 等纯净的官方文件

```


- 如果不行需要现在 /etc/yum.repos.d/ 下生成 bootstrap.repo （ 引导 ）

- **因为 dnf 是联网包管理器，删除了所有 /etc/yum.repos.d/ 下的所有repo文件 就不知道去哪个下载站下载**

```

# 直接在命令行输入以下整段命令
sudo tee /etc/yum.repos.d/bootstrap.repo <<EOF
[bootstrap-baseos]
name=Bootstrap BaseOS
baseurl=https://mirrors.aliyun.com/rockylinux/9/BaseOS/x86_64/os/
gpgcheck=0
enabled=1

[bootstrap-appstream]
name=Bootstrap AppStream
baseurl=https://mirrors.aliyun.com/rockylinux/9/AppStream/x86_64/os/
gpgcheck=0
enabled=1
EOF

```








Third Step --- 采用阿里云镜像站进行 "yum 加速"



```

# 使用 sed 批量替换官方地址为阿里云地址
sudo sed -e 's|^mirrorlist=|#mirrorlist=|g' \
         -e 's|^#baseurl=http://dl.rockylinux.org/$contentdir|baseurl=https://mirrors.aliyun.com/rockylinux|g' \
         -i /etc/yum.repos.d/rocky*.repo

# 建立索引记录
sudo dnf makecache



```








Forth Step --- 配置 EPEL 镜像源



```

# 1. 安装 EPEL 官方定义包 / 包未安装上重新安装
sudo dnf install epel-release -y || sudo dnf reinstall epel-release -y

# 2. 将 EPEL 也切换到阿里云镜像，保证下载 Rclone 飞快
sudo sed -e 's|^metalink=|#metalink=|g' \
		-e's|^#baseurl=https\?://download.fedoraproject.org/pub/epel/|baseurl=https://mirrors.aliyun.com/epel/|g' \ 
         -i /etc/yum.repos.d/epel*.repo

# 3. 开启 Rocky 9 必须的 CRB 库（很多 EPEL 包依赖它）
sudo dnf config-manager --set-enabled crb

# 4. 最后一次全量缓存刷新
sudo dnf makecache

```


- epel 用不了可参考阿里文档，一定选和系统适配的 epel：
```
https://developer.aliyun.com/mirror/epel/?spm=a2c6h.25603864.0.0.14959e5axyjIWx
```



Fifth Step --- 验证 yum 是否加速成功


```

# 检查现在的仓库列表，确认状态为 enabled 且路径正确 dnf repolist 
# 尝试安装 rclone 验证闭环 sudo dnf install -y rclone

```



下载rclone时候提示：

```

[devops@lb01 ~]$ sudo dnf makecache
Extra Packages for Enterprise Linux 8 - x86_64  43 kB/s | 4.0 kB     00:00
Rocky Linux 9 - BaseOS                          27 kB/s | 4.3 kB     00:00
Rocky Linux 9 - AppStream                       60 kB/s | 4.8 kB     00:00
Rocky Linux 9 - CRB                             52 kB/s | 4.8 kB     00:00
Rocky Linux 9 - Extras                          18 kB/s | 3.1 kB     00:00
Metadata cache created.

```

可能是 epel 8 系列 和 系统不一致导致系统直接忽略 rclone 包，所以重新安装了新的 epel 9 

```

# 1. 移除错误的 epel-release 包 
sudo dnf remove epel-release -y 

# 2. 清理所有残留的 8 版本缓存 
sudo dnf clean all

# 1. 安装适配 Rocky 9 的 EPEL 定义包
sudo dnf install -y https://mirrors.aliyun.com/epel/epel-release-latest-9.noarch.rpm

# 2. 再次执行镜像加速;这里如果执行不成功可以去 /etc/yum.repos.d/epel.repo 文件看baseurl的前面共同部分再切换到下面
 
sudo sed -e 's|^metalink=|#metalink=|g' \
         -e 's|^#baseurl=https\?://download.fedoraproject.org/pub/epel/|baseurl=https://mirrors.aliyun.com/epel/|g' \
         -i /etc/yum.repos.d/epel*.repo

# 激活 CRB仓库，rclone 在 9 版本中需要基础开发库的支持，被 Rocky 官方放在了 crb仓库
sudo dnf config-manager --set-enabled crb



# 1. 重新建立索引 
sudo dnf makecache 

# 2. 观察输出！确认看到的是 "Extra Packages for Enterprise Linux 9" 

# 3. 安装 
rclone sudo dnf install -y rclone 

# 4. 验证版本 
rclone version


```






rclone 安装成功，最后 epel 源 也是成功显示在仓库里：

```

[devops@lb01 ~]$ dnf repolist
repo id             repo name
appstream           Rocky Linux 9 - AppStream
baseos              Rocky Linux 9 - BaseOS
crb                 Rocky Linux 9 - CRB
epel                Extra Packages for Enterprise Linux 9 - x86_64
epel-cisco-openh264 Extra Packages for Enterprise Linux 9 openh264 (From Cisco) - x86_64
extras              Rocky Linux 9 - Extras

```




后续：私有镜像源管理（ image / binary ）？？


