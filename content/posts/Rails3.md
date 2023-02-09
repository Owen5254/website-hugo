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
舉例來說 A表紀錄
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


##### 6. has_and_belongs_to_many :


#### ⭐️ 自己關聯自己：

