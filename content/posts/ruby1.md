---
title: "Ruby手冊 第一章"
date: 2023-01-14T15:08:28+08:00
draft: false
tags: [Ruby on Rails, coding]
---

## ♦️ Ruby手冊 第一章
這篇文章中會著重介紹一些Ruby和其他語言的基礎差異      
有興趣的話歡迎閱讀喔！    

#### ☘️ 變數（variable) 常數（Constant）
 
| 種類 | 範例 | 預設值 | 
|----------|----------|----------|
| 區域變數 （local variable）| name   | 沒有   |
| 全域變數 （global variable) | $name   | nil   |
| 實體變數 （instance variable） | @name   | nil   |
| 類別變數 （class variable） | 	@@name  | 沒有   |

⭐️ **實體變數(@):**    
實體變數只能在類別內部訪問，如果要在外部訪問，需要定義讀寫方法    
類別外部存取一個實體變數，可以定義讀寫方法，例如 **attr_accessor**

#### ☘️判斷：
```ruby
# if
if a > b:
  puts 'a>b'
elsif a = b:
  puts 'a=b'
else
  puts 'a<b'
end
```
```ruby
# unless
unless is_adult?(20)
  puts "你是大人了"
end
# 相當於 if not
if not is_adult?(20)
  puts "你是大人了"
end
```
```ruby
# case
weather = "下雨"

case weather
when "下雨"
  puts "待在家!"
when "出太陽"
  puts "出去玩!"
else
  puts "在家睡覺!"
end
```

#### 迴圈 迭代(Loop and Iteration)：
在 Ruby 的迴圈主要有幾種：

1. while 迴圈
2. for..in 迴圈
3. times, upto, downto 方法
4. 迭代（iteration）

在Rails 開發中比較常用到的是第三和第四種

⭐️ **times, upto, downto:**
```ruby
5.times do
  puts "hello, ruby"
end

# 執行後得到結果：
# hello, ruby
# hello, ruby
# hello, ruby
# hello, ruby
# hello, ruby
``` 
```ruby
1.upto(5) do |i|
  puts "hi, ruby #{i}"
end

# 執行後得到結果：
# hi, ruby 1
# hi, ruby 2
# hi, ruby 3
# hi, ruby 4
# hi, ruby 5
```
```ruby
5.downto(1) do |i|
  puts "hi, ruby #{i}"
end

# 執行後得到結果：
# hi, ruby 5
# hi, ruby 4
# hi, ruby 3
# hi, ruby 2
# hi, ruby 1
```

⭐️ **迭代(each):**
```ruby
friends = ["eddie", "joanne", "john", "mark"]

friends.each do |friend|
  puts friend
end
```

#### ☘️字串：
**字串安插：**   

除了一般的 + 寫法     
Ruby 的字串有提供了字串安插的寫法
```ruby
name = "Yu"
age = 20

puts "你好，我是 #{name}，我今年 #{age} 歲"
# => 印出「你好，我是 Yu，我今年 20 歲」
# 注意！必須是雙引號 單引號沒有這種寫法！
```

#### ☘️陣列：

**增加陣列元素(<<):**
```ruby
my_array = [1, 2, 3]
my_array << 4
# my_array 現在為 [1, 2, 3, 4]
```

#### ☘️字典(Hash):
Hash 有兩種寫法   
```ruby
# 比較舊的寫法
old_hash = {:title => "Ruby", :price => 350}
```
```ruby
# 新的寫法類似 JSON 格式
new_hash = {title: "Ruby", price: 350 }
```

#### ☘️ symbol :
symbol 是一個在ruby中非常特別的東西     
而且又非常實用！      
我們可以把 symbol 是一個「帶有名字的物件」   
我們來看一個例子：        
```ruby
class Order
  attr_reader :status

  def initialize(items, status = :pending)
    @items = items
    @status = status
  end

  def compete
    @status = :complete
  end
end

order = Order.new(["item A", "item B", "item C"])

if order.status == :pending
  puts "order is pending"
end

```
上面的這段程式中    
:pending :complete 都是symbol   
代表 pending 跟 complete 這兩個「狀態」   

##### ✨symbol 跟變數有什麼不同：

Symbol 是一個「帶有名字的物件     
本身不需要指向任何東西也可以拿來用   
例如上面的 :pending 跟 :complete 的例子      
```ruby
:name = "Yu"   # 這得到 SyntaxError 的錯誤訊息
```
而且我們無法直接拿 Symbol 來當變數      
像這樣會出現語法錯誤 

##### ✨symbol 跟字串有什麼不同：

字串是可以修改的    
但symbol不行！
而因為symbol不可變的原因      
使他在處理上要比字串快非常多    
因此如果是不用更改的東西通常會使用symbol來代替字串    
例如 Hash 裏的 Key
```ruby
profile = { name: "Yu", age: 18 }
# 相當於 {:name => "Yu", :age=>18}
```
*****
第一章就先到這裡啦！      
有興趣的讀者歡迎接著看喔！      

參考資料：[為自己學Ruby on Rails](https://railsbook.tw/chapters/06-ruby-basic-2)