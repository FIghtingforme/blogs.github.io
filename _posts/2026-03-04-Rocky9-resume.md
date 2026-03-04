---
layout: post
title: Rocky9忘记密码--紧急救援
date: 2026-03-04
catagories: system
---

### 5分钟紧急救援RHEL-Root密码
#### 原理：在系统未完全启动强行中断引导程序（此时未加载密码验证模块），进入内核的紧急救援模式，从而越过密码

#### 步骤：

- 重启时出现内核选项前迅速上下按键停止倒计时，按下e进入编辑

- 找到以 linux 开头且在 initrd 前输入 rd.break；然后按下Ctrl+X进入紧急救援

- 随后进入到switch_root:/# 下，依次输入以下5个命令：
	
	- mount -o remount,rw /sysroot       重新挂载根目录可读写
	
	- chroot /sysroot                               切换到系统根目录（真实root）
		- 提示符会变成sh-4.2#
	
	- passwd root                                    修改root密码

	- touch /.autorelabel            

        - 自动重新标记，**这一步非常重要**

		- 因为RHEL的SElinux安全机制，修改密码文件会导致安全标签错乱，这段命令为了让系统下次重启时重新认领这些文件，不用担心文件在重启后会自动删除

	- exit                                                 
        退出chroot环境
        
	- exit                                                 
        - 退出紧急救援模式

		- 随后系统开始自动重启
		