---
title: Python Web Scraping
date: 2025-10-12 17:04:53
tags: [updating]
archive: true
category: Python
category_bar: true
---

### Python 爬虫
浏览器向服务器发送 http 请求，服务器返回 http 响应，`requests`库可以帮助我们在 python 中发送 http 请求
```python
import requests
response=requests.get('https://sighingfield.tech/')
print(response)
print(response.status_code)
```
对于服务器的响应结果会返回不同的**状态码**：
|状态码|含义|
|---|---|
|2xx|请求成功|
|4xx|客户端错误|
|5xx|服务器错误|
```text
<Response [200]>
200
# 这里表示响应成功
```
所以我们可以根据**状态码的范围**判断请求是否成功：
```python
import requests
response=requests.get('https://sighingfield.tech/')
if response.status_code>=200 and response.status_code<400:
    print("请求成功") # 获取相应内容
elif  response.status_code>=400 and response.status_code<500:
    print("请求失败，客户端错误")
elif response.status_code >=500:
    print("请求失败，服务器错误")
```

相比于范围判断较为繁琐，我们可以直接使用`response.ok`判断请求是否成功：
```python
import requests
response=requests.get('https://sighingfield.tech/')
if response.ok:
    print("请求成功")
else:
    print("请求失败")
```

这里我们的请求头是默认的，但是也可以**自定义请求头**：这样的好处是可以帮助我们将爬虫程序**伪装成正常的浏览器**，从而防止服务器拒绝访问。（有些服务器只想服务于真正的用户，会根据`User-Agent`判断是否为浏览器）
{%note info%}
请求头可以通过在浏览器网页中**鼠标右键**进入“检查”->“网络”->将 “User-Agent” 冒号后的字符串复制下来
{%endnote%}
```python
import requests
headers={
    "User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36 Edg/141.0.0.0"
}
response=requests.get('https://movie.douban.com/top250',headers=headers)
print(response.status_code)
```
`BeautifulSoup`调用函数相当于一个**解析器**，可以将看上去复杂的 html 结构转变为一个**树形结构**，方便我们提取其中的信息。
```python
import requests
from bs4 import BeautifulSoup
headers={
    "User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36 Edg/141.0.0.0"
}
content=requests.get('https://movie.douban.com/top250',headers=headers).text
soup=BeautifulSoup(content,"html.parser")
print(soup.p)
# 返回html中第一个p标签的内容
```

`find_all()`方法可以返回**所有匹配的标签**，这里爬取信息需要一定的技巧，通过**观察网页源代码**以及我们想要爬取的信息的**标签结构特点**（比如标签之间的嵌入关系等），来确定爬取的方法。
```python
import requests
from bs4 import BeautifulSoup
headers={
    "User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36 Edg/141.0.0.0"
}
content=requests.get('https://books.toscrape.com',headers=headers).text
soup=BeautifulSoup(content,"html.parser")
all_title=soup.find_all('h3')
for title in all_title:
    link=title.find("a")
    print(link.string)
```
```text
A Light in the ...
Tipping the Velvet
Soumission
Sharp Objects
Sapiens: A Brief History ...
The Requiem Red
The Dirty Little Secrets ...
The Coming Woman: A ...
The Boys in the ...
```

#### 爬取豆瓣 Top 250 的电影名：
```python
import requests
from bs4 import BeautifulSoup
headers={
    "User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36 Edg/141.0.0.0"
}
for start_num in range(0,250,25):
    response = requests.get(f"https://movie.douban.com/top250?start={start_num}", headers=headers)
    soup = BeautifulSoup(response.content, "html.parser")
    all_titles = soup.find_all("span", attrs={"class": "title"})
    for title in all_titles:
        if "/" not in title.text:
            print(title.text)
```
```text
肖申克的救赎
霸王别姬
泰坦尼克号
阿甘正传
千与千寻
```

下面对关键代码进行解析：
```python
for start_num in range(0,250,25):
    response = requests.get(f"https://movie.douban.com/top250?start={start_num}", headers=headers)
```
若是没有这个外层循环，则只能爬取Top 250 第一页中的 25 个，通过**观察每一页 URL 的变化**，发现每页的 URL 中只有 `start=` 后面的数字不同，所以可以通过循环来实现爬取所有页面的电影名。

```python
all_titles = soup.find_all("span", attrs={"class": "title"})
```
提取所有标签为 `span` 且类名为 `title` 的标签，这些标签中包含了电影名。
```python
for title in all_titles:
    if "/" not in title.text:
        print(title.text)
```
若直接进行打印则会将电影的原版名打印出来：
```text
肖申克的救赎
 / The Shawshank Redemption
霸王别姬
泰坦尼克号
 / Titanic
阿甘正传
 / Forrest Gump
千与千寻
 / 千と千尋の神隠し
```
而我们现在只需要中文电影名，所以可以通过判断电影名中是否包含 `/` 来进行过滤。

