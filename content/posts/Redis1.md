---
title: "Redis1"
date: 2023-03-04T22:10:41+08:00
draft: false
tags: [Redis, coding]
---

## Redis 簡介：
`* 前情提要：這是我第一次認識 Redis 感覺是一個很強大的東西 所以我想為他寫一些文章 介紹介紹這位朋友給大家！ ` 

Visit the [Redis](https://redis.io/) website!

----------
#### ⭐️ 什麼是 Redis:
Remote Dictionary Server (Redis)    
是一種開源（BSD 許可）內存數據結構存儲系統        
Redis的特點是速度快、内存占用小、支持豐富的數據結構和多種擴展功能        
Redis 可以通過定期將數據集轉儲到磁盤 或通過將每個命令附加到基於磁盤的日誌來持久保存您的數據         
Redis 也提供*異步複製* 具有快速非阻塞同步和自動重新連接，並在網絡拆分時進行部分重新同步       


*✨ 異步複製 Asynchronous Replication:*   
一種分佈式系統中常見的資料複製方式，它通過將資料從一個節點異步地複製到另一個節點，來實現資料的冗余和備份，以提高系統的可用性和容錯性      
在異步複製中，當主節點（源節點）接收到寫請求時，它會先將資料寫入本地存儲，並在後台異步地將資料複製到備份節點（目標節點）。複製的過程中可能會存在一定的延遲，即主節點已經完成寫操作，但備份節點尚未完成複製操作，因此如果主節點在此期間發生故障，那麼一部分資料可能會丟失。

#### ⭐️ 開始使用 Redis:   

##### Install on macos:
``` python
# Installation:
brew install redis
```
```python
# test your Redis installation
redis-server
```
###### Starting and stopping Redis using launchd:      
```python
brew services start redis
```
```python
# check the status
brew services info redis
```
```python
# To stop the service, run:
brew services stop redis
```
###### 與 Redis 互動：
Redis 執行後 可以試著叫出redis-cli:    
``` python
redis-cli
```
這樣就會打開一個 redis console !
可以試試下面的命令：
```python
lpush demos redis-macOS-demo
rpop demos
```
********
#### ⭐️ Redis 數據類型:
這邊簡單帶過 Redis 的數據類型         
詳細可以參考[官網](https://redis.io/docs/data-types/tutorial/)

###### ✨鑰匙(Key):
Redis 密鑰是二進制安全的        
意味著您可以使用任何二進制序列作為密鑰      
從“foo”這樣的字符串到 JPEG 文件的內容   
###### ✨字符串(String)：
使用SET和GET命令是我們設置和檢索字符串值的方式:   
```python
set mykey somevalue
```
```python
get mykey
# 得到 "somevalue"
```
另外我們也可以對字符串做增量：    
```python
set counter 100
incr counter
# (integer) 101
```
INCR命令將字符串值解析為整數    
最後將得到的值設置為新值        
還有其他類似的命令，如 INCRBY、 DECR 和 DECRBY    
###### ✨更改和查詢密鑰:
EXISTS命令返回 1 或 0 以指示給定鍵是否存在於數據庫中  
而 DEL 是刪除鍵和關聯值  
```python
set mykey hello
exists mykey
# (integer) 1
del mykey
# (integer) 1
exists mykey
#(integer) 0
```
###### ✨密要過期：
密鑰允許您為他設置時限    
也稱為 “ TTL ”     
當時間過去後 密鑰將自動銷毀     
*重要說明：*      
1. 可以使用秒或毫秒精度設置它們     
2. 過期的信息被複製並保存在磁盤上，當您的 Redis 服務器保持停止時，時間也會繼續計算      

使用EXPIRE命令設置密鑰的過期時間：      
```python
set key some-value
expire key 5
# 5秒後 key便會消失
```
-----------
#### ⭐️ 保護 Redis:
默認情況下 Redis 綁定到所有接口且沒有身份驗證!      
可以使用以下步驟以提高 Redis 的安全性：     
1. 確保 Redis 用於偵聽連接的端口被防火牆保護
（ 默認情況下為 6379，如果您在集群模式下運行 Redis，則為 16379，對於 Sentinel 則為 26379 ）
2. 使用 bind 以確保 Redis 僅偵聽您正在使用的網絡接口
例如 如果要讓 Redis 只監聽 127.0.0.1 对应的网络接口上的请求，可以在 Redis 配置文件中添加如下配置项：
    ```python
    bind 127.0.0.1
    ```
3. requirepass選項以添加額外的安全層 使客戶端需要使用AUTH命令進行身份驗證
4. 使用spiped或其他 SSL 隧道軟件來加密 Redis 服務器和 Redis 客戶端之間的流量    

--------
#### ⭐️ 在應用程序中使用 Redis:
要在應用程序中使用 Redis 我們需要安裝相關的 [client](https://redis.io/resources/clients/)       
以 Ruby 舉例 我們可以使用 'gem install redis' 來安裝    
使用 Ruby 的簡短交互式示例：      
```ruby
require 'rubygems'
require 'redis'
r = Redis.new
r.ping
#=> "PONG"
```

-----------
#### ⭐️ Redis 持久化：
      
簡單說明就是 如果我們希望數據庫在重啟後持久保存並重新加載     
要確保每次要強制執行數據集快照時手動調用 SAVE 命令      
否則就要使用*SHUTDOWN*命令關閉數據庫：    
```ruby
redis-cli shutdown
```
強烈建議閱讀[這篇](https://redis.io/docs/management/persistence/)！ 

-----------
OK, 那今天我們一起初次認識了 Redis !      
之後我們可以嘗試在專案中使用它啦！        
那麽我應該會為他寫第二天文章 就是關於實作的部分！     
歡迎閱讀喔！        