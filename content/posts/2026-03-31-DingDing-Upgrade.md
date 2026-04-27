---

title: "钉钉自动巡检告警--完全版"
date: 2026-03-31
categories: ["Python"]

---
### 钉钉自动巡检告警：
#### 注意：钉钉机器人有使用频率限制：每个机器人最多20条/min
#### 注意：配置前安装好相应库
#### 功能：自动检测并定时发送hostname，datetime，cpu，mem，disk，服务状态；Zabbix面板链接可自行添加
#### 文本格式：Markdown

#### 代码及解释：

```
import psutil

import socket

import requests

import json

from datetime import datetime

  

#配置区==================================================

  

WEBHOOK_URL = "你的钉钉群机器人token"

  

#设置合理告警阈值：

THRESHOLD_CPU = 80    #CPU

THRESHOLD_MEM = 85    #内存

THRESHOLD_DISK = 90   #磁盘

  

#=======================================================

  
  

def get_sys_info():

    """获取系统基础信息"""

    # 获取主机名和当前时间（格式自定义）

    hostname = socket.gethostname()

    now_time = datetime.now().strftime("%Y-%m-%d %H-%M-%S")

  

    # 1.CPU 使用率（采样1次/s）：

    cpu_usage = psutil.cpu_percent(interval=1)
    

    # 2.MEM 使用率：

    mem = psutil.virtual_memory()

    mem_usage = mem.percent

  

    # 3.DISK 使用率：

    disk = psutil.disk_usage('/')

    disk_usage = disk.percent

  

    return hostname, now_time, cpu_usage, mem_usage, disk_usage

  

def check_service(service_name):

    """检查特定进程（服务）是否在运行"""

    # 遍历当前所有进程名：

    for proc in psutil.process_iter(['name']):

        if service_name.lower() in proc.info['name'].lower():

            return "✔️ 该服务正在运行中。"

    return "❌️ 该服务已经停止！"

  

def send_markdown_msg(hostname, now_time, cpu, mem, disk, nginx_status):

    """发送 MarkDown 格式告警"""

  

    #根据状态判断标题颜色（若有异常则提醒）：

    status_title = "🤚 系统巡检正常"

    if cpu > THRESHOLD_CPU or mem > THRESHOLD_MEM or disk > THRESHOLD_DISK or "❌️" in nginx_status:

        status_title = "🤦🏻‍♂️ 系统告警：检测到异常"

  

    # Markdown 内容

    markdown_content = f"""### {status_title}

**主机名称**： {hostname}

**巡检时间**： {now_time}

  

---

- **CPU 使用率**：{cpu}%  {'⚠️' if cpu > THRESHOLD_CPU else ''}

- **内存 使用率**：{mem}% {'⚠️' if mem > THRESHOLD_MEM else ''}

- **磁盘 使用率**：{disk}% {'⚠️' if disk > THRESHOLD_DISK else ''}

- **Nginx服务**: {nginx_status}

  

---

[点击查看Zabbix监控看板](http://Zabbix地址)

"""

  
  

    data = {

        "msgtype": "markdown",

        "markdown": {

            "title": "服务器巡检报告",

            "text": markdown_content

        }

    }

  

    headers = {"Content-Type": "application/json"}

    try:

        res = requests.post(WEBHOOK_URL, data=json.dumps(data), headers=headers)

        print(f"发送状态：{res.text}")

  

    except Exception as e:

        print(f"发送失败：{e}")

  

if __name__ == "__main__":

    #执行巡检

    h, t, c, m, d = get_sys_info()

  

    #可换成其他进程名

    n_status = check_service("nginx")

  

    #发送报告

    send_markdown_msg(h, t, c, m, d, n_status)
```

#### 推荐：Emoji状态表情可自行复制进去。

#### 函数及库的使用
##### 1. 库的调用
psutil: 专门用来读取 CPU、内存、进程等硬件和系统信息。
socket: 用来获取 hostname（主机名），这在管理几十台服务器时非常重要，否则你不知道是哪台机器出的问题。
datetime: 获取当前时间，所有的运维报告必须带时间戳。

##### 2. get_sys_info() 函数
psutil.cpu_percent(interval=1): 为什么要设置 interval=1？因为 CPU 波动非常快，瞬间 100% 并不代表有问题，采样 1 秒能得到一个更准确的平均值。

##### 3. check_service() 函数
这是真实场景中最常用的。比如收银系统的后台进程、数据库进程，都可以通过这个函数监控。它 process_iter 遍历系统所有活着的进程名.

##### 4. send_markdown_msg() 函数
Markdown 格式：这是企业级告警的标配。
动态预警：使用了 Python 的 f-string 和简写 if。例如 {'⚠️' if cpu > THRESHOLD_CPU else ''}，只有当 CPU 超过阈值时，报告里才会出现警告图标。



---