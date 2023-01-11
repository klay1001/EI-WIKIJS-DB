---
title: S2.1.1New Installation - Standard
description: 
published: true
date: 2023-01-07T10:02:29.729Z
tags: 
editor: markdown
dateCreated: 2022-12-19T01:58:46.887Z
---

# New Installation - Standard
## 經驗分享

進行安裝前，請先確定以下配置
1. SAP產品版本及其所用的 NetWeaver 版本
2. 作業系統種類，是 Windows, Linux, 還是 Unix(AIX, HP-UX, Solaris)
3. CPU 類型，如 x86-64(64bit), x86(32bit), Itanium(64bit), Power, PA-RISC, SPARC, 等。
4. 資料庫種類 (HANA, Sybase ASE, Oracle , MSSQL, MAXDB，或其他)

有了上述的配置，再去 [SAP PAM](https://support.sap.com/pam) 確認以下事項
1. 支援這樣的組合。
2. 支援的作業系統版本。
2. 支援的 SAP Kernel 版本。
3. 支援的資料庫版本。

再來是確定能夠下載到相關檔案及工具，一般全新安裝都會有以下五大要件
1. SWPM (Software Provisioning Manager)。
2. SAP Kernel。
3. SAP 產品的 Installation Export。
4. 資料庫安裝檔。
5. 資料庫用戶端安裝檔。

最後就是參看前人安裝的記錄，或是看官指南(沒有人前分享)，確定安裝的重要步驟。
看指南的好處是可得知在規劃期要注意事項，如作業系統參數準備，空間大小準備，已知問題如何避免等。可以讓你免於踩雷或在客戶面前下不了台。

切記，SAP 博大精深，很少有完美無失的安裝，官方文件永遠會少一些你才會遇到的關卡，所以臨場應變的技能就區分出能不能獨當一面了。

## 官方指南
* [Guide Finder for SAP NetWeaver and ABAP Platform](https://help.sap.com/docs/SAP_NETWEAVER/9e41ead9f54e44c1ae1a1094b0f80712/576f5c1808de4d1abecbd6e503c9ba42.html?language=en-US)。
* NetWeaver

## 前人實作文件
|NW Technology Service Item|||AS ABAP||||||
|---------|---------|---------|---------|---------|---------|---------|---------|---------|
| **Operating System** |||**Windows** |**L(U)inux** | **Windows** | **L(U)inux** | **Windows** | **Linux** | 
| S2.1.1New Installation - Standard||| [J9](/home/S2_SAP_NetWeaver_Skills/New_Installation_Standard/J9)|  |[J11](/home/S2_SAP_NetWeaver_Skills/New_Installation_Standard/J11)|W圖 |  |[J13](/home/S2_SAP_NetWeaver_Skills/New_Installation_Standard/J13), [J14](/home/S2_SAP_NetWeaver_Skills/New_Installation_Standard/J14) |