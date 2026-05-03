---

title:  "网络工程师"    # 建议加双引号，防止标题中有冒号或特殊字符导致解析失败"
date: 2026-05-03T20:00:00+08:00 # 建议带上 ISO 8601 时间戳，方便精准排序和时区对齐
categories: ["Network"]         # 使用标准的 YAML 数组格式 []，可扩展性更好
tags: ["Network"]    # 强烈建议增加 tags 字段，这在 Hugo 中是默认的二级索引
draft: false                  # 显式声明发布状态，防止漏发

---




### 让不同网段下的pc互通：

- 拓扑图：

![alt text](image-3.png)



- pc1：

![alt text](image-1.png)


- pc2：

![alt text](image-2.png)


---

#### AR2220配置

- 0/0/0接口：192.168.10.1/24

- 0/0/1接口：192.168.20.1/24

```

[Huawei]interface giga	
[Huawei]interface GigabitEthernet 0/0/0
[Huawei-GigabitEthernet0/0/0]ip address 192.168.10.1 24
May  3 2026 18:52:02-08:00 Huawei %%01IFNET/4/LINK_STATE(l)[0]:The line protocol
 IP on the interface GigabitEthernet0/0/0 has entered the UP state. 
[Huawei-GigabitEthernet0/0/0]undo shutdown
Info: Interface GigabitEthernet0/0/0 is not shutdown.
[Huawei-GigabitEthernet0/0/0]q
[Huawei]q
<Huawei>save
  The current configuration will be written to the device. 
  Are you sure to continue? (y/n)[n]:y
  It will take several minutes to save configuration file, please wait.......
  Configuration file had been saved successfully
  Note: The configuration file will take effect after being activated
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]inter	
[Huawei]interface giga	
[Huawei]interface GigabitEthernet 0/0/1
[Huawei-GigabitEthernet0/0/1]ip add	
[Huawei-GigabitEthernet0/0/1]ip address 192.168.20.1 24
May  3 2026 18:53:06-08:00 Huawei %%01IFNET/4/LINK_STATE(l)[1]:The line protocol
 IP on the interface GigabitEthernet0/0/1 has entered the UP state. 
[Huawei-GigabitEthernet0/0/1]
[Huawei-GigabitEthernet0/0/1]q
[Huawei]q
<Huawei>save
  The current configuration will be written to the device. 
  Are you sure to continue? (y/n)[n]:y
  It will take several minutes to save configuration file, please wait.......
  Configuration file had been saved successfully
  Note: The configuration file will take effect after being activated


```


---


#### 测试两台pc间连通性：

- pc1

![alt text](image-4.png)

- pc2

![alt text](image.png)

#### 成功互相ping通！！

---
---




















