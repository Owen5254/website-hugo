---
title: "Rails寶典 第一章"
date: 2023-01-12T17:09:21+08:00
draft: false
---

## 🔥 Ruby on Rails 🔥

`* 前情提要：我把我所學都埋藏在這裡了 去找吧！希望能幫助到新手！ `

### Model View Controller :
這是最重要的概念了    
整個Rails專案是採用MVC(Model View Controller)所設計的     
這裡借用一張圖來說明關係 
![Example](/resources/_gen/images/Rails1/Ex1.png)
1. 使用者輸入網址 第一關會遇到的是Route (config/routes.rb）
2. Rails 會根據使用者輸入的網址及參數 告訴你應該去找哪個 Controller 上的哪個 Action
3. Controller通常會有多個action(也就是多個method)這個 Action 會決定要做什麼事
4. 接著可能會call Model請它幫忙查資料
5. Model 本身並不是資料庫 ! 但它可以會自己轉成資料庫看得懂的資料庫查詢語言
來和資料庫溝通
6. Model 跟資料庫取得你想要的資料
7. Model 把這包資料交回 Controller/Action 手上
8. 之後controller 會跟對應的View借畫面
9. Controller/action 把資料跟View的畫面組合 最後呈現給使用者看

##### Controller
Controller 就是放在專案的 app/controllers 目錄裡      
如果類別的名稱叫做 PostsController      
檔案的名字就會是 posts_controller.rb

##### Model
跟 Model 相關的檔案都放在 app/models 目錄裡     

![Example](/resources/_gen/images/Rails1/Model.png)

##### View
舉例來說 跟 PostsController 相關的 View     
就會放在 app/views/posts 目錄裡   
如果執行的是 PostsController 的 index Action      
預設會去找 app/views/posts/index.html.erb 這個檔案      
最後面的 erb 是 Embedded Ruby 的縮寫      
表示這個檔案會由 Ruby 標準函式庫中的 ERB 樣版引擎進行解讀

##### Route （警告：內容非常多）
Route的檔案為 config/routes.rb      
舉個例子 : http://rubyonrails.com/posts/123  
假設 routes.rb :
```ruby
Rails.application.routes.draw do
  get "/posts", to: "posts#index"
  get "/posts/:id", to: "posts#show"
end
``` 
當使用者輸入 /posts 這個網址    
它會交由 posts#index 來處理 (意思是 PostsController 上的 index 方法)        
同理 當使用者輸入 /posts/123 這個網址之後      
它會轉由 PostsController 上的 show 方法     
並且把 123 當做參數（:id）傳給 Controller

```
$ rails routes
```
終端機底下執行指令 可以看目前所有的路徑     

❗️**注意**：如果是放在專案的 public 目錄裡的檔案是不用經過 Route 的   

✨ **首頁網網址設定**：
```ruby
root "welcome#index"
```
☘️ **轉址** :
```ruby
get '/users', to: redirect('/accounts')
```
這樣一來就可以把 /users 轉往 /accounts 了

⭐️ **RESTful 設計**：     
Representational State Transfer     
導入 REST 的設計 可讓網址變得更直觀   
而且也幫開發人員訂了一套網址設計的慣例
當你對某個網址使用 POST 方法存取表示是新增資料    
當使用 PUT 或 PATCH 方法表示是更新資料    
使用 DELETE 方法則是表示刪除資料      

符合RESTful 的網址設計    
Rails 提供的 resources 方法   
```ruby
Rails.application.routes.draw do
  resources :users
end
```
![Example](/resources/_gen/images/Rails1/Ex2.png)

✨ **prefix**
在 rails routes 指令所秀出來的各項資訊中，有一欄叫 Prefix     
它在後面接上 _path 或 _url 後可以變成「產生相對應的路徑或網址」的 View Helper   
```
products + path     = products_path         => /products
new_product + path  = new_product_path      => /products/new
edit_product + path = edit_product_path(2)  => /products/2/edit
```
```
products + url     = products_url         => http://kaochenlong.com/products
new_product + url  = new_product_url      => http://kaochenlong.com/products/new
edit_product + url = edit_product_url(2)  => http://kaochenlong.com/products/2/edit
```

✨ **Resources 內建路徑不夠用...**
1. 使用 collection (不帶有 :id)：
例如我用resources產生order的路徑    
但我想再加一個路徑 GET /orders/cancelled        
    ```ruby
    Rails.application.routes.draw do
      resources :orders do
        collection do
          get :cancelled
        end
      end
    end
    ```
2. 使用member(帶有:id):
如果我想要以下路徑：
```
# 確認 2 號訂單
POST /orders/2/confirm

# 取消 3 號訂單
DELETE /orders/3/cancel
```
跟 collection 有點類似，就是在 orders 這個 Resources 裡加上 member
```ruby
Rails.application.routes.draw do
  resources :orders do
    member do
      post :confirm
      delete :cancel
    end
  end
end
```
這樣一來就會產生下圖新的路徑：
![Example](/resources/_gen/images/Rails1/Ex3.png)

*****
第一篇文章就先到這邊結束吧
歡迎繼續往下一章閱讀喔！

**參考資料:**
[為你自己學 Ruby on Rails](https://railsbook.tw/chapters/11-routes)