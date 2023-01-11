---
title: S1.2.1.4	sqlplus logon and simple query
description: 
published: true
date: 2022-12-23T06:49:49.391Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:35:24.430Z
---

# sqlplus logon and simple query
WINDOWS環境
用<SID>ADM登入
UNIX/LINUN環境
用ORA<SID>登入
執行指令: >sqlplus / as sysdba
查詢資料庫目前狀態: READ/WRITE(代表資料庫目前為OPEN狀態)
SQL>select open_mode from v$database;
圖:
  ![image1.jpg](/s1214/image1.jpg)
