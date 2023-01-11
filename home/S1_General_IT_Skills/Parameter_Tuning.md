---
title: S1.2.1.10	Parameter Tuning
description: 
published: true
date: 2022-12-23T07:16:58.019Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:38:17.236Z
---

# Parameter Tuning
根據note 1171650 - Automated Oracle DB parameter check
![image1.png](/s12110/image1.png)
提供的script:
![image2.png](/s12110/image2.png)
PS:依據不同oracle去選用。
Tx:DB02
![image3.png](/s12110/image3.jpg)
跑出結果匯出Excel
![image4.png](/s12110/image4.png)
![image5.png](/s12110/image5.png)
其中檢查出非OK即為原廠建議需調整的參數:
![image6.png](/s12110/image6.png)
調整語法參考:
alter system set EVENT=
 '10142 trace name context forever, level 1',
 '10183 trace name context forever, level 1',
 'xxxxx trace..<skip>....., level y',
 'xxxxx trace..<skip>....., level y',
 '44951 trace name context forever, level 1024'
 SCOPE=SPFILE;
  
  alter system set "_FIX_CONTROL"=
 '6055658:OFF','6120483:OFF',
  <skip..>
  <skip..>
 '14255600:ON','14595273:ON'
 SCOPE=SPFILE;

