---
title: S1.2.2.3	isql logon and simple query
description: 
published: true
date: 2023-01-05T03:03:02.328Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:40:24.856Z
---

# isql logon and simple query
#### SAP (Sybase) ASE - isql command line 工具
前言 :  若您對 ASE database 完全沒有概念，請先看 Sybase ASE 給入門者的說明 。

一般資料庫管理，都會有 command line 工具能讓管理者進行 SQL Statement 的執行。如同 Oracle 資料庫的管理有 sqlplus ，Sybase ASE 也有對應的工具叫做 isql 。
當你安裝好 SAP application with Sybase ASE 後，每一台 sybase ASE server 上都有 isql 程式。執行時請先登入(Windows)或 切換 (Linux , 利用 su - syb<dbsid>) 身份到
syb<dbsid>，因為只有這身份才有正確的環境變數可以找到及執行 isql command。

除了作業系統 user 必須是 syb<dbsid> 之外，執行 isql 時，還必須對您的 ASE database 做登入，這時通常是利用安裝時建立的 sapsa 帳號(存在 ASE 裡，不在 OS)，他
的密碼通常也是 SWPM  安裝時用的 master password。 isql command 必須在 cmd console (Windows) 或 Terminal (Linux) 這種文字模式下執行。

isql 的官方說明文件以及完整的參數，請參閱  Sybase ASE isql 文件(外)


如何執行 isql (互動式)
以 syb<sid> 身份登入 ASE database 主機的作業系統，開啟一個 cmd console (Windows) 或 Terminal (Linux)。利用以下 syntax 進入 isql 的互動模式 :

     isql -U<userid> -P<password> -X -D<database> -S<DB server>

註: 若不給 -P<password>，在按 ENTER 後會被詢問密碼。-D 是指 database 如 DEV, master, saptools，而 -S 是指 database server ，也就是 /sybase/<DBSID> 的那個 DBSID，
通常在一台主機裝多個 sybase database server 時才有需要用到。 -X 是指要用加密的方式連結，這在後來新版的 isql 變成是必須。

上述指令完成(按 ENTER) 並成功登入後，就會看到 1> 這樣的 prompt 出現，你就進入 isql 互動模式。要進出這個互動模式請打 exit <ENTER> ，就會回到 cmd console 或 terminal 環境。
如下圖，登入到 DEV database，密碼提供在 -P參數。  注意 ，參數和參數值中間不需要有空格(最好統一都不要有空格)。
![image1.png](/1223/image1.png)
 

如果不給密碼，會被詢問，再於  Password:  後打入密碼，按 ENTER 即可進入，如下:
![image2.png](/1223/image2.png) 

在 isql 環境裡，每一行打完即使按 ENTER，也不代表要執行這一行指令。ENTER 後 isql 會把 prompt # 增加 1, 如 1> 變成 2>。
所以你可以繼續打指令，組成一個多行的指令，然後最後一行打 "go" 再按 ENTER，就會把 1>到 go 之前的指令當 input 執行。
isql 的執行不帶結尾的分號，這點和 oracle 不同。

如下圖，在 isql 環境理切換目前工作對象的 database 到 master database。
![image3.png](/1223/image3.png) 

當前述的多行指令執行完成後，isql prompt 會再回到 1>，又是重新可以打入多行的指令了。

下圖是利用 SELECT statement 去讀取 SAP Table T001 的筆數(count(*))。
![image4.png](/1223/image4.png) 
要結束互動模式，請打 exit 及按 ENTER 就可以跳出。
 
如何執行 isql (非互動式, 即指定 sql 檔案)

有時你需要利用 cron job 執行 SQL statement 去做一些自動化的工作。這時無法利用 isql 互動模式來一行一行敲。所以請先寫好要執行的 sql statement 存放於檔案內，
利用 isql -i 參數指定 sql statement file，然後可利用 -o 參數把 output 寫到別的檔案去。 Syntax 如下:

     isql -U<userid> -P<password> -X -D<database> -S<DB server> -i<PathToSQLFile> -o<PathToOutputFile>

For example, 我們有一段 SQL statement 可以把 DEV server 上所有 database 的 data segment, log segment 的使用量及百分比列出來，這段 statement 我們存在一個叫  getdbusage.sql 檔裡，如下
![image5.png](/1223/image5.png) 

我們打算利用 cronjob 每天去執行它，並把結果輸出到一個叫 dbusage.txt 檔去。 我們要先能寫出一行 isql command 能完成整個動作，才能設定到 crontab 裡。
這行 command 如下 :
![image6.png](/1223/image6.png) 

我們看一下結果
![image7.png](/1223/image7.png)

後續 crontab 設定及如何 parse 結果檔就不在此述。應用是無限的，各位自行發揮。
另外提到一個 isql 的參數是 -w<column-width> 。預設 isql text output 是 80 個字就換行，若要讓畫面拉平少換行，可以增加，如利用 -w240 可以到 240 個字再換行。
其他 formating 的參數和語法請參考原廠文件。

常用的 isql 指令
