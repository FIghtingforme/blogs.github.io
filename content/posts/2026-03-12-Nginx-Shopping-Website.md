---

title: "用Nginx搭建站点"
date: 2026-03-12
categories: ["Nginx"]

---


## 一、基础站点配置

### 1. 站点信息

- **域名（server_name）**：`buy.fighting.top`
- **站点根目录（root）**：`/app/code/buy/`
- **首页文件**：`/app/code/buy/index.html`

---

### 2. Nginx 配置

编辑配置文件：


vim /etc/nginx/conf.d/buy.fighting.top.conf

写入：

server {
    listen 80;
    server_name buy.fighting.top;

    root /app/code/buy;

    location / {
        index index.html;
    }

    location /admin/ {
        allow 172.16.1.0/24;
        deny all;
    }
}
3. 关键概念

URL

- http://buy.fighting.top/admin

URI

- /admin/

URL = 域名 + URI
<br>
**Nginx 实际匹配的是 URI**

4. 创建测试文件
mkdir -p /app/code/buy/admin/

echo "购物网站" > /app/code/buy/index.html
echo "admin" > /app/code/buy/admin/index.html
5. 本地测试
curl -H "Host: buy.fighting.top" http://127.0.0.1
curl -H "Host: buy.fighting.top" http://127.0.0.1/admin/
二、进阶：静态资源缓存优化
1. 使用场景

/video

/image/image.1

2. 配置缓存规则
server {
    listen 80;
    server_name buy.fighting.top;

    root /app/code/buy;

    location / {
        index index.html;
    }

    # 静态资源缓存（文本类）
    location ~* \.(html|css|js)$ {
        expires 1d;
    }

    # 图片缓存
    location ~* \.(jpg|png|jpeg|gif|bmp)$ {
        expires 1h;
    }
}
3. 验证缓存

打开浏览器：

按 F12

查看 Network

检查 Cache-Control / Expires

三、location 匹配规则
常见写法
location / {
    # 默认匹配（兜底）
}

location /image/ {
    # 按路径匹配
}

location ~ \.(jpg|jpeg)$ {
    # 正则（区分大小写）
}

location ~* \.(jpg|jpeg)$ {
    # 正则（不区分大小写）
}
匹配优先级（重要）
优先级	类型
1	= 精确匹配
2	^~ 前缀匹配
3	~ / ~* 正则匹配
4	普通前缀（如 /image/）
5	/ 默认
四、请求处理流程
Client 
  ↓
Nginx
  ↓
conf.d/*.conf
  ↓
匹配 server_name
  ↓
匹配 location
  ↓
返回 response

示例：

default.conf      → 默认站点
buy.conf          → buy.fighting.top
五、总结

你现在实现了：

✅ 基础网站部署

✅ 访问控制（admin 限制）

✅ 静态资源缓存

✅ 理解 location 匹配机制

🚀 建议下一步

加 HTTPS（Let's Encrypt）

开启 gzip 压缩

配 CDN（如 Cloudflare）