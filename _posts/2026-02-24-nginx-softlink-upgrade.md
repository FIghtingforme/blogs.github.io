---
layout: post
title: Nginx 软链接升级流程
date: 2026-02-24
categories: nginx
---

- 创建nginx软链接 #替换 从而让用户实现无感

- 获取源码：
	- wget http://nginx…… -P /root/test/
- 在/root/test/nginx-1.20下解压
	- tar -zxvf /root/test/nginx-1.20.tar.gz -C /root/test/nginx_1.20/
- 编译源码：
	- 在/root/test/nginx_1.20/nginx1.20 下 ./configure --prefix=/root/test/nginx_1.20
	- make
	- make install
	- 当前目录下可看到configure文件
- 在前一个目录检查：
	- /root/test/nginx-1.24/sbin/nginx -v
- 之前已经创建的软链接：
	- ln -s /root/test/nginx-1.20  /usr/local/nginx
		- 源文件是在源码编译后的目录
		- /usr/local/nginx -> /root/test/nginx-1.20    #当前软链接
		- 确认nginx正在运行：ps -ef | grep nginx
		- ls -l /usr/local/nginx     #软链接指向
- 备份软链接指向的路径 #防止出错回退
	- cp -r /usr/local/nginx  /root/test/nginx-backup-1.20
		- 物理备份
		
- 创建新的源码
- 编译安装
- 建立新的软链接


- 方案A：
	- 直接覆盖原链接（用户不中断）
	- ln -sfn /root/test/nginx-1.22   /usr/local/nginx
		- -f 如果存在旧链接则覆盖
		- -n 处理目标是目录时不进入目录
	- 立即指向新版本
	- 如果新版本异常 #立即回滚
		- ln -sfn /root/test/nginx-1.20_backup /usr/local/nginx
		- /usr/local/nginx/sbin/nginx -s reload

- 方案B：
	- 创建新链接
	- 停nginx/reload后切换
		- systemctl stop nginx
		- mv /usr/local/nginx  /usr/local/nginx_backup
		- mv /usr/local/nginx1.22  /usr/local/nginx
		- systemctl start nginx
