---
title: S1.2.2.1	ASE Architecture Knowledge
description: 
published: true
date: 2022-12-23T07:19:03.603Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:39:25.359Z
---

# ASE Architecture Knowledge
1.2.2.1 ASE Architecture - 給入門者的說明
ASE 介紹
Sybase ASE (Adaptive Server Engine) Database (現在叫做 SAP ASE) , 是 SAP 原廠除了 HANA 之外，最多人用的資料庫。
大部份客戶使用 ASE 的原因不是因為效能或熟悉，而是軟體 run time 費用比起 Oracle, MSSQL 都來的便宜，而硬體費用也遠比 HANA 便宜。
常看到的部署組合是 ERP 和 BW 用 HANA，而 Solution Manager,  Fiori Frontnet (NetWeaver Gateway) 等用 Sybase ASE。

SAP 有完整的官方文件，說明 ASE 個個面向的技術細節。目前 ASE 的版本是 16.0，前一版是 15.7，16.0 之後的文件都在 sap help 網站上以 HTML (or PDF) 方式呈現。
最有用的(但也最多內容的) 是二份 (Volume1 & Volume2) Administration Guide。

* SAP ASE System Administration Guide Volume 1
* SAP ASE System Administration Guide Volume 2


ASE 架構
Sybase ASE 可運行在多種作業系統上，常見的是 Windows 和 Linux 環境。ASE 是 multiple-thread based 的 Server，使用最少的 OS process (與 Oracle 很不同)。
詳細的技術架構圖，請參見 SAP ASE Architcture (PDF) <-- 內容很深。

ASE 資料庫的實體組成單位，分 Database, Disk, Device, 和 Segment 等概念(以下為 administration guide 裡的描述)

The physical disk or physical device is the hardware that stores the data.

A database device or logical device is all or part of a physical disk that has been initialized (with the disk init command) for use by ASE

A database device can be an operating system file, an entire disk, or a disk partition.

A segment is a named collection of database devices used by a database. The database devices that make up a segment can be located on separate physical devices.
A segment is a label that points to one or more logical devices. 

A partition is a subset of a table. Partitions are database objects that can be managed independently. You can split partitioned tables, so multiple tasks can access it simultaneously. 

You can place a partition on a specific segment. If each partition is on a different segment and each segment has its own database device, queries accessing these tables benefit from improved parallelism. 


總之，一個 database 會有至少一個 database device，而 database devices 裡的空間會被分成數個 segements : 預設是 system,  default , logsegment 三個 segments。
一個 segement 的空間可以由數個 database devices  裡的部份或全部組成。system segment 是給 database 裡的 system objects 用的, default segment 是給 data 用的，
logsegment 是給 transaction log 用的。 

Segment 扮演很重要的角色，好處之一是它可以改善 throughput
Splitting large tables across disks, including tables that are partitioned for parallel query performance

Separating tables and their nonclustered indexes across disks

Separating table partitions and index across the disks

Placing the text and image page chain on a disk other than the one on which the table resides, where the pointers to the text values are stored


Segment 扮演很重要的角色，好處之二是它可以控管空間
Tables or partitions cannot grow larger than their segment allocation. You can use segments to limit the table or partition size.

Tables or partitions on other segments cannot use the space allocated to objects on another segment.

The threshold manager monitors space usage.


ASE 安裝後檔案位置，OS user
通常我們 BASIS 不會單獨安裝 Sybase ASE，而是在安裝 ERP6.0, Solution Manager, 或其他 SAP application 時，由 SWPM "順便" 安裝 Sybase ASE。
只要提供 ASE 的安裝 source file 位置給 SWPM，SWPM 就會自動裝好。除了安裝 主程式，也會建立一個和 SAP Application 同名的 ASE Database server，
如安裝 SAP application 叫 PRD，預設就會建立一個叫 PRD 的 ASE database server，或說 DBSID = PRD，也會在 這個 server 裡建立內建的 databases 以及一個也叫 PRD 的 database。

在作業系統層，SWPM 會為 ASE 建立一個帳號 syb<dbsid>，預設密碼是 SWPM 裡給定的 master password。例如你的 ASE DBSID 是 DEV，就會建一個 sybdev，是PRD ，
就會建一個 sybprd 的作業系統帳號。在 Windows 或 Linux 上都是這個名字。這個帳號是用來運行 ASE 各項服務，管理者也可以登入這個帳號到 OS 層，做 isql 等 command line 操作。

在檔案系統層，安裝後會有一個 /sybase/<DBSID> 目錄，包含了 ASE 的程式，log, database 目錄。在 Windows 環境上是 <DRIVE>:\sybase\<DBSID> ，在 Linux 環境上是 /sybase/<DBSID>
如果你裝的 DBSID 是 PRD，就會有 /sybase/PRD 目錄。其他預設安裝時產生的重要目錄如下 (以 linux, DBSID=DEV 為例)

