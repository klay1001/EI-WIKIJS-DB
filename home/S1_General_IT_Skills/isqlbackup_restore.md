---
title: S1.2.2.9	isql: backup / restore
description: 
published: true
date: 2022-12-23T08:11:43.309Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:44:08.052Z
---

# isql: backup / restore
1.2.2.9 SAP (Sybase) ASE Backup Management
Sybase ASE 的備份，有很多種方式可以進行。如果 Oracle 一樣，ASE  的備份也分為 data backup 
和 (transaction) log backup 二種。
一個完整可以 point-in-time reocver 的 ASE database，必須要有 data backup 和 log backup 的備份排程。
## 確定你的 database 可以被還原

若考量到大規模的 Server 損壞如整台 Database Server 或機房損毀，只能利用備份和重裝來重建 database
，那你必須有以下的備份
(參見 note 1585981 - SYB: Ensure Recoverability for SAP ASE) 存在

•	    <DBSID> database              (SAP 的 database, 必要備)
•	    master database                   (必要備)
•	    sybsystemprocs database    (若 auto expansion  有啟用則必須要備)
•	    sybmgmtdb database            (optional)
•	    saptools database                 (optional)
此外，SAP 的 database 要有以下的 database options
•	 'trunc log on chkpt' , 'false'
•	 'full logging for all', 'true'
•	 'enforce dump tran sequence' , 'true'

## 基本備份動作 / 還原 - 利用 isql

你可以利用 isql 去手動驅動一次備份。你必須先在作業系統找到一個目錄空間，記下它的絕對路徑，
使為備份目標。
備份時，Sybase database 必須是運作中的，基本上我們都用做 online  backup。

### data 備份語法

1>	dump datagbase <DBSID> to '<path to your file with file name>' 
with compression = 101
    2> go

         ex.  

1>	dump datagbase PRD to '/sybdump/MYCUSTOMER/PRD/data/db_PRD_20031202-1.
dmp' with compression = 101
    2> go

### transaction log 備份語法

1>	dump transaction <DBSID> to '<path to your file with file name>' 
with compression = 101
    2> go

         ex.

1>	dump transaction PRD to '/sybdump/MYCUSTOMER/PRD/log/trans_PRD_20031202001.dmp' 
with compression = 101
    2> go

上面的備份動作是一次性的，沒有排程。
### 還原資料庫的方法
         還原資料庫的動作，若是還原給同一個資料庫，原則上不太會有問題。
但若是還原到一個新的資料庫(剛裝好的 ASE)，要確定 data segment 和 log segment 空間要必
         原始備份資料庫來的大。不然無法 load 資料進去。

         此外，要還原資料庫並讓資料庫上線，分三個動作 :

1.	         load data backup file
2.	         load transaction log file(s)
3.	         online datagbase
    ### 首先 load data backup file :
    
    1> load datagbase <DBSID> from '<path to your dump with file name>' 
    2> go
 
        ex.

    1> load datagbase QAS from '/sybdump/MYCUSTOMER/PRD/data/db_PRD_200302-1.dmp' 
    2> go


    ### 再來 load transaction backup file :

1>	load transaction <DBSID> from '<path to your translog directory>' with 
until_time = 'mmm dd, yyyy, HH:MM:SS:sss<am/pm>' 
    2> go
    若不要指定時間，可以把  with until_time = 'mmm dd, yyyy, HH:MM:SS:sss<am/pm>' 拿掉 

        ex.

1>	load transaction QAS from '/sybdumpMYCUSTOMER/PRD/log' with until_time = 
'mar 20, 2008, 10:51:32:333am' 
    2> go
    
    可以繼續執行多次 load transaction 去補 trans log
   ### 最後 online database :

    1> online database <DBSID>  
    2> go

        ex.

    1> online database QAS  
    2> go

## DBACockpit 備份排程
另一個常用的備份方式，是利用 DBACockpit 裡的排程機制去排程備份動作。
在設定排程時，需要給定 Backu Configuration Name，這是一個預先用 isql 設定好的組態，記載要備份的目的目錄，
以及是否要壓縮。

