---
title: S1.2.2.4	shutdown/startup ASE
description: 
published: true
date: 2022-12-23T07:33:45.865Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:41:30.775Z
---

# shutdown/startup ASE
1.2.2.4 SAP (Sbyase) ASE - 資料庫的啟動/停止
除非你管理的 ASE 資料庫不是 SAP ERP, NetWeaver Gateway, Solution Manager 等 AS ABAP 系統所用，
否則啟動和停止 SAP ASE 的動作都是利用 startsap 和  stopsap command 來進行。
另外在 application kernel 裡有一個程式叫  sybctrl ，也可以用來單獨啟停 Sybase

利用 application 層工具/command 的方法
停止 SAP ASE (stopsap)
請先以 <sid>adm 作業系統帳號登入 ASE 作業系統
開啟 Terminal 或  cmd console

執行以下 command 可先停掉 application
       stopsap  r3

執行以下 command 可一次先停掉 application， 再停掉 ASE
       stopsap  

執行以下 command 可單獨停掉 ASE (若 Application 已先停)
      stopsap db
啟動 SAP ASE (startsap)
請先以 <sid>adm 作業系統帳號登入 ASE 作業系統
開啟 Terminal 或  cmd console

執行以下 command 可單獨啟動 ASE 
      startsap db

執行以下 command 可單獨啟動 application (若 ASE 已啟動)
       startsap  r3

執行以下 command 可一次先啟動 ASE，再啟動 application
       startsap  

停止 SAP ASE (sybctrl)
請先以 <sid>adm 作業系統帳號登入 ASE 作業系統
開啟 Terminal 或  cmd console

執行以下 command 可單獨停掉 ASE (若 Application 已先停)
      sybctrl stop
啟動 SAP ASE (sybctrl)
請先以 <sid>adm 作業系統帳號登入 ASE 作業系統
開啟 Terminal 或  cmd console

執行以下 command 可單獨啟動 ASE 
      sybctrl start

利用 ASE 自己的方法(待確認)
停止 SAP ASE
請先以 syb<dbsid> 作業系統帳號登入 ASE 作業系統
如果是 Windows，可以到 service.msc 裡(服務管理)，把 RUN_<SID> (sybase ASE server) 和 RUN_<SID>_BS (bakcup service) 
二個服務停掉
如果是 Linux/Unix, 可以 isql 登入，直接下 shutdown 

 啟動 SAP ASE
請先以 syb<dbsid> 作業系統帳號登入 ASE 作業系統
如果是 Windows，可以到 service.msc 裡(服務管理)，把 RUN_<SID> (sybase ASE server) 和 RUN_<SID>_BS (bakcup service) 
二個服務啟動
如果是 Linux/Unix，要利用 startserver 和 RUN_<SID> script , 如下 

     # start sql server
     cd /sybase/<SID>/ASE-16_0/install
     nohup ./startserver -f RUN_<SID> &
     # start Backup Server
     nohup ./startserver -f RUN_<SID>_BS &
