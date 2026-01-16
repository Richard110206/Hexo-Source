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
|:-:|:-:|
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



### 调用API

```python
import requests
def get_weather(city:str)->str:
    """
    通过调用 wttr.in API 查询真实的天气信息。
    """
    url=f"https://wttr.in/{city}?format=j1"

    try:
        response=requests.get(url)
        response.raise_for_status()
        data=response.json()
        # 发起网络请求、检查状态码
        print(response)
        print(response.status_code)
        print(data)

        # 提取当前天气状况
        current_condition=data['current_condition'][0]
        weather_desc=current_condition['weatherDesc'][0]['value']
        temp_c=current_condition['temp_C']

        return f"{city}当前天气：{weather_desc}，气温{temp_c}摄氏度"
    
    except requests.exceptions.RequestException as e:
        # 处理网络错误
        return f"错误，查询天气时遇到网络问题-{e}"
    except (KeyError,IndexError) as e:
        # 处理数据解析错误
        return f"错误：解析天气数据失败-{e}"

print (get_weather('南京'))
```

调用 `wttr.in` API 获取实时天气数据时，我们可以通过打印返回的结果来查看 `data` 的结构。该结果是一个由字典和列表相互嵌套组成的复杂结构。明确其层级关系后，即可**根据键名和索引**准确提取所需字段的信息。

