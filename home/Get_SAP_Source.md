---
title: 如何取用 SAP 官方資源
description: 
published: true
date: 2022-12-12T07:09:46.216Z
tags: 
editor: markdown
dateCreated: 2022-12-12T06:24:00.373Z
---

# 如何取用 SAP 官方資源

## 常用連結
SAP 官方網站上的技術資源，絕大部份都是有權限限制的。您需要有一組 S-User 帳號(如 S0007722213)及密碼，才能登入讀取資料。若您沒有，請先向總管理處詢問。沒有這個 S-User 帳號，基本上是玩不下去的。

BASIS 日常工作常用連結:
[• SAP 官網 ](http://www.sap.com)   
[• SAP Help ](http://help.sap.com)(產品線上文件)
[• SAP Support Portal ](https://support.sap.com)(需登入) 
[• SAP Notes ](https://support.sap.com/notes)(需登入) 
[• SAP 社群 (SCN) ](http://scn.sap.com)(需登入) 
[• Software Download Center (SWDC) ](https://support.sap.com/swdc )(需登入) 
[• Product Availability Matrix (PAM) ](https://apps.support.sap.com/sap/support/pam )(需登入)

上面的連結打久了就會記得，但建議放在您的 Browser bookmark 裡，以方便取用。最常用到的是 SAP Notes 以及 SAP 社群(SCN)，尤其在做 troubleshooting 時是必備工具。
#### 提醒:請先確定你已經有 S-User 帳號。試著登入上面所有的連結。

## 要了解完全陌生的產品
若要了解陌生的(或最新的) SAP 產品而想看官方文件，先到 [SAP Help](http://help.sap.com) (產品線上文件) 裡，找到所謂的 Master Guide。SAP 對所有產品的每一個主要版本都會有一個所謂的 Master Guide，這是對該產品概念及定位的總說明。對一個完全不了解的產品，正統的方式是先去研讀 Master Guide，有了概念之後，再去讀 Installation Guide, Administration Guide, Security Guide 等實作的細節。此外，也可以到 [ SAP 社群 (SCN) ](http://scn.sap.com) 裡找到相關的 "Place"，裡面會有整理好的連結，通常也會有常見問題 (FAQ) 以及討論區，對新手來說十分有幫助。

#### 提醒:請找到 SAP ERP6.0 EHP7 的 Master Guide 及 Installation Guide。

## 解決問題的方式
當你在做 troubleshooting，遇到不明的錯誤訊息或無法解決的狀況時，可用以下五個步驟來試著找答案:
1.	記下畫面上的錯誤訊息，最好是 ST22 的 shortdump，或 log file 裡的訊息。
2.	先用錯誤訊息到 Google 查詢有沒有人有類似狀況，看怎麼解。
3.	再到 SAP 社群查詢有沒有人有類似狀況，有沒有人提供解法。
4.	再到 SAP Notes 裡查詢，看是不是已知的 bug，或是操作上需要特別的設定。
5.	如果到這裡都沒有用，向 SAP 開 Case (OSS Message)。

## 關於 SAP Support Portal－https://support.sap.com
BASIS 常會需要到 SAP Support Portal 上做一些申請的動作。你應會在文件裡看到所謂 SAP Service Market Portal 這個字眼。實際上說的就是 SAP Support Portal。一般在做新導入專案時，或新安裝 SAP System 時，都會有必要使用到 Support portal。有些動作是鎖客戶的，所以要用客戶的 S-User 帳號登入 support portal 來操作，如申請 License Key，申請 Developer Key，申請 Remote Connection 及 Router Certificate 等；有些是可以用您自己(碩益的) 的 S-User 帳號來做，如查詢 SAP Note，查詢 PAM 等。SAP 對軟體下載也會有權限卡控，一般用 碩益的 S-User 帳號是可以下載大部份的軟體，但是對某些客戶所使用的產品(如 ERP on MSSQL，碩益不能下載 MSSQL)，必須用客戶的 S-User 帳號來登入下載。
以下為常見會用到 SAP Support Portal (又稱 SAP Service Market Portal) 的時機：
1.	要下載 SAP 軟體來安裝。
2.	要下載 SAP 軟體的 Patch (Support Package, Hotfix)。
3.	安裝完要申請 License Key。
4.	要為 ABAP programmer 申請 Developer Key 或 Object Key。
5.	要設定 SAP Router 連線到 SAP 的 Remote Connection。
6.	要開通(Enable) Remote Connection，讓 SAP 連進來做 support。
7.	發 ticket (OSS Message) 給 SAP 要求支援。
![Image](https://blogs.sap.com/wp-content/uploads/2014/03/smpstarttutorial_410965.png =500x250)








