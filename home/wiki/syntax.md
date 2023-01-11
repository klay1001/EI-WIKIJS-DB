---
title: 如何利用 Wiki 語法？
description: 利用簡單的前輟及/或後輟字元，可以讓您的文字有多種程式變化。
published: true
date: 2022-07-27T09:32:00.920Z
tags: wiki, syntax
editor: markdown
dateCreated: 2022-07-27T06:36:00.847Z
---

# Markdown
Wiki.js 的官方 Markdown 語法說明如下 :
https://docs.requarks.io/en/editors/markdown

# 懶人包版
* ## 標題 (前輟井字空一格)
```
# 標題一
### 標題三
###### 井字愈多，標題等級愈低，最多五級
```
# 1個井字為標題一
### 3個井字為標題三
###### 井字愈多，標題等級愈低，最多五級




* ## 斜體字 (頭尾加星不留空)
```
這行裡有三個*斜體字*在裡面
There is an *italic* word here.
```
這行裡有三個*斜體字*在裡面
There is an *italic* word here.




* ## 粗體字 (頭尾加二個星不留空)
```
這行裡有三個**粗體字**在裡面
There is an **bold** word here.
There are three ***bold and italic*** words, 頭尾加三星!!.
```
這行裡有三個**粗體字**在裡面
There is an **bold** word here.
There are three ***bold and italic*** words, 頭尾加三星!!.

* ## 分隔橫線 (行頭連線三個橫線再空一行)
```
分隔上的一段文字

---

分隔下的一段文字


```

分隔下的一段文字

---
分隔下的一段文字


* ## 列舉三角 (單星打頭)
```
* 這行前面會有三角，星號後要有一個空格 item1
* 這行前面會有三角，星號後要有一個空格 item2
* 這行前面會有三角，星號後要有一個空格 item3
* 這行前面會有三角，星號後要有一個空格 item4

```
* 這行前面會有三角，星號後要有一個空格 item1
* 這行前面會有三角，星號後要有一個空格 item2
* 這行前面會有三角，星號後要有一個空格 item3
* 這行前面會有三角，星號後要有一個空格 item4



* ## 數字依序項次 (起始數字打頭加點空格)
```
1. 最重要的事
1. 很重要的事
1. 不太重要的事
1. 可以忘記的事

```
1. 最重要的事
2. 很重要的事
3. 不太重要的事
4. 可以忘記的事

* ## URL連結 (中括裡放連結名，小括裡放URL)
```
請常常到[碩益官網](http://www.soetek.com.tw)逛逛。
如果我完全不用 [ ] 和 ( )，直接打網址，像 https://www.yahoo.com ，也會自動變成 LINK，但連結名就是 URL
```
請常常到[碩益官網](http://www.soetek.com.tw)逛逛。 
如果我完全不用 [ ] 和 ( )，直接打網址，像 https://www.yahoo.com ，也會自動變成 LINK，但連結名就是 URL

* ## 圖片 (驚歎號加連結，或用編輯工具)
```
沒驚歎號只是連結
[wiki logo](https://js.wiki/img/wikijs-full-2021.b840e376.svg)
有驚歎號就會顯示連結到的圖。
![wiki logo](https://js.wiki/img/wikijs-full-2021.b840e376.svg)
可利用 =axb 設長寬, 80x20  
![wiki logo](https://js.wiki/img/wikijs-full-2021.b840e376.svg =80x20)
可利用 =xp 設縮放, x30=30%
![wiki logo](https://js.wiki/img/wikijs-full-2021.b840e376.svg =x30)

```
沒驚歎號只是連結
[wiki logo](https://js.wiki/img/wikijs-full-2021.b840e376.svg)
有驚歎號就會顯示連結到的圖。
![wiki logo](https://js.wiki/img/wikijs-full-2021.b840e376.svg)
可利用 =axb 設長寬， 80x20
![wiki logo](https://js.wiki/img/wikijs-full-2021.b840e376.svg =80x20)
可利用 =xp 設縮放, x30 = 30%
![wiki logo](https://js.wiki/img/wikijs-full-2021.b840e376.svg =x30)


* ## 列舉三角

