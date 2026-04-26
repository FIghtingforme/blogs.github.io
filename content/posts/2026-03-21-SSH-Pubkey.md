---

date: 2026-03-21
title: "SSH安全密钥设置"
catogories: SSH

---


## ==SSH开放高位端口以及免密钥匙登录==



**极度重要提示：在进行以下操作时，请务必全程保持当前 SSH 连接不断开！** 所有的测试请在**新开的终端窗口**中进行。如果配置有误，你可以使用未断开的原窗口进行修改挽救，否则你可能会被彻底锁在服务器外面。

## 以下是完整的操作步骤：

### 第一步：确保你已经配置好了密钥登录

#### 在禁用密码登录之前，你必须确保密钥登录已经可以成功使用。

1. 如果你在本地还没有生成过密钥，请在**本地电脑**终端执行：
    
    
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
    
2. 将公钥上传到服务器（把 user 和 ip 换成你的服务器账号和IP）：
    
    
```
ssh-copy-id user@ip
```
    
3. **测试密钥登录**：新开一个终端，输入 ssh user@ip，如果不需要输入密码就能直接登入，说明密钥配置成功。


---

### 第二步：修改 SSH 配置文件

在服务器上，使用你熟悉的编辑器（如 vim 或 nano）编辑 sshd 服务器端配置文件：


```
sudo vim /etc/ssh/sshd_config
```

找到并修改以下三项内容（如果没有找到该项，或者前面有 # 注释符，请去掉 # 并修改）：

1. **修改端口**：找到 Port 22，将其修改为你想要的的高位端口（建议在 10000 - 65535 之间，例如 50022）：
    
```
Port 50022
```
    
2. **禁用密码登录**：找到 PasswordAuthentication，将其修改为 no：
    
    
```
PasswordAuthentication no
```
    
3. **确认开启密钥登录**：找到 PubkeyAuthentication，确保其为 yes：
    
    codeText
    
```
PubkeyAuthentication yes
```
    

保存并退出编辑器。

---

### 第三步：放行防火墙规则（非常关键）

在重启 SSH 服务之前，必须在防火墙中放行你刚才设置的高位端口，否则你将被拦截在外面。  
根据你的系统，选择对应的防火墙命令：

**如果是 Ubuntu / Debian (UFW):**


```
sudo ufw allow 50022/tcp
sudo ufw reload
```

**如果是 CentOS / RHEL / AlmaLinux (Firewalld):**


```
sudo firewall-cmd --permanent --zone=public --add-port=50022/tcp
sudo firewall-cmd --reload
```

(注意：如果你使用的是阿里云、腾讯云、AWS等云服务器，你还必须登录云服务商的网页控制台，在**安全组规则**中添加一条入方向规则，放行 TCP 协议的 50022 端口)

**如果你的系统开启了 SELinux (通常是 CentOS/RedHat 系):**  
你还需要告诉 SELinux 允许 SSH 绑定这个新端口：


```
sudo semanage port -a -t ssh_port_t -p tcp 50022
```

(如果提示找不到 semanage 命令，请先运行 sudo yum install policycoreutils-python-utils)

---

### 第四步：重启 SSH 服务

应用刚才修改的配置：

**Ubuntu / Debian:**


```
sudo systemctl restart ssh
```

**CentOS / RHEL:**


```
sudo systemctl restart sshd
```

---

### 第五步：验证登录

**再次提醒：不要关闭当前的 SSH 窗口！**

打开一个**全新的终端窗口**，尝试使用新端口进行连接：


```
ssh -p 50022 user@ip
```

- **如果能成功登录**：恭喜！说明配置完全正确。你现在可以安全地关闭旧的终端窗口了。
    
- **如果连接被拒绝 (Connection refused) 或超时 (Timeout)**：说明端口未开启、防火墙阻挡或云安全组未放行。请切回旧窗口排查错误。
    
- **如果提示 Permission denied (publickey)**：说明你的密钥没有配置正确，或者你连接时没有使用正确的私钥。请在旧窗口中把 PasswordAuthentication 改回 yes，重启 sshd 后重新配置密钥。






