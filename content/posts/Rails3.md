---
title: "Rails寶典 第三章"
date: 2023-02-08T02:16:21+08:00
draft: false  
tags: [Ruby on Rails, coding]
---

## ♦️ Ruby on Rails   
`* 前情提要：我把我所學都埋藏在這裡了 去找吧！希望能幫助到新手！ `

嗨囉！大家好啊！      
今天的這篇文章會跟大家分享models的關聯    
寫完才發現篇幅有些大      
所以下一章再來介紹網站安全認證     
還有Rails的環境   
那事不宜遲 GoGo !     

### ✨ models表格關聯
#### primary_key 和 foreign_key :
再介紹有哪些關聯之前    
我們先來介紹兩個重要的key   
primary_key and foreign_key     
##### primary key:
primary key 簡單來說就是一筆資料在一個表格中的地址    
他們被用來辨識每筆資料      
需要注意的是他可以是多個欄位的    
舉例來說像下面這格表格：![image](https://i.imgur.com/3Wl12DT.png) 
而在 rails 中 就算你不寫    
在生出每一個表格的時候
他也預設會幫你做出一個 id 的欄位
並用 id 號碼作為 primary key

##### foreign key:  
foreign key 是用來做不同表格之間的關聯      
**Rails的慣例是「要被對到的那個 Model 的名字」加上 _id**
在一張表格中 可以跟很多表格有關聯性的欄位      
因此很常 foreign key 不只有一個     
當然也不一定要是 primary key

#### 關聯的型別：    
Rails models 有六種型別的關聯:
1. belongs_to
2. has_one    
3. has_many   
4. has_many :through
5. has_one :through
6. has_and_belongs_to_many  

##### 1. belong_to:
```ruby
class Book < ApplicationRecord
  belongs_to :author
end
```
![image](https://guides.rubyonrails.org/images/association_basics/belongs_to.png)

##### 2. has_one:
```ruby
class Supplier < ApplicationRecord
  has_one :account
end
```
![image](https://guides.rubyonrails.org/images/association_basics/has_one.png)
##### 3. has_many:
has_many關聯類似於has_one     
不同的是和另一個模型是以一對多連線      
##### 4. has_many :through :
has_many :through關聯通常用於與另一個模型建立多對多的連線     
![images](https://guides.rubyonrails.org/images/association_basics/has_many_through.png)
另外 has_many :through 在設置巢狀的has_many關聯的時候也很實用   
舉例來說 一個document 有多個sections    
然後一個sections有多個paragraphs      
並且你希望可以直接獲取document 中的paragraphs   
我們就可以這樣設置：  
```ruby
class Document < ApplicationRecord
  has_many :sections
  has_many :paragraphs, through: :sections
end

class Section < ApplicationRecord
  belongs_to :document
  has_many :paragraphs
end

class Paragraph < ApplicationRecord
  belongs_to :section
end
```
透過 through: :sections 我們可以直接取得paragraphs   
```ruby
@document.paragraphs
```

##### 5. has_one :through :
跟has_many :through 是差不多的    
只是這邊換成has_one   
舉例子 每個供應商都有一個帳戶 每個帳戶與一個帳戶歷史記錄相關聯
![image](https://guides.rubyonrails.org/images/association_basics/has_one_through.png)

##### 6. has_and_belongs_to_many :
has_and_belongs_to_many關聯與另一個模型建立了直接的多對多連線     
沒有干預模型
![image](https://guides.rubyonrails.org/images/association_basics/habtm.png)
#### ⭐️ 自己關聯自己：
有一次在我製作專案的時候    
遇到了這個問題：要怎麼將models關聯自己呢？
先說說這個專案叫做 whisper 是一個前輩帶我一起做的     
我們希望他可以像twitter一樣     
每個tweets下可以再有很多關聯的tweet留言     
在 whisper中的 buzzs 就是 tweets     
我們要讓 buzz可以透過 parent_id 這個foreign_key 來關聯自己
下面是最後成功的程式碼 歡迎參考🥳： 

首先是 model的設定
```ruby
# models/buzz.rb

class Buzz < ApplicationRecord
    belongs_to :user

    has_many :buzzs, foreign_key: "parent_id" 
    belongs_to :parent, class_name: "Buzz", optional: true 

    acts_as_votable
end
```
```ruby
# db/migrate/_add_parent_to_buzzs

class AddParentToBuzzs < ActiveRecord::Migration[7.0]
  def change
    add_column :buzzs, :parent_id, :integer, null: true 
  end
end
```
接著是 view的部分
```erb
<%# 展示首頁的buzzs(沒有 parent_id 的 buzzs)%>
<%# 檔案：views/buzzs/index.html.erb %>

<h1>Buzzs</h1>

<div id="buzzs">
  <% @buzzs.where(parent_id: nil).each do |buzz| %>
    <%= render buzz %>
    <p>
      <%= link_to "Show this buzz", buzz %>
    </p>
  <% end %>
</div>

<%= link_to "New buzz", new_buzz_path %>
```
``` erb
<%# 展示單一 buzz 的畫面(會有他的子 buzzs) 及創建child buzz 的 form %>
<%# 檔案：views/buzzs/show.heml.erb %>

<%= render @buzz %>

<%# render reply buzz %>
<% @buzz.buzzs.each do |buzz| %>
  <div class="sub_buzz">
    <%= render buzz %>
  </div>
  <%= link_to "Show this buzz", buzz %>
  <%= render partial: 'buzzs/form', locals: {buzz: buzz.buzzs.build, parent: buzz} %>
<% end %>


<div>
  <%= link_to "Edit this buzz", edit_buzz_path(@buzz) %> |
  <%= link_to "Back to buzzs", buzzs_path %>

  <%= button_to "Destroy this buzz", @buzz, method: :delete %>
</div>

<%# create form for child buzz %>
<%= render partial: 'buzzs/form', locals: {buzz: @buzz.buzzs.build, parent: @buzz} %>

```
```erb
<%# views/buzzs/_buzz.html.erb %>
<div id="<%= dom_id buzz %>">
  <% if buzz.parent_id == nil %>
    <p>
      <strong>Content:</strong>
      <%= buzz.content %>
    </p>

    <%= button_to like_buzz_path(buzz), method: :put do %>
	    <%= buzz.get_likes.size %>
    <% end %>

    <%= button_to dislike_buzz_path(buzz), method: :put do %>
	    <%= buzz.get_dislikes.size %>
    <% end %>
  
  <% else %>
    <p>
      <strong>Reply:</strong>
      <%= buzz.content %>
    </p>
  <% end %>
</div>
```
```erb
<!-- views/buzzs/_form.html.erb 用來創建新 buzz的 form -->

<%= form_with(model: buzz) do |form| %>
  <% if buzz.errors.any? %>
    <div style="color: red">
      <h2><%= pluralize(buzz.errors.count, "error") %> prohibited this buzz from being saved:</h2>

      <ul>
        <% buzz.errors.each do |error| %>
          <li><%= error.full_message %></li>
        <% end %>
      </ul>
    </div>
  <% end %>

  <div>
    <%= form.label "content", style: "display: block" %>
    <%= form.text_field :content %>
  </div>
  <% if !parent.nil? %>
    <input type="hidden" name="buzz[parent_id]" value= "<%= parent.id %>">
  <% end %>
  <div>
    <%= form.submit "Create"%>
  </div>
<% end %>
```
```erb
<!-- views/buzzs/new.html.erb -->

<h1>New buzz</h1>
<%= render partial: 'buzzs/form', locals: {buzz: @buzz, parent: nil} %>
<br>

<div>
  <%= link_to "Back to buzzs", buzzs_path %>
</div>
```
```erb
<!-- views/buzzs/edit.html.erb -->
<h1>Editing buzz</h1>

<%= render partial: 'buzzs/form', locals: {buzz: @buzz, parent: @buzz.parent} %>

<br>

<div>
  <%= link_to "Show this buzz", @buzz %> |
  <%= link_to "Back to buzzs", buzzs_path %>
</div>
```

----------------
今天的內容就分享到這邊啦！      
下一篇文章來介紹簡單的 debug 方法   
還有 rails console 的用法       
那我們下篇文章見啦！！！