---
title: "LineBot 使用心得"
date: 2023-01-10T16:03:57+08:00
draft: false
tags: [python, linebot, coding]
---

## 使用LineBot後的心得分享

大家好啊！  
今天要跟大家分享一些LineBot的使用心得     
是說為什麼會突然接觸LineBot呢？     
故事要從一個寒冷的冬天說起......    
大三的上學期 我選了一堂台大的 python 應用課程    
那時認識了兩個新同學一起組隊    
其中有一個護理系的組員提出希望做一個可以幫忙挑選專輯的程式   
故事太長了......    
結局就是我們寫完後還有一些時間    
然後我就想說來試看看用LineBot包裝他好了     
然後就接觸LineBot了🎃       
好！故事說完了 接下來分享一些學到的東西！   

*****
#### 🍀 LineBot 使用手冊
這邊會從零開使分享如何架設並部署一個LineBot   
後面會簡單介紹一些官方的 API    
開始吧!

##### 第一步：到[LineDeveloper](https://developers.line.biz/zh-hant/)申請

到LineDeveloper官網   
申請一隻帳號（或用自己line帳號也可以）    
然後申請一隻機器人      
詳細可以參考: [這篇文章](https://www.learncodewithmike.com/2020/06/python-line-bot.html)

##### 第二步：架設 Django
當然也可以用其他框架來架設    
不過因為是python的專案      
所以這邊選擇使用Django
```
$ pip install django
$ pip install line-bot-sdk
```
安裝上面這兩個套件    
之後就可以建立Django
```
$ django-admin startproject mylinebot .  
$ python manage.py startapp albumlinebot  
$ python manage.py migrate  
```
❗️這邊要注意幾點：      
1. 第一個指令是建立一整個Django
2. 第二個指令是建立這個Django內的應用程式
也就是說他是建立一個albumlinebot這個應用程式在mylinebot內      
3. 第三個指令是執行資料遷移     

##### 第三步：連結應用程式（albumlinebot）和主專案程式(mylinebot)
開啟mylineboty資料夾底下的settings.py   
在INSTALL_APPS的地方    
加上剛剛所建立的應用程式: albumlinebot
```python
# Application definition    

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    
    'albumlinebot.apps.AlbumlinebotConfig',
]

```
接著設定連結網址      
在albumlinebot下 建立一個 urls.py 檔案 
```python
from django.urls import path
from . import views
 
urlpatterns = [
    path('callback', views.callback)
]
```
並在mylinebot資料夾下 的 urls.py 中加入以下設定     
```python
from django.contrib import admin
from django.urls import path, include
 
urlpatterns = [
    path('admin/', admin.site.urls),
    #包含應用程式的網址
    path('albumlinebot/',include('albumlinebot.urls')) 
]
``` 
第三步基本上就ok啦

##### 第四步：連結LineBot和Django
開啟mylineboty資料夾底下的settings.py     
增加Line Developers上所取得的兩個憑證設定     
來與LineBot進行連結
``` python
LINE_CHANNEL_ACCESS_TOKEN = 'Messaging API的Channel access token'

LINE_CHANNEL_SECRET = 'Basic settings的Channel Secret'
```
好啦！    
這樣基本上已經連結差不多了      
接下來就是要部署啦！

##### 第五步：部署網站
我這邊用[Ngrok](https://ngrok.com/)來部署     
安裝好後執行：
```python
$ ngrok http 8000
# Django在本機運行的埠號為8000
```
Ngrok就會隨機產生一個HTTPS的網址      
把這個網址填入LINE Webhook URL
還有應用程式albumlinebot的settings.py檔案中的ALLOWED_HOSTS    
```python
ALLOWED_HOSTS = [
    '網域名稱'  
]
```
##### 最後一步：執行LINE Bot應用程式
```
$ python manage.py runserver
```
*****
做完上面的步驟後      
就成功完整架設好一個linebot啦     
後面一篇文章會分享一些API的用法     
下篇見啦 掰掰！

✨ 參考資料：[[Python+LINE Bot教學]](https://www.learncodewithmike.com/2020/06/python-line-bot.html)









