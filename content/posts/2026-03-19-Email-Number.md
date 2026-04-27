---

title: "电话号码和 E-mail 提取器"
date: 2026-03-19
categories: ["Python"]

---





### 提取电话和邮箱


#### 导入库

```
import re, pyperclip
```



  

#### 创建手机号正则表达式(手机号格式)

```
phoneRegex = re.compile(r'\d{3}\S?\d{4}\S?\d{4}')
```

  

#### 创建邮箱正则表达式（邮箱格式）


```
emailRegex = re.compile(r'''(

    [a-zA-Z0-9._%+-] +      # 用户名部分

    \S                      # @ 符号

    [a-zA-Z0-9.-] +         # 域名部分

    (\.[a-zA-Z]{2,4})      # 最后的后缀 (.com/.cn)

    )''', re.VERBOSE)

```


#### 从剪贴板获取文本

```
text = str(pyperclip.paste())
```


  
  
  

#### 提取匹配项

```
matches = []

for groups in phoneRegex.findall(text):

    matches.append(groups)          #纯文本内容，所有全部加入

for groups in emailRegex.findall(text):

    matches.append(groups[0])       #最外层大括号内容全部加入

```


  

#### 将结果存回剪贴板

```
if len(matches) > 0:

    pyperclip.copy('\n'.join(matches))

    print('提取成功，已存入剪贴板！')

else:

    print('未发现电话或邮箱。')
```




