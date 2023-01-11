---
title: S1.2.2.6	isql: Extend data/log segment
description: 
published: true
date: 2022-12-23T07:36:04.524Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:42:33.403Z
---

# isql: Extend data/log segment
1.2.2.6 SAP (Sybase) ASE - 增加 data 和 log segment 空間
本篇是說明如何利用 isql command line 去增加 database 的空間。至於利用 DBACockpit 
或 Sybase Control Central 等其他方式去增加空間(會更便利) 不在本文目的，請參考相關官方文件。
由於空間滿常會造成 Sybase 或其上的 SAP 無法運作，至致於 DBACockpit (若裝在一起)
也很可能不能使用，所以會利用 isql 去增加空間是 BASIS 必學的技能。

Sybase ASE 資料庫的空間滿時，在 database log (/sybase/<DBSID>/ASE-XX_X/install/<DBSID>.log) 
裡會看到明顯的訊息。

   <place holder : 找到圖再貼上去>

看到類似的訊息，要先確定幾個重要的資訊，才能正確調整 database 大小，
不然很可能搞錯方向，浪費時間和空間。
•	哪一個 database ？ (是 PRD ? saptools ? sybmgmtdb ? sapsecurity ? 還是其他的)
•	哪一個 segment   ?  (是 default (data) 還是 log segment ?)
•	若是 log segment 滿了，先確定該 database 的 database option (利用 sp_helpdb <Datagbase> 去看) 
是否有設 "trunc log on chkpt" !
o	若沒有設定 "trunc log on chkpt" (表示產出的 trans log 需要備出來)，
請檢查是否定期 backup transaltion log (translaction log dump) 的動作是否不成功？
o	若有設定 "trunc log on chkpt" (表示產出的 trans log 會被覆寫掉，不需要備份出來)，
為何還會有 log segment 滿的狀況 ???
時常 ，問題在於 transaction log 備份不成功，導致 log segment 的空間無法釋出，
這時解法就是把備份問題解決(如備出去的空間不夠了，或 Job Scheduler Agent 又死掉了等等)。
在救急的當下，等不到下次排程的備份時間到，那可以先利用 isql 的 dump transaction 語法
把 transaction log 先備到某個空間去，以使 Sybase ASE 能繼續運行，事後再處理備份或空間的問題。


增加 data 空間

下例為在 Windows 平台上，為 database DEV  增加10GB 空間給 DEV 裡的 default (data) segment。

## STEP 1: 首先利用 disk init 去建立一個 database device 叫 "dataDEV_9"，
建立在一個 10GB 的 實體檔案叫 "dataDEV_9.dat"。
##                建立 database device 是在 master database 下做的，提供空間的實體檔案不需要先存在，
但它的路徑一定一先存在，否則會出錯。
1> use master
2> go
1>	disk init name =‘dataDEV_9’, physname=‘D:\sybase\DEV\sapdata_9\dataDEV_9.dat’
, size=‘10240M’
2> go
## STEP 2: 再來指派這個實體檔案裡的空間 (此例是全部都給，100%，也就是10GB) 給 DEV database。
1> use master
2> alter database DEV on dataDEV_9 = 10240
3> go
## STEP 3: 以 database DEV 的角度來看，把新加入了 device dataDEV_9 配置給 "default" (data) segment
1> use DEV
2> go
1> sp_extendsegment "default", DEV, dataDEV_9
2> go

增加 log 空間
前例為 "default" (data) segment 的狀況。如果問題發生在 logsegment，有可能真的是 logsegment 不夠大。
但在擴大 logsegment 之前請先利用 isql 去 dump 一次 transaction log，試著把 logsegment 清空，語法如 

    dump transaction <DBSID> to '/path/to/dumpfile'

通常在常常有 logsegment 滿，而且增加 transaction log 備份頻率後仍會發生; 
或是 transaction log 備份機制常失敗造成 logsegment 無法適時釋放；
這二種狀況就建議可以加大 logsegment 空間，以延後 logsegment 滿的發生時間。

下例為把 logsegment 增加 5GB。注意在 alter database DEV 後面，若是要給 logsegment，
要多加一個 "log" 。

1> use master
2> go
1>	disk init name = DEV_log_002, physname=‘D:\sybase\DEV\saplog_2\DEV_log_002.dat’
, size=‘5120M’
2> go
1> use master
2> alter database DEV log on DEV_log_002 = 5120
3> go
1> use DEV
2> go
1> sp_extendsegment "logsegment", DEV, DEV_log_002
2> go
