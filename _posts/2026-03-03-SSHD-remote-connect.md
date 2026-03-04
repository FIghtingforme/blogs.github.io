---
layout: post
title: SSHD 远程工具连接失败
date: 2026-03-03
catagories: SSHD 服务
---

#### 关闭SElinux:
- getenforce        
	查看SEllinux

- setenforce 0	    
	临时关闭测试

#### 关闭防火墙:
		   systemctl stop firewalld	      

#### 重启网卡:
		 	systemctl restart NetworkManager             

#### 查看ip:
- 输入:ip a           
	- 若ip更改则在shell输入nmtui 命令编辑

#### kali系统下:
	powershell 下 ssh -v root@192.168.1.104             #输出详细模式,用于调试
		提示:The fingerprint for the ECDSA key sent by the remote host is
		      Host key verification failed.
		      
		解决方法:
		ssh-keygen -R 192.168.1.104
			自动删除旧指纹