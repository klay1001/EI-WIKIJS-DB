---
title: SAP HANA Database
description: In-memory, column-based Database。未來 SAP 的系統都會移到 HANA 平台上。
published: true
date: 2023-01-07T10:44:16.677Z
tags: database, basis, hana
editor: markdown
dateCreated: 2022-07-27T10:28:35.384Z
---

# SAP HANA Database
## 資料庫組成
資料庫由一個 System DB 搭配一個或多個 Tent DB 運行。
System DB 是 name server，Tenet DB 是 index server，另外還有輔助的 XS Server 提供網頁。
SAP HANA 和其他資料庫有個不同點是他算是 SAP System，所以安裝時會需指定 SAPSID。所以當它和 SAP Application Server 裝在一起時，二者的 SID 要錯開不能相同。
## 儲存機制
實體儲存層有 data volume 及 log volume 二個區塊。
## Transaction Log 機制
## 備份機制
## 其他特點

# 搭配的 SAP 產品
Your content here

# 安裝

### Sizing
### 原廠文件
### 安裝檔案下載
### 安裝作業
### 安裝後作業

# 備份還原
### 利用 HANA Studio 備份
### 利用 AS ABAP DB13 備份
### 利用 HANA Cockpit 備份
### 利用 hdbsql command 備份
### 還原


# 定期維護工作
### 原廠文件
* [SAP HANA Administration Guide](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/latest/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html) 見其中的 "Administration Task at a glance"
* [SAP HANA Multitenant Database - Operation Guide ](https://help.sap.com/docs/SAP_HANA_PLATFORM/78209c1d3a9b41cd8624338e42a12bf6?locale=en-US&state=PRODUCTION&version=2.0.06)
* 依照 [SAP Note 2400024 - How-To: SAP HANA Administration and Monitoring](https://launchpad.support.sap.com/#/notes/2400024) 裡面規劃的日常維運項目，可依執行週期分別如下。
### 每日工作 (Daily)
* Administration : Database Backup
  確定每天 data backup 完成，若沒有完成，排除問題並儘早補做。
* Monitoring : Alert
  檢查 HANA Alert，處理事件及清除 Alert。
* Monitoring : Solution Manager (Optional)

### 每週工作 (Weekly)
* Administration : Security & Hot News
* Housekeeping : 清理 Backup Catalog，保留 42 天。
* Housekeeping : 清理 Trace files，保留 42 天。
* Housekeeping : 清理 Backup 或 Backint log，只留 50MB。
* Housekeeping : 清理 Technical Tables。
* Housekeeping : 重整 row store，讓f ragmentation < 30%
* Monitoring : 執行 Minicheck。
* Monitoring : 檢查 Trace files。


### 每月工作 (Monthly)
* Administration : Consistency Check。
* Housekeeping : 視 fragmentation 狀況重整 Data files。
* Housekeeping : 視 log segment 狀況 RECLAIM。

### 每季工作 (Quartly) 或 on-demand
* Administration : Backup/Restore 演練。
* Administration : Replication Takeover 演練。

### 每年工作 (Anuual) 或 on-demand
* Configuration : Statement Hints。(每半年)
* Configuration : Timezone。
* Administration : HANA patch 或升級。
* Monitoring : Indexes (for S/4, run report SHDB_INDEX_ANALYZE)

# 不定期嚴重事件(critical issues)處理
### 原廠文件
* Database 連不上 (Note:1999020)
* 記憶體吃緊 (Note:1999997)
* 效能不良 (Note:2000000)
* 高CPU用量 (Note:2100040)
* 資料損壞(corruptions) (Note:2116157)
* Blocked GC(Garbage Collection) (Note:2169283)
* 不正確的資料產出 (Note:2222121)
### 原廠 Guided Answer

# 效能及參數調校
### 原廠文件
* SAP HANA Performnace & Tuning Guide。
### 原廠 Guided Answer

### 前人實戰經驗




