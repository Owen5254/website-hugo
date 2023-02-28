---
title: "Rails寶典 第五章 form"
date: 2023-02-28T01:29:19+08:00
draft: false
tags: [Ruby on Rails, coding]
---

## ♦️ Ruby on Rails   
`* 前情提要：我把我所學都埋藏在這裡了 去找吧！希望能幫助到新手！ ` 

哈囉！      
這篇文章主要分享 Rails 透過 Action View 提供的表單簡化撰寫          
那我們就開始吧！GoGo!     
******************

### ✨ 簡單的表單 (form_tag):
##### 1. 通用搜索表單：
```ruby
<%= form_tag("/search", method: "get") do %>
  <%= label_tag(:q, "Search for:") %>
  <%= text_field_tag(:q) %>
  <%= submit_tag("Search") %>
<% end %>
```
產生的HTML:
```html
<form accept-charset="UTF-8" action="/search" method="get">
  <input name="utf8" type="hidden" value="&#x2713;" />
  <label for="q">Search for:</label>
  <input id="q" name="q" type="text" />
  <input name="commit" type="submit" value="Search" />
</form>
```
*❗️搜索表單永遠使用 GET 動詞 這允許使用者可以把搜索結果加入書籤 之後便能透過書籤瀏覽 !*

##### 2. 同時傳多個 Hash 產生的問題:
form_tag 輔助方法接受 2 個參數: 表單送出的目標路徑和 Hash 選項      
Hash 選項用來指定表單所使用的方法           
以及其它 HTML 選項 如class              
下面得程式碼產生了一個問題：
```ruby
form_tag(controller: "people", action: "search", method: "get", class: "nifty_form")

#HTML => '<form accept-charset="UTF-8" action="/people/search?class=nifty_form&amp;method=get" method="post">'
```
這裡 method 與 class 變成了 URL 的查詢字串          
因為 Rails 將這四個參數認成了一個 Hash          
需要把第一組 Hash 放在大括號裡：
```ruby
form_tag({ controller: "people", action: "search" }, method: "get", class: "nifty_form")

# HTML => '<form accept-charset="UTF-8" action="/people/search" class="nifty_form" method="get">'
```

##### 3.特別的表單輔助方法：
Rails 提供一系列的輔助方法用來產生表單元素          
像是多選方框（checkboxes）文字欄位（text fields）單選按鈕（radio button）   
1. *多選方框*
```ruby
<%= check_box_tag(:pet_dog) %>
<%= label_tag(:pet_dog, "I own a dog") %>
<%= check_box_tag(:pet_cat) %>
<%= label_tag(:pet_cat, "I own a cat") %>
```
html:
```html
<input id="pet_dog" name="pet_dog" type="checkbox" value="1" />
<label for="pet_dog">I own a dog</label>
<input id="pet_cat" name="pet_cat" type="checkbox" value="1" />
<label for="pet_cat">I own a cat</label>
```

2. *單選按鈕*
```ruby
<%= radio_button_tag(:age, "child") %>
<%= label_tag(:age_child, "I am younger than 21") %>
<%= radio_button_tag(:age, "adult") %>
<%= label_tag(:age_adult, "I'm over 21") %>
```
html:
```html
<input id="age_child" name="age" type="radio" value="child" />
<label for="age_child">I am younger than 21</label>
<input id="age_adult" name="age" type="radio" value="adult" />
<label for="age_adult">I'm over 21</label>
```
*⭐️永遠記得幫多選方框與單選按鈕加上 label*        
*label 可以為特定的輸入新增說明文字 也會加大可按範圍 讓使用者更容易選中!*

*************

### ⭐️ 處理 Model 物件:
#### 1.Model 物件輔助方法:
表單通常拿來新建或編輯 Model 物件。             
可以使用 *_tag 這些輔助方法來處理 但太繁瑣了        
Rails 提供更多方便的輔助方法像是 text_field、text_area 等專門用來處理 Model 物件            

*這些輔助方法的第一個參數是實體變數的名字 第二個參數是要對實體變數呼叫的方法名稱*
假設 Controller 已經定義了 @person          
@person.name 是 Henry 則：
```ruby
<%= text_field(:person, :name) %>
```
產生：
```html
<input id="person_name" name="person[name]" type="text" value="Henry"/>
```
*送出表單後 使用者的輸入會存在 params[:person]*           
*params[:person] 可傳給 Person.new*                   
*或是如果 @person 是 Person 的實體 則可傳給 Person#update*    

❗️第一個參數必須是實體變數的“名稱” 如：:person 或 "person"          
而不是傳實際的實體物件進去          

#### 2. 將表單綁定到物件上：
若 Person 有很多屬性時 得一直重複傳入要編輯的物件名稱 來生成對應的表單
不過 Rails 提供了 form_for 用來將表單綁定至 Model 的物件             
假設有處理文章的 Controller app/controllers/articles_controller.rb      
```ruby
def new
  @article = Article.new
end
```
```ruby
<%= form_for @article, url: {action: "create"}, html: {class: "nifty_form"} do |f| %>
  <%= f.text_field :title %>
  <%= f.text_area :body, size: "60x12" %>
  <%= f.submit "Create" %>
<% end %>
```
*❗️@article 是實際被編輯的物件*
產生的HTML:
```html
<form accept-charset="UTF-8" action="/articles/create" method="post" class="nifty_form">
  <input id="article_title" name="article[title]" type="text" />
  <textarea id="article_body" name="article[body]" cols="60" rows="12"></textarea>
  <input name="commit" type="submit" value="Create" />
</form>
```
在 create 動作裡的 params[:article] 會是有著 :title 與 :body 鍵的 Hash 

使用 *fields_for* 輔助方法也可以達到上面的效果 但不會產生出 form 標籤       
同個表單用來編輯多個 Model 物件時很有用         
如 Person Model 有個關聯的 ContactDetail Model          
下面的表單可以同時建立兩個 Model 的物件 :
```ruby
<%= form_for @person, url: {action: "create"} do |person_form| %>
  <%= person_form.text_field :name %>
  <%= fields_for @person.contact_detail do |contact_details_form| %>
    <%= contact_details_form.text_field :phone_number %>
  <% end %>
<% end %>
```
```html
<form accept-charset="UTF-8" action="/people/create" class="new_person" id="new_person" method="post">
  <input id="person_name" name="person[name]" type="text" />
  <input id="contact_detail_phone_number" name="contact_detail[phone_number]" type="text" />
</form>
```

#### 3. 記錄自動識別技術:
處理 RESTful 資源時 若用了記錄自動識別技術 則呼叫 form_for 便很容易使用          
```ruby
## Creating a new article
# long-style:
form_for(@article, url: articles_path)
# short-style:
form_for(@article)
```
```ruby
## Editing a existing article
#long-style:
form_for(@article, url: article_path(@article), html: {method: "patch"})
# short-style:
form_for(@article)
```
*❕處理命名空間*
若建立的路由有命名空間 form_for 也有對應的簡寫形式          
假設應用程式有 admin 命名空間：
```ruby
form_for [:admin, @article]
```
會在 admin 命名空間裡 建立出對 ArticlesController 提交的表單        
送出結果到 admin_article_path(@article)
多層命名空間也是同理：
```ruby
form_for [:admin, :management, @article]
```
************
好啦！今天就先到這邊吧！            
這篇文章簡單整理了一些 rails 好用的表單輔助方法！       
如果想知道更多可以參考[這篇文章](https://rails.ruby.tw/form_helpers.html)喔！       
那就下一篇見啦 掰掰！