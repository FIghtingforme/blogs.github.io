---
layout: post
title: Rsync远程同步文件时的排错流程
date: 2026-03-05
catagories: Rsync
---
### 配置Rsync服务：

#### **Server(10.0.0.5)：**


```
	dnf install rsync -y

	dnf list installed | grep rsync

	vim /etc/rsync.conf
```


```
fake super = yes
uid = rsync
gid = rsync
use chroot = no
max connections = 2000
log file = /var/log/rsyncd.log
read only = false
list = false
ignore errors = yes
hosts allow = 10.0.0.210/24
#hosts deny = 0.0.0.0/24
auth users = rsync_backup
secrets file = /etc/rsyncd.passwd
#####################################
[data]
path = /data
comment = test module	
```

```
useradd -s /sbin/nologin -M rsync     
``` 

#### 创建虚拟用户来启动Rsync进程

```
- echo 'rsync_backup:123456' > /etc/rsyncd.passwd 

- chmod 600 /etc/rsyncd.passwd


- mkdir -p /data/

- chown -R rsync:rsync /data/
	- 创建模块路径对应文件名
	- -R递归更改



- rsync --daemon            

	- 旨在将rsync升级为后台守护进程，监听文件传输，端口为873

- systemctl enable rsyncd

- systemctl start rsyncd

- systemctl status rsyncd
```

##### 本地测试：
```
- touch ./a.txt

- rsync -Paz ./a.txt rsync_backup@10.0.0.5::data
```

#### **Client(10.0.0.210):**
```
- echo '123456' > /etc/client.passwd
	
	- 文件名及位置无硬性要求

- chmod 600 /etc/client.passwd


- touch ./asd.txt

- rsync -Paz ./asd.txt rsync_backup@10.0.0.5::data

- rsync -paz /etc/hostname rsync_backup@10.0.0.5::data --password-file /etc/client.passwd
	- 使用密码文件传输，减少手输密码		
```



```
- /var/log/rsync.log
```

**日志非常重要,可以多开一个窗口查看错误输出**
	
```
- tail -f /var/log/rsync.log
```

**注意：SElinux/firewalld或许会阻止**


