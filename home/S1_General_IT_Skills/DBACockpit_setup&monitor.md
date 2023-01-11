---
title: S1.2.2.8	DBACockpit setup / monitor
description: 
published: true
date: 2022-12-23T07:49:59.625Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:43:38.502Z
---

# DBACockpit setup / monitor
1.2.2.8 SAP (Sybase) ASE - DBACockpit
裝完 SAP on Sybase ASE 後，SWPM 不會安裝任何管理 Sybase 的工具。
有別於 Oracle 和 HANA,可以利用 SAP GUI 的 T-code (DB13, DB12, ST04 等) 進行管理及備份排程，
SAP 提供給 Sybase ASE 的管理 T-code 只有一個叫  DBACOCKPIT (即使用 ST04 也是最後連到 DBACockipit)。

然而 DBACockpit 是 ABAP Webdynpro 的應用程式，也就是說，是要跳 Web 畫面，
不是在 SAP GUI 裡的 DYNPRO 程式。
這增加了外網使用的困難，因為 ABAP Web dynpro 畫面不能經由  SAP Router 連入，
而且在 Browser 中必須要有 
URL  domain name 解析機制如 DNS & hosts file 設定。

DBACockpit 不是只針對 Sybase ASE 設計，SAP 有意在未來用 DBACockpit 來統一對 
HANA, ASE, 或其他資料庫的管理。
此外，DBACockpit 可以設定管理遠端的系統，所以有些客戶的環境裡，利用 Solution Manager 
或是 P, Q, D 裡某一台設定
DBACockpit 去管理所有的 Sybase。本文不先不考慮集中管理設定方法。

## 如何啟用 DBACockpit
要啟用 DBACockpit ，必須要先 activate ICF 裡的 service，請先參考以下二份 notes
•	1245200 - DBA : ICF Service-Activation for WebDynpro DBA Cockpit
•	1605680 - SYB Troubleshoot the setup of the DBA Cockpit on Sybase ASE
重要的動作有:
1.	上最新的 DBACockpit notes (非常重要，DBA Cockpit 的 bug 非常多一定要上到最新的 note)
2.	啟用 ICF services for DBA Cockpit
3.	系統參數設定 for HTTP
4.	使用者電腦的 DNS / hosts file 名稱解析
 個別動作描述如下
### 1)上最新的 DBACockpit notes
參見 note 1605680 裡的內容，找到對應 SAP_BASIS 版本的 note 去更新 DBA Cockpit，如下表。
   每一個 note 都有複雜的子  note 要上，所以不要手動去下載，請用 snote 自動下載。

|SAP Note	|SAP Basis Version|
|---|---|
|1558958|SAP Basis 7.02 / 7.30|
|1619967|SAP Basis 7.31|
|1882376|SAP Basis  7.40|
|2293673|SAP Basis  7.50|
|2380028|SAP Basis  7.51|
|2615827|SAP Basis  7.52|
|2743221|SAP Basis  7.53|

### 2) 啟用 ICF services for DBACockpit
要啟用 Web Dypro ABAP 相關 ，及 Web Dynpro for DBACockpit 相關的 ICF services.
叫用 T-Code:  SICF_INST 
在 Technical Name 欄裡填 "WEB DYNPRO ABAP"  ，按 Execute (F8) 去啟用.
在 Technical Name 欄裡填 "WEB DYNPRO DBA COCKPIT"  ，按 Execute (F8) 去啟用.
參見 Note 1245200 裡所描述的
### 3) 系統參數設定 for HTTP
   叫用 T-Code:  RZ10
     設定以下參數 , 並重啟 SAP instance 使生效  
•	     login/create_sso2_ticket = 2
•	     login/accept_sso2_ticket = 1
•	     icm/server_port_0 = PROT=HTTP,PORT=80$$,PROCTIMEOUT=600,TIMEOUT=600
•	     icm/server_port_1 = PROT=HTTPS,PORT=443$$,PROCTIMEOUT=600,TIMEOUT=600
•	             icm/host_name_full = myserver.mydomain.com.tw
    其中 icm/host_name_full 是當你打 T-code DBACOCKPIT 時，GUI 跳出 Browser 所帶的 URL ，
所以必須要能在使用者電腦上解析得出來。
### 4) 使用者電腦的 DNS / hosts file 名稱解析
   當使用者在 SAP GUI 裡打 DBACOCKPIT 時，會跳出 Browser，
網址類似如 https://myserver.mydomain.com.tw:8000/sap/bc/.....
      如果你的 Browser 出現找不到 Server，很有可能是根本解不出 Server IP。
      請確認在該電腦去執行 ping，能 ping 到 myserver.mydomain.com.tw (舉例)。
此外，要確定不是在外網，若在外網必須要架 Web Dispatch 且解出的是 internet domain, 
通常不會這麼做。
若沒有 DNS 設定，請把    <IP address>   myserver.mydomain.com.tw  一行設定在使用者電腦的 
hosts file 裡。
## 初啟用 DBACockpit 後的設定 (for Sybase ASE)
啟用後，有針對 Sybase ASE 的資料庫，請做以下的設定，以增進 ASE 效能

1.	啟用 ATM (Automatic Table Maintenance) 
2.	進行 Server Configuration 檢查及調整
3.	設定備份排程

