---
title: "Rails寶典 第四章 Devise"
date: 2023-01-12T17:09:21+08:00
draft: false
tags: [Ruby on Rails, coding]
---

## ♦️ Ruby on Rails   
`* 前情提要：我把我所學都埋藏在這裡了 去找吧！希望能幫助到新手！ ` 

哈囉！      
今天跟大家分享Rails中很強大的套件就是 Devise！    
那我們就開始吧！GoGo!     

### ✨ Devise：
Devise 是 Rails 中一套最有名的使用者認證套件      
他可以幫你快速產生一套完整的使用者註冊登入介面      
以下是使用方法：
##### ☘️ 安裝 Devise：
```ruby
gem 'devise'
```
然後記得 bundle install 安裝套件    
之後輸入rails g devise:install產生devise設定檔        
```ruby
rails g devise:install
```
編輯 config/environments/development.rb 和 production.rb 加入寄信時預設的網站網址       
```ruby
config.action_mailer.default_url_options = { :host => 'localhost:3000' }
```
輸入 rails g devise user產生 User model 及 Migration    
```ruby
rails g devise user
```
輸入 rails generate devise:views產生HTML樣板      
放在app/views/devise 目錄下
```ruby
rails g devise:views
```

##### ☘️ Devise 使用方法：
在需要登入的 controller 加上before_action :authenticate_user!       
Layout 中加上登入登出選單
```h
<% if current_user %>
  <%= link_to('登出', destroy_user_session_path:method => :delete) %> 
  <%= link_to('修改密碼', edit_registration_path(:user)) %>
<% else %>
  <%= link_to('註冊', new_registration_path(:user)) %> 
  <%= link_to('登入', new_session_path(:user)) %>
<% end %>
```
##### ☘️ Devise 自訂欄位：
如果你想加一些新的欄位    
舉例來說我們要加 displayname 到 User Model:     
在 db/migrate/devise_create_users.rb
```ruby
class DeviseCreateUsers < ActiveRecord::Migration[7.0]
  def change
      t.string :display_name, null: false, default: ""
  end
end
```
然後執行 rails db:migrate     
我們可以在 db/migrate/schema.rb 內查看所有的 table 資訊     


-----
Devise 真的是 Rails內非常厲害的套件       
如果想更佳熟用它可以參考[官方Github](https://github.com/heartcombo/devise)!       
那今天就介紹到這邊啦！        
下一章主題是表單form 歡迎繼續收看喔！


