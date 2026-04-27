---

date: 2026-03-15
title: "SSH 配置踩坑"
categories: ["SSH"]

---


#### SSH服务---Github 远程连接报错排查
			
```
远程连接到github时出现报错，不能连接

估计是公钥路径不对，git不能在基础目录~/.ssh/ 下看到相关文件

尝试：

	删除原来的公钥，重新建一个在正确目录下

	ssh-keygen -t ed25519 -C "xxx@xxx.com"   
        填自己邮箱，生成ssh密钥

	将公钥内容复制到github中的ssh管理页面中添加公钥

    成功后页面会有提示


- 痛点：用add一个个添加推出文件重新加载是容易重新输入造成记忆压力

- 解决痛点：
- 
	 - 用git bash在 ~/.ssh/ 下创建config路由表以此管理不同密钥

     - 后续管理多个免密钥服务时非常方便，强烈推荐使用！！！
		


用私钥测试是否匹配：

	ssh -T -i ~/.ssh/id_rsa git@github.com

用git bash写在~/.ssh/config的内容：

```
# --- 第 1 把钥匙：专门给个人的 GitHub 用 --- 
Host github.com
	HostName github.com 
	User git 
	# 填入你刚才测试成功的，属于 GitHub 的私钥路径（不要带 .pub） 
	IdentityFile ~/.ssh/id_ed25519
	
Host gitee.com 
	HostName gitee.com 
	User git 
	IdentityFile ~/.ssh/id_rsa_work  
	
Host vps HostName 
	User root 
	Port 22 
	IdentityFile ~/.ssh/id_rsa_vps
```


- 配置完，后续可以直接使用“代号”而不用死记ip：
- 
	- 比如：ssh github.com
	- 
	- 其他服务同理

然后就可以开始在vscode上随意更改代码啦！！！

==注意：每次更改仓库代码时最好先git pull，以免引起冲突==
```