請務必參照 SAP Note 1588316 - SYB: Configure automatic database and log backups 的內容，做好排桯的準備。

## 設定 Dump Configuration

首先，要利用 sp_config_dump store procedure (在 isql 裡執行) 來設定二個 dump configuration。
下面的 sql 是來查看現有的 dump config

    # log on isql as sapsa 
    1> use master
    2> go
    1> sp_config_dump
    2> go

如下圖有二個 dump config 存在，一個叫 PRDDB, 一個叫 PRDLOG。(如果你的結果是空的，
就表示還沒有設定 dump config)
![image1.png](/s1229/image1.png)

然後我們再利用 sp_config_dump 去看這二個 dump config 的內容。

    1> sp_config_dump 'PRDDB'
    2> go 
![image2.png](/s1229/image2.png)
    1> sp_config_dump 'PRDLOG'
    2> go
![image3.png](/s1229/image3.png)
 

由上面二個 dump config 來看，總結他們的三個 attributes 為

|config_name|stripe_dir|compression|verify|
|--|--|--|--|
|PRDDB	| /backup/PRD/DB/data |	 101	| header|
|PRDLOG|	 /backup/PRD/DB/log	 |101	| header|

很明顯，PRDDB 是給 data dump 的 config，而 PRDLOG 是給 transaction log 的 config , 二者唯一不同的是
stripe_dir，也就是備份目的目錄。這個路徑請自行把空間先準備好。

如果你的環境裡什麼 config 都沒有，請手動用 isql 去建，如下 (dump config name 可以自訂):

    # isql logon as sapsa
    1> use master
    2> go
    1> sp_config_dump @config_name='P01DB',
    2> @stripe_dir = '/mybackup/path/P01/data',
    3> @compression = '101',
    4> @verify = 'header
    5> go

    # isql logon as sapsa
    1> use master
    2> go
    1> sp_config_dump @config_name='P01LOG',
    2> @stripe_dir = '/mybackup/path/P01/log',
    3> @compression = '101',
    4> @verify = 'header
    5> go

建好了 dump config 後，你之後的備份可以利用它了。
例如前面用 dump database 去做 data backup，利用 dump config 可以寫成 

    1> dump datagbase P01 using config='P01DB'
    2> go

    1> dump transaction P01 using config='P01LOG'
    2> go

### 設定 Dump Configuration

你可以利用 DBACockpit 去排備份，登入 SAPGUI，T-Code: DBACOCKPIT
切換到你的  Database，選 Jobs --> DBAPlanning Calendar
![image4.png](/s1229/image4.png) 

DBAPlanning Calendar 畫面如下, 通常是空白，本例是已有 schedule 存在。
![image5.png](/s1229/image5.png)
 

要排程一個工作，請按 "Add" 按鈕。(建議把操作切換到 Browser 裡動作，不然有可能視窗跳不出來)
![image6.png](/s1229/image6.png)

按 "Continue"
![image7.png](/s1229/image7.png)
 
選擇要排程的工作，這裡我們選 Database Dump (也就是會執行  "dump database")
![image8.png](/s1229/image8.png)

排程時間請選 "Schedule as Recurring Action" ， 按 "Continue"
![image9.png](/s1229/image9.png)
  
這裡要指定要備份的 database ，以及選一個 dump configuration (前面有說如何設定 dump config)，按 "Continue"
![image10.png](/s1229/image10.png)

給定一個執行時間。記得在下方 "Recurrence Range" 要勾 "No End Date"，不然預設是有期限的。按 "Continue"
![image11.png](/s1229/image11.png)
 
好，若確定設定正確，按 "Execute" 就會排程下去了。

用 DBACockpit 的 DBA Calendar 排程的備份，備份出來的檔名是由系統給的，
如下圖為某客戶的每日 data & log backup 檔案
![image12.png](/s1229/image12.png)

Data backup 的檔名格式

    <DBSID>.DB.yyyymmdd.HHMMSS.###

Transaction Log backup 的檔名格式

    <DBSID>.DB.yyyymmdd.HHMMSS.###

你可以撰寫 backup 清理 script，利用這個檔名格式去篩選備份檔。