```text
<Response [200]>
200
{'current_condition': [{'FeelsLikeC': '9', 'FeelsLikeF': '48', 'cloudcover': '0', 'humidity': '62', 'localObsDateTime': '2026-01-15 07:47 PM', 'observation_time': '11:47 AM', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1016', 'pressureInches': '30', 'temp_C': '9', 'temp_F': '49', 'uvIndex': '0', 'visibility': '9', 'visibilityMiles': '5', 'weatherCode': '113', 'weatherDesc': [{'value': 'Clear'}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'ESE', 'winddirDegree': '109', 'windspeedKmph': '5', 'windspeedMiles': '3'}], 'nearest_area': [{'areaName': [{'value': 'Nanjing'}], 'country': [{'value': 'China'}], 'latitude': '32.062', 'longitude': '118.778', 'population': '3087010', 'region': [{'value': 'Jiangsu'}], 'weatherUrl': [{'value': ''}]}], 'request': [{'query': 'Lat 32.06 and Lon 118.79', 'type': 'LatLon'}], 'weather': [{'astronomy': [{'moon_illumination': '13', 'moon_phase': 'Waning Crescent', 'moonrise': '04:14 AM', 'moonset': '02:01 PM', 'sunrise': '07:06 AM', 'sunset': '05:23 PM'}], 'avgtempC': '13', 'avgtempF': '56', 'date': '2026-01-15', 'hourly': [{'DewPointC': '-2', 'DewPointF': '28', 'FeelsLikeC': '10', 'FeelsLikeF': '51', 'HeatIndexC': '11', 'HeatIndexF': '53', 'WindChillC': '10', 'WindChillF': '51', 'WindGustKmph': '20', 'WindGustMiles': '13', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '0', 'chanceofrain': '0', 'chanceofremdry': '80', 'chanceofsnow': '0', 'chanceofsunshine': '85', 'chanceofthunder': '0', 'chanceofwindy': '0', 'cloudcover': '3', 'diffRad': '0.0', 'humidity': '38', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1016', 'pressureInches': '30', 'shortRad': '0.0', 'tempC': '11', 'tempF': '53', 'time': '0', 'uvIndex': '0', 'visibility': '10', 'visibilityMiles': '6', 'weatherCode': '113', 'weatherDesc': [{'value': 'Clear '}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'SSW', 'winddirDegree': '212', 'windspeedKmph': '10', 'windspeedMiles': '6'}, {'DewPointC': '-1', 'DewPointF': '31', 'FeelsLikeC': '10', 'FeelsLikeF': '50', 'HeatIndexC': '10', 'HeatIndexF': '51', 'WindChillC': '10', 'WindChillF': '50', 'WindGustKmph': '14', 'WindGustMiles': '9', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '0', 'chanceofrain': '0', 'chanceofremdry': '92', 'chanceofsnow': '0', 'chanceofsunshine': '90', 'chanceofthunder': '0', 'chanceofwindy': '0', 'cloudcover': '6', 'diffRad': '0.0', 'humidity': '46', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1016', 'pressureInches': '30', 'shortRad': '0.0', 'tempC': '10', 'tempF': '51', 'time': '300', 'uvIndex': '0', 'visibility': '10', 'visibilityMiles': '6', 'weatherCode': '113', 'weatherDesc': [{'value': 'Clear '}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'WSW', 'winddirDegree': '255', 'windspeedKmph': '7', 'windspeedMiles': '4'}, {'DewPointC': '-0', 'DewPointF': '32', 'FeelsLikeC': '8', 'FeelsLikeF': '46', 'HeatIndexC': '9', 'HeatIndexF': '48', 'WindChillC': '8', 'WindChillF': '46', 'WindGustKmph': '17', 'WindGustMiles': '10', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '0', 'chanceofrain': '0', 'chanceofremdry': '89', 'chanceofsnow': '0', 'chanceofsunshine': '88', 'chanceofthunder': '0', 'chanceofwindy': '0', 'cloudcover': '0', 'diffRad': '0.0', 'humidity': '52', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1017', 'pressureInches': '30', 'shortRad': '0.0', 'tempC': '9', 'tempF': '48', 'time': '600', 'uvIndex': '0', 'visibility': '10', 'visibilityMiles': '6', 'weatherCode': '113', 'weatherDesc': [{'value': 'Clear '}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'W', 'winddirDegree': '261', 'windspeedKmph': '8', 'windspeedMiles': '5'}, {'DewPointC': '-1', 'DewPointF': '31', 'FeelsLikeC': '11', 'FeelsLikeF': '52', 'HeatIndexC': '11', 'HeatIndexF': '52', 'WindChillC': '11', 'WindChillF': '52', 'WindGustKmph': '7', 'WindGustMiles': '4', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '0', 'chanceofrain': '0', 'chanceofremdry': '88', 'chanceofsnow': '0', 'chanceofsunshine': '94', 'chanceofthunder': '0', 'chanceofwindy': '0', 'cloudcover': '0', 'diffRad': '55.8', 'humidity': '44', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1018', 'pressureInches': '30', 'shortRad': '201.4', 'tempC': '11', 'tempF': '52', 'time': '900', 'uvIndex': '1', 'visibility': '10', 'visibilityMiles': '6', 'weatherCode': '113', 'weatherDesc': [{'value': 'Sunny'}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'W', 'winddirDegree': '265', 'windspeedKmph': '5', 'windspeedMiles': '3'}, {'DewPointC': '-5', 'DewPointF': '24', 'FeelsLikeC': '17', 'FeelsLikeF': '62', 'HeatIndexC': '17', 'HeatIndexF': '62', 'WindChillC': '17', 'WindChillF': '62', 'WindGustKmph': '9', 'WindGustMiles': '6', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '0', 'chanceofrain': '0', 'chanceofremdry': '80', 'chanceofsnow': '0', 'chanceofsunshine': '91', 'chanceofthunder': '0', 'chanceofwindy': '0', 'cloudcover': '3', 'diffRad': '82.5', 'humidity': '23', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1017', 'pressureInches': '30', 'shortRad': '422.6', 'tempC': '17', 'tempF': '62', 'time': '1200', 'uvIndex': '3', 'visibility': '10', 'visibilityMiles': '6', 'weatherCode': '113', 'weatherDesc': [{'value': 'Sunny'}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'W', 'winddirDegree': '266', 'windspeedKmph': '8', 'windspeedMiles': '5'}, {'DewPointC': '-7', 'DewPointF': '20', 'FeelsLikeC': '19', 'FeelsLikeF': '66', 'HeatIndexC': '19', 'HeatIndexF': '66', 'WindChillC': '19', 'WindChillF': '66', 'WindGustKmph': '10', 'WindGustMiles': '6', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '0', 'chanceofrain': '0', 'chanceofremdry': '84', 'chanceofsnow': '0', 'chanceofsunshine': '92', 'chanceofthunder': '0', 'chanceofwindy': '0', 'cloudcover': '11', 'diffRad': '84.3', 'humidity': '17', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1016', 'pressureInches': '30', 'shortRad': '451.6', 'tempC': '19', 'tempF': '66', 'time': '1500', 'uvIndex': '1', 'visibility': '10', 'visibilityMiles': '6', 'weatherCode': '113', 'weatherDesc': [{'value': 'Sunny'}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'NW', 'winddirDegree': '306', 'windspeedKmph': '8', 'windspeedMiles': '5'}, {'DewPointC': '-6', 'DewPointF': '21', 'FeelsLikeC': '15', 'FeelsLikeF': '59', 'HeatIndexC': '15', 'HeatIndexF': '59', 'WindChillC': '15', 'WindChillF': '59', 'WindGustKmph': '9', 'WindGustMiles': '5', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '0', 'chanceofrain': '0', 'chanceofremdry': '86', 'chanceofsnow': '0', 'chanceofsunshine': '88', 'chanceofthunder': '0', 'chanceofwindy': '0', 'cloudcover': '12', 'diffRad': '47.4', 'humidity': '22', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1017', 'pressureInches': '30', 'shortRad': '213.2', 'tempC': '15', 'tempF': '59', 'time': '1800', 'uvIndex': '0', 'visibility': '10', 'visibilityMiles': '6', 'weatherCode': '113', 'weatherDesc': [{'value': 'Clear '}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'E', 'winddirDegree': '81', 'windspeedKmph': '4', 'windspeedMiles': '3'}, {'DewPointC': '-6', 'DewPointF': '21', 'FeelsLikeC': '13', 'FeelsLikeF': '55', 'HeatIndexC': '13', 'HeatIndexF': '55', 'WindChillC': '13', 'WindChillF': '55', 'WindGustKmph': '13', 'WindGustMiles': '8', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '0', 'chanceofrain': '0', 'chanceofremdry': '92', 'chanceofsnow': '0', 'chanceofsunshine': '94', 'chanceofthunder': '0', 'chanceofwindy': '0', 'cloudcover': '11', 'diffRad': '0.0', 'humidity': '26', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1017', 'pressureInches': '30', 'shortRad': '0.0', 'tempC': '13', 'tempF': '55', 'time': '2100', 'uvIndex': '0', 'visibility': '10', 'visibilityMiles': '6', 'weatherCode': '113', 'weatherDesc': [{'value': 'Clear '}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'SE', 'winddirDegree': '128', 'windspeedKmph': '6', 'windspeedMiles': '4'}], 'maxtempC': '19', 'maxtempF': '66', 'mintempC': '8', 'mintempF': '47', 'sunHour': '10.0', 'totalSnow_cm': '0.0', 'uvIndex': '1'}, {'astronomy': [{'moon_illumination': '8', 'moon_phase': 'Waning Crescent', 'moonrise': '05:10 AM', 'moonset': '02:51 PM', 'sunrise': '07:06 AM', 'sunset': '05:24 PM'}], 'avgtempC': '14', 'avgtempF': '57', 'date': '2026-01-16', 'hourly': [{'DewPointC': '-5', 'DewPointF': '23', 'FeelsLikeC': '10', 'FeelsLikeF': '51', 'HeatIndexC': '11', 'HeatIndexF': '52', 'WindChillC': '10', 'WindChillF': '51', 'WindGustKmph': '17', 'WindGustMiles': '11', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '38', 'chanceofrain': '0', 'chanceofremdry': '89', 'chanceofsnow': '0', 'chanceofsunshine': '77', 'chanceofthunder': '0', 'chanceofwindy': '0', 'cloudcover': '32', 'diffRad': '0.0', 'humidity': '31', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1017', 'pressureInches': '30', 'shortRad': '0.0', 'tempC': '11', 'tempF': '52', 'time': '0', 'uvIndex': '0', 'visibility': '10', 'visibilityMiles': '6', 'weatherCode': '116', 'weatherDesc': [{'value': 'Partly Cloudy '}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'SE', 'winddirDegree': '130', 'windspeedKmph': '8', 'windspeedMiles': '5'}, {'DewPointC': '-4', 'DewPointF': '25', 'FeelsLikeC': '8', 'FeelsLikeF': '47', 'HeatIndexC': '10', 'HeatIndexF': '49', 'WindChillC': '8', 'WindChillF': '47', 'WindGustKmph': '19', 'WindGustMiles': '12', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '33', 'chanceofrain': '0', 'chanceofremdry': '80', 'chanceofsnow': '0', 'chanceofsunshine': '84', 'chanceofthunder': '0', 'chanceofwindy': '0', 'cloudcover': '52', 'diffRad': '0.0', 'humidity': '39', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1016', 'pressureInches': '30', 'shortRad': '0.0', 'tempC': '10', 'tempF': '49', 'time': '300', 'uvIndex': '0', 'visibility': '10', 'visibilityMiles': '6', 'weatherCode': '116', 'weatherDesc': [{'value': 'Partly Cloudy '}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'SE', 'winddirDegree': '126', 'windspeedKmph': '9', 'windspeedMiles': '6'}, {'DewPointC': '-2', 'DewPointF': '28', 'FeelsLikeC': '8', 'FeelsLikeF': '46', 'HeatIndexC': '9', 'HeatIndexF': '48', 'WindChillC': '8', 'WindChillF': '46', 'WindGustKmph': '15', 'WindGustMiles': '9', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '31', 'chanceofrain': '0', 'chanceofremdry': '90', 'chanceofsnow': '0', 'chanceofsunshine': '86', 'chanceofthunder': '0', 'chanceofwindy': '0', 'cloudcover': '32', 'diffRad': '0.0', 'humidity': '46', 'precipInches': '0.0', 'precipMM': '0.0', 'pressure': '1016', 'pressureInches': '30', 'shortRad': '0.0', 'tempC': '9', 'tempF': '48', 'time': '600', 'uvIndex': '0', 'visibility': '10', 'visibilityMiles': '6', 'weatherCode': '116', 'weatherDesc': [{'value': 'Partly Cloudy '}], 'weatherIconUrl': [{'value': ''}], 'winddir16Point': 'SE', 'winddirDegree': '130', 'windspeedKmph': '7', 'windspeedMiles': '4'}, {'DewPointC': '-1', 'DewPointF': '30', 'FeelsLikeC': '11', 'FeelsLikeF': '53', 'HeatIndexC': '12', 'HeatIndexF': '53', 'WindChillC': '11', 'WindChillF': '53', 'WindGustKmph': '10', 'WindGustMiles': '6', 'chanceoffog': '0', 'chanceoffrost': '0', 'chanceofhightemp': '0', 'chanceofovercast': '32', 'chanceofrain': '0', 'chanceofremdry': '85', 'chanceofsnow': '0', 'chanceofsunshine': '88', 'chanceofthunder': '0', 'chanc
南京当前天气：Clear，气温9摄氏度
```



