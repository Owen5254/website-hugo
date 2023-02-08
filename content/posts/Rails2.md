---
title: "Rails寶典 第二章"
date: 2023-02-08T02:16:21+08:00
draft: true
tags: [Ruby on Rails, coding]
---

## ♦️ Ruby on Rails   
`* 前情提要：我把我所學都埋藏在這裡了 去找吧！希望能幫助到新手！ `

上一章節介紹了Rails基本的MVC架構     
在這一章節我會介紹RESTful API     
以及一些Rails專案中常用的命令     
還有如何抓蟲子(debug)!  
那我們就開始啦! GoGo!   
### ✨ RESTful API:
#### 什麼是 REST ?
Representational State Transfer (REST) 是一種軟體架構     
它對 API 的運作方式施加了條件     
簡單來說就是大家一起規定製作API的標準架構   
REST有主要有兩個核心精神：      
1. 使用Resource來當做識別的資源，也就是使用一個URL網址來代表一個Resource      
2. 同一個Resource則可以有不同的Representations格式變化。        

RESTful帶給Rails最大的好處是：它幫助我們用一種比較標準化的方式來命名跟組織Controllers和Actions      
將RESTful帶入Rails路由系統的點子，出自它對應了HTTP動詞POST、GET、PATCH/PUT、DELETE到資料的新增、讀取、更新、刪除等四項操作。    

#### 🌰 舉個栗子：
```ruby
resources :events
``` 
如此就會自動建立四個命名路由(named routes)，搭配四個HTTP動詞，對應到七個Actions       
| Helper | GET | POST | PATCH/PUT | DELETE | 
|----------|----------|----------|----------|----------|
| event_path(@event)| /events/1   show action |    | /events/1  update action |/events/1 destroy action |
| events_path | /events index action |  /events create action |
|edit_event_path(@event) | /events/1/edit edit action|
|new_event_path|	/events/new new action |

詳細資料可以參考這個喔：[Rails實戰聖經](https://ihower.tw/rails/restful.html)

### ✨ 常用的命令：
介紹完REST後 接著我們來看一下Rails中常用的指令 
     
```ruby
# 關於專案常用創建指令

rails new #新建rails應用程式

rails s #啟動網路server

rails g #使用模版來generate很多東西 例如 rails g controller

rails console #可以從命令列跟 Rails互動 背後用的是 IRB

rails destroy #generate 的反操作

bundle install #據專案中的 Gemfile 去讀取專案有哪些 Gem 並安裝

rails generate scaffold User name:string email:string
# 快速創建一個名為 User 的基礎架構
```
```ruby
# 關於資料庫遷移 

rails db:migrate #進行資料庫遷移
rails db:migrate VERSION=20080906120000 #指定版本遷移
rails db:rollback #退回上次遷移
rails db:reset #將資料庫移除，再重新建立
```

```ruby
# 其他
rails db # 知道正在使用的資料庫

rails about # 列出整個 Rails 框架與環境的版本

rails assets:clean # 移除過時已編譯過的 Assets

rails db:create # 根據 config/database.yml 給目前的環境建立資料庫
```
更多資料歡迎參閱：[Rails命令列](https://rails.ruby.tw/command_line.html)
### ✨ Rails Debug
參考[官方Doc](https://rails.ruby.tw/debugging_rails_applications.html)

-------
        
今天的文章就先到這邊啦！      
之後第三章來寫寫資料庫關聯性    
以及rails環境吧！

