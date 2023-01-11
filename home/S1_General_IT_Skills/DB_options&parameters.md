---
title: S1.2.2.5	DB options & parameters
description: 
published: true
date: 2022-12-23T07:35:05.160Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:41:59.951Z
---

# DB options & parameters
1.2.2.5 SAP (Sybase) ASE - Database Options & Parameter Configuration
Sybase ASE 在裝完後，有一些必要的設定必須確定完成，系統的效能以及是否可 point-in-time recover 
才能符合 SAP 的要求。

一般 Sybase ASE 裡可以做調整的參數，有以下二類 :
1.	Database Options
2.	Database Parameters
Database Options 
Database Options 是針對一個 database (如 master,  PRD,  saptools, 等)，可以設定的旗標。
每個 Option 只有設定或不設定，沒有數值上的給定。

以下是 SAP 要求，production  ASE database 必須要有的 Options

<tbd>

查看目前 database 的 options 可以在 isql 用 sp_helpdb  去查看，Syntax 如下

  # logon isql 
  1> use master
  2> go 
   1> sp_helpdb  PRD
  2> go 

上例就會列出長長的 PRD database 內容，在最上面的地方會列出有設定給 PRD database 的  options.

但要設定 option，必須利用 sp_dboption 去一個一個 option 設定。

  # logon isql 
  1> use master
  2> go <ENTER>
  1> sp_dboption PRD, "select into/bulkcopy/pllsort", true
  2> go <ENTER>

上例就是把 option "select into/bulkcopy/pllsort" 設定給 database PRD

啟用 transaction log 
  利用 sp_dboption 去關掉 'trunc log on chkpt'
  # logon isql 
  1> use master
  2> go <ENTER>
  1> sp_dboption PRD, 'trunc log on chkpt', 'false'
  2> go <ENTER>

  另外，依 note 1585981 的說明，一併要設定  'full logging for all' 為 true ；
以及  'enforce dump tran sequence' 為 true 。
  當 trunc log on chkpt 由 true 改為 false 後，log segment 就不會自動在 check point 清資料，
所以若寫滿了 database 就不能運作。
  所以要立即做二件事 :
1.	   做一次 database dump 備份
2.	   設定定期做 transaction dump 備份  
   若是要停止寫出 trans log，只要再用 sp_dboption 去把 trunc log on chkpt 設為 true 就好了。
Database Parameters
Parameter 就是可以指定數值的調校參數。因為很多，不在此列舉。
你可以在 isql 利用 sp_configure  去查看及設定他們的值。有些是必須要重啟 service 才會生效的。

  # logon isql 
  1> use PRD
  2> go  <ENTER>
  1> sp_configure 'net password encryption reqd', 0
  2> go <ENTER>
  ....結果輸出.....
  1>

若用 DBACockpit 去查看 Sybase ASE, 你可以讓 DBACockpit 在 "Database Configuration" 
那一節裡做自動檢查 (Check Configuration)。
若你的 parameter 參數不符建議值，會被例出，你可以直接在上面(DBACockpit UI) 做修改，
不用手打 isql statement。
但要注意，DBACockpit bug 很多，一定要儘可能上到最新的 DBACockpit note ，不然這功能不容易成功。
