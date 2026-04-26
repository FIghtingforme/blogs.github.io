---

layout: post
title: Nginx--网站搭建
date: 2026-03-08
catagories: Nginx

---

#### 先挑选一个自己喜欢的云服务器：
##### 阿里云/腾讯云/。。。



```
- cloudFlare配置name server：
	- 常规配置：
		- A策略        二级域名开头           ip               

	- 二级域名可单独配置一个：  
		- A策略          @         ip
	


- ==公网server防火墙一定要放行，否则不显示页面！！！==

	- firewall-cmd --permanent --add-service=http
	
	- firewall-cmd --permanent --add-service=https
	
	- 平缓重载：
		- firewall-cmd --reload
	
	- 最后检查：
		- firewall-cmd --list-all


- Nginx（不同下载方式file/dir可能不同）：
	
	- 主配置文件：
	
			- 修改：/etc/nginx/nginx.conf
				- 

```

```
server {  
	listen 80  
	server_name  **自己的域名**；
	root **网站目录**;  
	location / {             /是基于网站目录结构
	index index.html;    
	}  
	}  
```

```
	
			- 检查配置是否错误：
			
				- nginx -t
			
			- 平滑重载：
			
				- systemctl reload  nginx  
			
			
			- 创建目录：
				
				- mkdir -p /.../
			
			- 创建首页文件：
				
				- touch /.../index.html
				
				- echo "Welcome to my website" >> /.../index.html
			
		
		- 要在两个文件中添加的内容 "你的ip 你的域名"
				
		
		- windows下面win键+r 输入drivers 访问etc下的hosts
			
			- 若提示权限不足可通过hosts文件属性修改administrator权限
		
		
		- linux：  /etc/hosts
		
			
		- 即可访问您的域名
```

注意：如果是自己的真实域名不用改hosts文件，但要注意防火墙放行 && cloudFlare的子域名添加，以及cloudFlare中设置为flexible
