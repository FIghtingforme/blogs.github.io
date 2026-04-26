---

layout: post
title: 自动发送天气预报
date: 2026-03-31
catagories: Python

---
#### 接收方：钉钉
#### 发送方：和风天气
#### API_HOST,  WEBHOOK, API_KEY都在和风官方开发者后台里/头像首页
#### 平台：Github托管代码

#### 代码及解释（直接放在main分支）：

```
import requests

import json

  

# ================= 配置区 =================

API_HOST = 'p34y3kvamv.re.qweatherapi.com' # 你的控制台设置里

DINGTALK_WEBHOOK = 自己的

WEATHER_API_KEY = 自己的

CITY_ID = 天气api城市id    #城市代码可自行查询当地天气代码

# ==========================================

  

def get_weather():

    """获取和风天气数据"""
    

    url = f'https://{API_HOST}/v7/weather/3d?location={CITY_ID}'

    # 新版推荐将API KEY放在请求头中

    headers = {

        'X-QW-Api-Key': WEATHER_API_KEY

    }

    try:

        response = requests.get(url, headers=headers).json()

        if response['code'] == '200':

            today = response['daily'][0]

            date = today['fxDate']

            temp_max = today['tempMax']

            temp_min = today['tempMin']

            text_day = today['textDay']

            wind_dir = today['windDirDay']

            uv_index = today['uvIndex']

            # 注意：正文中必须包含你在钉钉里设置的“自定义关键词”（如：天气）

            msg = f"""### 🌤️ 今日天气预报

**日期**：{date}

**白天天气**：{text_day}

**温度**：{temp_min}℃ ~ {temp_max}℃

**风向**：{wind_dir}

**紫外线指数**：{uv_index}

  

祝你今天有个好心情！

"""

            return msg

        else:

            print(f"获取天气失败，错误码：{response['code']}")

            return None

    except Exception as e:

        print(f"请求发生异常: {e}")

        return None

  

def send_to_dingtalk(msg_text):

    """发送 Markdown消息到钉钉 """

    headers = {'Content-Type': 'application/json'}

    data = {

        "msgtype": "markdown",

        "markdown": {

            "title": "今日天气",

            "text": msg_text    

        }

    }

    response = requests.post(DINGTALK_WEBHOOK, headers=headers, data=json.dumps(data))

    print("钉钉返回结果：", response.text)

  

if __name__ == '__main__':

    weather_info = get_weather()

    if weather_info:

        send_to_dingtalk(weather_info)
        
```

---

### 把yml文件放在workflows下：
#### 代码：

```

name: Daily Weather Bot
on:
  schedule:
    - cron: '30 23 * * *' # 注意：这里是UTC时间！UTC的23:30相当于北京时间早上7:30。2000次最大/月
  workflow_dispatch: # 允许手动点击运行

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install requests
      - name: Run script
        run: python weather_bot.py #脚本名字
        
```

---

### 完成后每天7点半即可接收到钉钉消息！！！