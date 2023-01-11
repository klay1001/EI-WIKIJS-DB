---
title: S1.2.1.2	Installation (Win: server.cmd)
description: 
published: true
date: 2022-12-23T06:32:54.164Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:34:09.799Z
---

# Installation (Win: server.cmd)
Target主機安裝Oracle 
![image1 ](/s1212/image1.png)  
![image2 ](/s1212/image2.png)  
![image3 ](/s1212/image3.png)  
![image4 ](/s1212/image4.png)  
![image5 ](/s1212/image5.png)  
![image6 ](/s1212/image6.png)  
![image7 ](/s1212/image7.png)  
![image8 ](/s1212/image8.png)  
![image9 ](/s1212/image9.png)  
![image10 ](/s1212/image10.png)
![image11 ](/s1212/image11.png)
![image12 ](/s1212/image12.png)
Target主機執行sapinst
![image13 ](/s1212/image13.png)
![image14 ](/s1212/image14.png)
![image15 ](/s1212/image15.png)
![image16 ](/s1212/image16.png)
![image17 ](/s1212/image17.png)
![image18 ](/s1212/image18.png)
![image19 ](/s1212/image19.png)
![image20 ](/s1212/image20.png)
![image21 ](/s1212/image21.png)
![image22 ](/s1212/image22.png)
P2ssw0rd
![image23 ](/s1212/image23.png)
![image24 ](/s1212/image24.png)
![image25 ](/s1212/image25.png)
![image26 ](/s1212/image26.png)
![image27 ](/s1212/image27.png)
![image28 ](/s1212/image28.png)
![image29 ](/s1212/image29.png)
![image30 ](/s1212/image30.png)
![image31 ](/s1212/image31.png)
![image32 ](/s1212/image32.png)
![image33 ](/s1212/image33.png)
![image34 ](/s1212/image34.png)
![image35 ](/s1212/image35.png)
![image36 ](/s1212/image36.png)
![image37 ](/s1212/image37.png)
![image38 ](/s1212/image38.png)
![image39 ](/s1212/image39.png)
![image40 ](/s1212/image40.png)
![image41 ](/s1212/image41.png)
使用OraBrCopy工具, 使用p37adm登入source db系統
ora_br_copy.bat -generateFiles -targetSid P37 -listenerPort 1521 -password ab12AB##cd34 (注意:若使用-forceLogSwitches選項DB會被shutdown) 
![image42 ](/s1212/image42.png)
![image43 ](/s1212/image43.png)
![image44 ](/s1212/image44.png)
![image45 ](/s1212/image45.png)
![image46 ](/s1212/image46.png)
![image47 ](/s1212/image47.png)
![image48 ](/s1212/image48.png)
![image49 ](/s1212/image49.png)
![image50 ](/s1212/image50.png)
Axiomtek2017
![image51 ](/s1212/image51.png)
![image52 ](/s1212/image52.png)
![image53 ](/s1212/image53.png)
![image54 ](/s1212/image54.png)
![image55 ](/s1212/image55.png)
![image56 ](/s1212/image56.png)
![image57 ](/s1212/image57.png)
換p37adm登入target系統操作
![image58 ](/s1212/image58.png)
1.1 CONTROL系統自己會copy，確認一下路徑中有就好(instdir目錄) 
![image59 ](/s1212/image59.png)
1.2 copy initP37.ora至D:\oracle\P37\11204\database\下(binary的目錄) 
![image60 ](/s1212/image60.png)
Create an Oracle spfile from init<TARGET_DBSID>.ora  profile as follows:(用p37adm登入系統)
sqlplus /nolog
connect / as sysdba
create spfile from pfile;
exit

![image61 ](/s1212/image61.png)
![image62 ](/s1212/image62.png)
連線備份檔案位址(須給予足夠權限的帳號)
![image63 ](/s1212/image63.png)
Brtools 還原oracle
copy source主機D:\oracle\ORA\11204\database\initP37.sap至 target主機目錄為D:\oracle\P37\11204\database\initP37.sap

![image64 ](/s1212/image64.png)
建立P37 oracle目錄，由原先P37目錄修改
![image65 ](/s1212/image88.png)
![image66 ](/s1212/image66.png)
copy source主機backup detail log E:\oracle\P37\sapbackup檔案至E:\oracle\P37\sapbackup
![image67 ](/s1212/image67.png)
![image68 ](/s1212/image68.png)
![image69 ](/s1212/image69.png)
brrestore -m full -b bexjhzwj.and -d disk -c 
![image70 ](/s1212/image70.png)
![image71 ](/s1212/image71.png)
![image72 ](/s1212/image72.png)
![image73 ](/s1212/image73.png)
手動執行CONTROL.SQL
![image74 ](/s1212/image74.png)
![image75 ](/s1212/image75.png)
exit
從備份位置copy需要的log檔下來用brtools解壓

![image76 ](/s1212/image76.png)
![image77 ](/s1212/image77.png)
![image78 ](/s1212/image78.png)
![image79 ](/s1212/image79.png)
![image80 ](/s1212/image80.png)
![image81 ](/s1212/image81.png)
![image82 ](/s1212/image82.png)
![image83 ](/s1212/image83.png)
select open_mode from v$database;
會看到database已經open
手動加Temp tablespace透過CONTROL.SQL去找指令

![image84 ](/s1212/image84.png)
![image85 ](/s1212/image85.png)
換回administrator登入後，繼續執行sapinst 
![image86 ](/s1212/image86.png)
![image87 ](/s1212/image87.png)

