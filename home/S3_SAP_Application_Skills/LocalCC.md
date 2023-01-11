---
title:  S.3.1.5 Local Client Copy
description: 
published: true
date: 2023-01-04T08:33:16.596Z
tags: 
editor: markdown
dateCreated: 2023-01-04T08:32:57.991Z
---


Client Copy 分成 Local Client Copy (同一個系統內，如 DEV 裡 client 互 copy). 
和 Remote Client Copy (不同系統間的 client copy，常見的如 PRD 888 copy 到 QAS 500)。


## 如何進行 Local Client Copy

進行 Local Client Copy 時，是利用 T-Code SCCL 在目標 Client 進行。
例如你要把 DEV Client 000 Copy 出 Client 200 (也在 DEV)，就要在 Client 200 進行。
但通常要複制的目標 Client 不存在於系統內，所以要先在 SCC4 裡建立它的 client 資料，
並且利用 SAP* 登入到目標 client。為了讓 SAP* 可以登到目標 client，必須調整 RZ10 參數以及重啟 SAP Instance。
若原有的 Client 已經存在，你也可以執行 Client Copy，但原有的 Client 資料會被複寫。

## Client Copy Profiles

Client Copy 會 copy 那些資料，是在每次 copy 時於操作的 T-Code 
介面裡去指定一個設定叫 Copy Profile 來決定。常用的 profile 有
•	SAP_ALL        -->   複製所有內容如 交易，主檔，IMG設定，帳號，權限 
•	SAP_CUST     -->   只複製 IMG 設定，不含交易，主檔，帳號和權限
•	SAP_USER     -->   只複製帳號，其他都不複製。
其他還有很多 profile，請看官方說明。
依 Client Copy 的目的來決定選用什麼 copy profile。通常若是要測試新程式，
往往要真實資料，所以常會用 SAP_ALL。若只是要複製出新的 IMG client，用 SAP_CUST。

## 重要的小技巧
•	使用平行處理，以大幅縮短複製時間。
•	依資料庫類型，查詢必要的優化參數。
•	若目標 client 已存在，建議先執行 Client Deletion ，再做 Client Copy 。
•	先把 Archive Log (or transaction log) 關避，以免大量寫出造成磁碟空間不足。
