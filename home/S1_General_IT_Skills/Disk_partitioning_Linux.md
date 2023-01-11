---
title: S1.1.1.2	Disk partitioning
description: 
published: true
date: 2022-12-19T06:40:08.736Z
tags: 
editor: markdown
dateCreated: 2022-12-19T01:29:25.035Z
---

# Disk partitioning
在不同資料庫安裝 SAP，建議先行規劃相關 partitions 

--------- SAP AS ABAP ----------  

/sapmnt, /usr/sap 

 

---------- Database ---------- 

Oracle: 

/oracle 

 

Sybase ASE: 

/sybase 

 

HANA: 

/hana/shared, /hana/data, /hana/log 

 

MaxDB: 

/sapdb, /sapdb/program, /sapdb/data 
參考資料 

http://linux.vbird.org/linux_basic/0130designlinux.php#partition_install (基本概念，中文) 

https://blog.gtwang.org/linux/parted-command-to-create-resize-rescue-linux-disk-partitions/ (實務操作，中文) 

https://documentation.suse.com/zh-tw/sles/12-SP5/html/SLES-all/cha-advdisk.html (SLES12 with yast，中文) 

https://maxdb.sap.com/doc/7_6/44/118d969c471a75e10000000a1553f6/content.htm (MaxDB，英文) 