- 系統主程式目錄，config，log 

   /sybase/DEV/bin
   /sybase/DEV/ASE-16_0  ("16_0" 數字視所安以壞版本而定)

   這個 ASE-16_0 目錄裡有一個 DEV.cfg 檔，是 ASE 的系統參數設定檔，每當對 configuration 或 DB option  有修改時就會產生一個備份檔(有序號, 如 DEV.000000012)。
   子目錄 install 含有 dataserver , backup server, 以及 job scheduler 的 log files. 分別命名為 :
   
   DEV.log 
   DEV_BS.log
   DEV_JSAGENT.log

   若有空間不足或嚴重問題，請看 DEV.log。若有備份問題，請看 DEV_BS.log。若在 DBACockpit 裡排程沒有執行，備份根本沒有發生，請查看 DEV_JSAGENT.log。
   在 SAP on ASE 的環境裡，DBACockpit 裡的排程並不是利用 App server SM37 scheduler 去排程 ，而是利用 ASE 自己的 job scheduler 排程。根據經驗， ASE 的 job scheduler 常有問題，要小心。      

   此外，在 install 目錄裡，還有二個很重要的 script files

   RUN_DEV
   RUN_DEV_BS

   這二個 script files 是啟動 data server 和 backupserver 必要的，不能修改或刪除他們。但一般我們不會直接利用這二個 script file 去啟停 Sybase ASE。
   啟停 dataserver 一般可用 sapstart db 及 sapstop db 去做(<sid>adm 身份)。
 

- 系統內建 DB 的 data 及 trans log devices 目錄

  在 SAP on ASE 的環境裡， SWPM 會在 ASE 裡建立一些管理上必要的資料庫
  計有 
     master
     model
     saptempdb
     saptools
     sybmgmtdb
     sybsecurity
     sybsystemdb
     sybsystemprocs
     tempdb
  它們的用途這裡就不說明，總之是必要的。但要注意的是這些 database 若滿了，也會造成你的 DEV database 有問題，尤其 saptools, sybmgmtdb,  sybsecurity 等空間常常出問題，
  會間接造成 job scheduler 無法執行 job，備份不成功而倒致 log segement 滿了，DB 就不運作了。這個也要注意。
  這些內建 database 的 data/log file 目錄如下

/sybase/DEV/sybsystem     --> 存放 master, sybsystemdb , sybsystemprocs, sybmgmtdb 等 database 的 device files
  /sybase/DEV/sybtemp        --> 存放 tempdb database 的 device file
  /sybase/DEV/sybsecurity    --> 存放 sybsecurity database 的 device file
  /sybase/DEV/saptemp        -->  存放 saptempdb database 的 device file
  /sybase/DEV/sapdiag         -->  存放 saptools database 的 device file


- User Database (DEV)  Data 及 trans log devices 目錄  

  在 SAP on ASE 的環境裡，SAP 的資料是存在 User Database (此例為 DEV)
  預設上 SWPM 至少會建立以下二個目錄 
   /sybase/DEV/sapdata_1
   /sybase/DEV/saplog_1
   sapdata_1 應是佔用空間最大，日後成長最快的目錄，裡面有如  DEV_data_001.dat 這樣的檔案(預設全部空間指派給 'default' segment)，這就是 DEV database 的 data file。
   另外，saplog_1 裡有如  DEV_log_001.dat 這樣的檔案(預設全部空間指派給 'logsegment' segment)，通常是用來存放 transaction log，空間應不太會長大。
   視資料成長速度而定，若空間不夠，可以增加 file (可放同目錄或另外目錄)，並把 file 裡的空間指派為 default (data) segment 或 logsegment，
   以被用來當作 data or trans log 存放用。擴充空間的方式見其他技術說明項目。

ASE 安裝後 Process & Ports 使用

以在 Linux 上為例，若 DBSID = DEV，在 SAP & ASE 都啟動的狀況下，可以利用 netstat 和 ps 取得以下訊息
![image1.png](/s1221/image1.png)

上圖的 "netstat -an -ltpn | grep 490"  可以看到 4901, 4902, 4903 分別由 dataserver(pid=29040),  backupserver(pid=29131), 以及 jsagent(pid=29422) 所 LISTEN。
預設(Default)上，安裝 SAP & ASE 時SWPM 會找尋以下的 ports，作為 ASE 各 service 的 listen port :
Port 4901  :   ASE dataserver (database) 
Port 4902  :   ASE backup server
Port 4903  :   ASE job scheduler
你可以在 SWPM 安裝參數給定畫面裡修改這些 default value。若已裝好要改 port 請參照 Note 1729176 進行。

上圖的後半部 ps -ef | grep sybdev 是列出以 syb<dbsid> user 身份運行的 processes。可以看出來，dataserver(pid=29040) 是由 RUN_DEV script(pid=29038) 啟動的，backupserver(pid=29131) 是由 RUN_DEV_BS scripts 啟動的 ，而 jsagent (job scheduler，pid=29422) 是由 datataserver(pid=29040) 啟動的。 所以在 OS level ,  dataserver, backupserver, 和 jsagent 三個是 ASE 的三個 processes.


先寫到這裡。
