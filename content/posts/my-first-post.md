---
title: "My first post - 新朋友 Hugo"
date: 2022-12-16T01:06:15+08:00
draft: true

---

## 使用 Hugo 快速打造出自己的部落格  

大家好啊！！
這是我在部落格發的第一篇文章
我決定要來介紹介紹 [Hugo](https://gohugo.io)   
也就是幫我打造出這個部落格的東東    
在進入正文前先來分享心得  
*****
##### ✨ 心得的部分 ✨
真的太開心啦 我終於也能打造出一個屬於自己的部落格啦！🥳  
多虧 Hugo 的幫助   
一開始因為在學 Ruby on Rails 的緣故    
本來想直接用 Rails 打造出來   
而且前端也想自己刻出來     
阿不過寫到一半就放棄了    
寫部落格用 Rails 真的太大材小用了 😞   
而且我那時候甚至連文章可以用mk寫都不知道 ......  
不過後來懂了動態網頁和靜態網頁的區別後      
我找到了 Hugo
然後看起來很順利的打造出這個部落格了！  
對 大概整個心路歷程就是這樣 後面直接進入正題 ！
*****
#### 🍀 Hugo 使用手冊
這邊會簡單介紹如何使用 Hugo   
詳請的話可以參考官網 [Hugo](https://gohugo.io) 

##### 安裝 ( macOS or Linux )：
```
brew install hugo
```
##### 初始化
```python
hugo new site quickstart
cd quickstart 
git init
hugo server # 開啟 server
# hugo server -D  可以開啟沙盒 server
```
好了之後能在本地上就能看到server運行結果了    
接著就是關於主題還有內文
*****
##### Hugo Theme 佈景：    
* 首先可以到 [Hugo Theme](https://themes.gohugo.io/tags/blog/) 找自己喜歡的主題 
（這邊以 tranquilpeak 這個主題做舉例 ） 
* 複製主題:   
    ```git
    git clone https://github.com/kakawait/hugo-tranquilpeak-theme.git themes/tranquilpeak
    ```
* 複製他的 config ([Tranquilpeak Config](https://github.com/kakawait/hugo-tranquilpeak-theme/blob/master/exampleSite/config.toml)):   
    ```python
    cp themes/tranquilpeak/exampleSite/config.toml ./config.toml
    ```
* 客製化一些 config [ config.toml ]
  ```toml
  baseURL = "https://example.org/"
  languageCode = "zh-tw"
  defaultContentLanguage = "zh-tw"
  title = "Yu 的 Blog"
  theme = "tranquilpeak"
  disqusShortname = "hugo-tranquilpeak-theme"
  paginate = 7
  canonifyurls = true
  ```
*****
##### 文章撰寫：  
* 推薦的 VScode extension: [Markdown Preview](https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced) 

* Hugo Content - Archetypes 套用順序:    
  ```
  hugo new posts/my-first-post.md 
  ```
  新增一份 Content Page 之後    
  翻印來源會依據指令下列順序    
  找出有無符合模板    
  有的話則翻印一份產生新的文章   
  1. archetypes/posts.md
  2. archetypes/default.md
  3. themes/my-theme/archetypes/posts.md
  4. themes/my-theme/archetypes/default.md    
*****
##### 部署網站：

*****
### 參考連結： 
* [Hugo](https://gohugo.io/getting-started/quick-start/)   
* [Archetypes](https://gohugo.io/content-management/archetypes/)
* [Hugo content](https://ithelp.ithome.com.tw/articles/10243072)







