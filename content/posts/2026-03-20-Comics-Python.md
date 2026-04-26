---

title: "Python自动下载整个网站的漫画 "
date: 2026-03-20
categories: Python

---


### 实现源码： 

```
import requests

import os

import bs4

  

def download_comics():

    url = 'https://xkcd.com/3216/'

    floder_name = 'xkcd_comics'

    os.makedirs(floder_name, exist_ok=True)

  

    count = 0

  

    while not url.endswith('#') and count < 5:

        print(f"正在访问网页： {url}")

  

        headers = {'UserAgent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64)'}

        res = requests.get(url, headers=headers)

        res.raise_for_status()

  

        soup = bs4.BeautifulSoup(res.text, 'html.parser')

  

        comic_elem = soup.select('#comic img')

  

        if comic_elem == []:

            print("未在该页面找到图片！！！")

        else:

            comic_url = 'https:' + comic_elem[0].get('src')

  

            print(f'下载图片：{comic_url}')

            res = requests.get(comic_url, headers=headers)

            res.raise_for_status()

  

            image_name = os.path.basename(comic_url)

            image_path = os.path.join(floder_name, image_name)

  

            with open(image_path, 'wb') as image_file:

                for chunk in res.iter_content(100000):

                    image_file.write(chunk)

  
  

        prev_link = soup.select('a[rel="prev"]')[0]

        url = 'https://xkcd.com' + prev_link.get('href')

  

        count += 1

    print("所有漫画下载完毕！！请看xkcd_comics文件夹")

  
  

if __name__ == '__main__':

    download_comics()
```


##### 注意：处于网络问题可能有访问不了的情况！


