---

title: "Github Pages绑定域名"
date: 2026-03-12
categories: Github

---

### Github Pages绑定域名并用cloudFlare加速

```
- 假设域名为：
	
	- github.fighting.top
	
	- 需要在github ==> Settings ==> pages ==> 添加域名：github.fighting.top
	- 
		- 随即官方会给您发需要在您的域名厂商那添加的： record 和 txt记录；用于身份验证
	
	- 你需要提前在cloudFlare托管好自己的根域名，并且添加需要绑定Pages的三级域名（github.fighting.top)
	
		- 三级域名添加：
		- 
			- 用CNAME记录，加github前缀，后面写用户名.github.io
			- 注意：先不开小黄云
			
		- 再添加一个TXT类型，用于身份验证：
		- 
			- 提供官方给的信息：
			- 
			- CNAME          record            number 
		
	- 两个添加完成后需要等github官方授权，或者自己可以先用绑定的域名查看网站要用http协议，因为https需要等dns验证完后在仓库的setting选项栏点强制https后才生效
	- 
	- 有的浏览器会有cookie等信息记录强制指定到https导致网页打不开，这时候需要使用无痕页面查看网站
	- 
	- 注意：Pages绑定域名后需要在config_yaml中把baseurl和url修改；否则会指向错误页面
	
	- 一段时间后可以在github的仓库中的setting和头像页的setting查看dns检查情况，成功验证后即可勾选https和dns的verity

```