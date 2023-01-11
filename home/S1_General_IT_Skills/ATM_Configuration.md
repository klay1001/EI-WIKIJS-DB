---
title: S1.2.2.7	ATM Configuration
description: 
published: true
date: 2022-12-23T07:40:48.454Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:43:01.721Z
---

# ATM Configuration
1.2.2.7 SAP (Sybase) ASE - ATM Configuration
Sybase ASE 如同 Oracle database 一樣，也會需要利用 Table 裡資料分佈狀況的統計值來優化 query plan。
在 SAP on Oracle database 環境下，通常可利用 DB13 去排程 Check Database & Optimize statisitcs 的工作
，就可定期去做 statistics 的更新，以保效能。
而在 SAP on Sybase ASE 的環境下，BASIS 則需要利用 DBACockpit 去啟用(setup) Automatic Table 
Maintenance (ATM) 功能。
一旦 ATM 功能啟動後，ASE 會自行排程去檢查以及更新個個 table 的統計值，
而 BASIS 的工作最主要的是查看 ATM log 有沒有錯誤。



## 啟用 ATM

在 SAP  on  Sybase ASE 的環境下，請登入到你的 SAP (用 SAP GUI)，下 T-code:  DBACOCKPIT，
出現如下圖的 WEB 介面。
然後在點選您要設定的 Database (如本例為 Database SYZ), 然後再下拉 "Configuration" 選單 
選 "ATM Configuration"，就會看到如下的畫面  

錯誤! 尚未指定檔名。
![image1.png](/s1227/image1.png) 


如果該 Database 的 ATM 沒有啟動，就會看到如上圖的樣子 ，明顯告知你 
"ATM has not yet been setup for system XXX"
並有一個 "Set up ATM" 的 Link 出現。 你只要點 "Set up ATM"  link 就可以啟用 ATM 了。
啟用完後，就可以看到如下的畫面 ，Maintenance Windows 和 Profiles 都會生效。

錯誤! 尚未指定檔名。
![image2.png](/s1227/image2.png) 


上圖中的 Maintenance Windows 是 SAP 預設設定好的，通常都不會更改。

## 查看 ATM 待執行工作和執行 History

一旦 ATM 啟用 ，Sybase 會利用它自己的 Job Scheduler (見架構) 依照 Maintenance Windows 的時間去自動做資料檢查。
若發現有 Table/Index 需要更新統計值，就會把它們丟到 ATM Queue 裡去等待統計值的更新，或是做 Table reorg。
你可以在 DBACockpit 裡的，Diagnositics 選單裡選 ATM , 再選 ATM Queue，可看到待執行的工作 ，如下圖
![image3.png](/s1227/image3.png) 


而你也可以看執行完成的更新動作，這是在 Diagnostics --> ATM --> ATM History 裡，如下圖
![image4.png](/s1227/image4.png)
 



## 查看 ATM 的 Log
ATM 的機制若發生狀況，可以由  ATM Log 裡看到問題。你可由  Diagnostics --> ATM --> ATM Log 裡看，如下圖
![image5.png](/s1227/image5.png)
 

原則上 ATM log 會被保留 14 天。

其他更進階的內容，如 ATM Windows, ATM Profile 等概念，請參考 
官方文件 DBACockpit : Automatic Table Maintenance for Sybase ASE

