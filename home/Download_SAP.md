---
title: Download_SAP
description: 
published: true
date: 2022-12-12T08:18:00.005Z
tags: 
editor: markdown
dateCreated: 2022-12-12T07:11:50.660Z
---

# 安裝 SAP 系統
### 當你研讀了半天，總會需要裝一台來看看。對於完全沒有裝過 SAP 系統的人，請先了解以下幾個概念：
•	安裝工具是 Software Provisioning Manager (SWPM)，需要到 [SWDC](https://www.google.com/url?q=https%3A%2F%2Fsupport.sap.com%2Fswdc&sa=D&sntz=1&usg=AFQjCNG7M-XkRFbPUTC6JGrTIGLScsT6vA) 下載，不同 OS 平台有不同的下載檔，且這個工具隨時在更新，建議使用最新的。但是你需要的不只是 SWPM，它只是個工具，並不含要安裝產品的實際檔案。你還需要針對要安裝的產品，下載它的 Kernel, Export, Language 等檔案。

•	你下載的檔案若是 .SAR 或 .SCA 檔名的，是 SAP 專有的壓縮格式，要用 SAP 提供的工具 [SAPCAR](https://sites.google.com/a/soetek.com.tw/sapbasis/home/installation/saptools-sapcar) 來解壓縮。SAPCAR 是一個 command line 工具，用法和 unix 的 tar 十分相似。若下載的檔案是 .ZIP 檔名，可直接解壓，不需要用 SAPCAR 工具。

•	除了作業系統之外，其他的軟體(包括 Database Software)都必須從 SAP SWDC 下載。你不可以自行到 database vendor 官網上下載 database 來裝。有些平台或 database 不需要先安裝 database，而是在執行 SWPM 的過程中，會被自動安裝，但你仍要先下載 database 的安裝檔。

•	一般 SAP 的安裝檔案都很大，你最好準備至少 60GB 的空間在 server 上專門來存放它們。除了存放安裝程式的空間，以及 OS 本身的空間，大部份的空間需求是 Database 資料，通常都至少要個 100GB 以上，若是 IDES 系統，要 400GB。

•	你可以發 OSS Message 要求寄發 DVD/Blue-ray Disc 給你(或客戶)，但通常我們會直接由 SWDC 下載。

•	請熟悉並適應研讀 SAP Note 的本領，做 BASIS 的工作，會需要大量英文閱讀，尤其是花在官方文件及 Note 上。不管是 Master Guide, Installation Guide, 還是其他的 Guides，裡面都會有一堆連結到相關的 SAP Note 去，而且很多 SAP Note 又串到別的 Notes 去，你要有耐心能盡可能的把相關的 Note 都讀到，不一定每行每字，但要能判斷那些是有關要讀而那些是可忽略的，不然會花很多時間，一下子就沒信心了。

•	除非你曾經裝過同一產品同一版本同一作業系統同一資料庫種類的組合，否則請**一定一定一定一定一定**要先讀過 Installation Guide。

## 安裝前準備工作
開始安裝前，你需要做好準備，包括 :
1.	決定要安裝的產品及版本。
2.	決定要使用的資料庫及作業系統平台，以及安裝架構。
3.	查詢 PAM 確定上面的組合是支援的。
4.	下載並研讀該產品的 Installation Guide。
5.	到 SWDC 下載必要的安裝檔，以及工具 (SWPM)。
6.	準備伺服器環境(或 VM)。
### 1. 決定要安裝的產品及版本
如果是 ERP，通常是要安裝最新的版本。若不用 S/4HANA，一般是要安裝 EHP7 ERP6.0。若是要安裝 EP (Enterprise Portal), PI (Process Integration), BPM/BRM， 或其他 SAP Java based 的系統，
一般是安裝 EHPx NetWeaver 7.4。
#### 提醒 : 請先研讀一下 
### 2. 決定要使用的資料庫及作業系統平台，以及安裝架構
一般使用 Windows 或 Linux。Windows 請用 Windows 2012(R2) 64bit，現在沒人用 32－bit windows 了。
而 Linux 建議使用 SUSE Linux Enterprise Server 11 SP2 或 SP3。這二個都是 x86_64bit 的版本，一般 PC Server 也都可以跑，且可以放在 VM 上。作業系統你要自已去 Microsoft 或 SUSE (Novell) 網站上下載。其中 SUSE 有專門提供一個 for SAP 的 ISO 檔，建議用這個 ISO 來裝。

資料庫通常會用 Oracle, MSSQL, 或 Sybase ASE。以我們碩益的環境及能下載的軟體，Oracle 及 Sybase 是首選，而 MSSQL 因為沒有下載權限，比較少用。
SAP 對 Oracle 的支援目前還是最好，Sybase ASE 的管理較麻煩。但長遠來看，Oracle 可能不見得會有優勢，因為與 SAP 關係漸形漸遠了。

我這裡提到的安裝架構，是指要集中安裝，或分散安裝，或有 HA/Cluster 的安裝。除了一些 Standalone Engine 之外，一個完整的 SAP 系統包括 Application 及 Database 二個部份。一般我們會把 Application 和 Database 裝在同一個 server (同一個 OS 裡)上，叫做 Standard Installation。當然也可以把 Application 和 Database 分開，這叫做 Distributed Installation。另外，若要避免 Single Point of Failure，也可以做複雜的 High Availability Installation (配合 OS level 的 cluster software)。
### 3. 查詢 PAM 確定上面的組合是支援的

SAP 對於產品所能運行的環境有十分嚴格的規定，不然花了一大堆工夫裝不起來，是很令人沮喪的。
所以當你決定好產品版本，OS，及 DB 之後，你必須到 Product Availability Matrix (PAM) 網站上去確認。
若不支援，就需要修改你的設計。務必要能找到支援的組合。這個組合可能也隱含需要下載一些特別的 Kernel Patch Level 或其他手動工作。

### 4. 下載並研讀該產品的 Installation Guide

如果你在碩益找不到前人的安裝記錄文件，或是是第一個安裝某個產品的人，恭喜您，先讀 Installatin Guide 吧。

Installation Guide 可以在 http://help.sap.com 裡找到，如果你是裝 EHP7 ERP6.0，請連結到 http://help.sap.com/erp607 ，如果你是裝 S/4HANA，請連結到 http://help.sap.com/s4hana ，如果是裝Java based 如 EP 等，連結到 http://help.sap.com/nw74 。
這些連結可能會隨時間而改變，或有新版，所以你最好過一陣子從http://help.sap.com 主頁進去，查看是否有新的版本，並記下它的 URL。

以 EHP7 ERP6.0 為例，進入 http://help.sap.com/erp607 後，看到如下畫面
![Image](https://img.onl/KbldF =500x250)
點選 Installation and Upgrade Information 來跳到頁面下方關於 Installation 的部份。
![Image](https://img.onl/nkIgam =500x250)
這裡你就可以看到 Installation Guide 的連結，它是再連到另一個網頁(SAP Service Marketplace) 去。再點下去，會先有一個 agreement 頁面，按 I Agree, 然後到下頁 (要登入了)
![Image](https://img.onl/1OngD =450x250)
上圖中點第三項，你就可以看到 PDF 了。建議你把 PDF 下載下來。
![Image](https://img.onl/4UVGeH =450x400)
不過這一份只有針對 ERP 的注意事項，並沒有教你如何去操作 SWPM 安裝工具。
所以你也要把上上圖中的第四項 (Software Provisioning Manager) 的文件也下載下來。
點到第四項後，會要你選資料庫及作業系統。如下圖
![Image](https://img.onl/5Xhz5l =300x150)
以 Sybase ASE 資料庫 ＋ Linux 為例，請點 SAP Adaptive Server Enterprise 連結(現在 Sybase ASE 已改名叫 SAP ASE 了)然後你再找到 Operating System 為 Unix (AIX, HP-UX, Linux, Solaris) 這節。如下圖 
![Image](https://img.onl/eKruHA =700x50)
你會在上圖的第一行看到右方有三個 Technical Stack : ABAP , Dual Stack ABAP+Java, 以及 Java。你要選其中一個。
一般裝 ERP 要選 ABAP，裝 EP 要選 Java，若是裝 Solution Manager，要選 Dual Stack。點下去就可以看到 PDF 了。
![Image](https://img.onl/xirHLl =500x250)
請下載這個 PDF。
有了這二份 PDF，就開始 K 吧。第一次一定會很久，但多讀幾份後，就會自動省略掉一堆重複的廢話，找到不同的部份。
請注意，第二份 PDF 是依 OS+Database 不同而有不同的 PDF，所以你若要裝別的 Database or Operating System，請選用對應的 PDF。

還有一個問題，文件有 167 頁，要如何開始？其實苦工是免不了的，沒有什麼捷徑。

對於第一次安裝的人，我建議從頭開始，尤其是 Chapter 2 Installation Options 一章，對於不同的安裝架構有詳細的說明，你讀完一次有概念後，以後再看其他 Installation Guide 都大同小異可以略過。但一定要至少讀過一次而且有概念。

我通常會先看 Chapter 3 Planning，裡面有 Planning Check List 以及 Hardware/Software Requirement，先確定你的環境是 OK 的，以及那些必要的調整(如 swap space, disk space requirement)。這個動作通常在做安裝前就要先看看，尤其是要提供 Spec 給客戶時，要先查一下有沒有要客戶先準備的事。

再來是看 Chapter 1.3 SAP Notes for installation，把裡面的 Note 讀一下。這要花些時間，但裡面通常會寫安裝時可能遇到的問題，以及某些 DB/OS 版本組合下會要做的 workaround。SAP 很少會修改 Installation Guide，但 Guide 裡說的通則通常在執行時會有一些小問題，SAP 只好用 Note 來做補充，而 Note 也會常增修內容。所以為了減少踩到地雷的狀況，建議沒事就讀一讀。不讀的話，風險自負。

再來是 Chapter 4 Preparation，裡面有說明在你執行 SWPM 前，你的作業系統環境要準備的工作，Shared File Directory，以及要下載那些檔案等，你必須先完成這節裡說的工作，才能執行 SWPM。這一章有很多安裝後的檔案架構說明，十分重要且值得熟記。


Chapter 5 Installation 是你要執行 SWPM 時的說明，照做就對了。

Chapter 6 Post-Installation 千萬不要忽略。執行完 SWPM 不代表系統可以立即給顧問或客戶用，還有很多要連到 SAP 系統去做的 configuration。所以請務必要完成 Post-Installation 一章裡說的動作。

以本人的經驗，Installation Guide 是一份十分有教育意義的文件。它可以讓你對 SAP 安裝架構有完整的了解。
尤其是在做 High-Availability 安裝時，更是重要。另外一般初入 BASIS 工作的人，搞不懂 SAP Global Host/File System 與 SAP Transport Host/Directory ，以及 SAP 裝完後 /sapmnt  /usr/sap/<SID> 之間複雜的 symbolic link 關係，在 Installation Guide 裡都有詳細的解釋。這是我唯一能在 SAP 文件裡找到完整說明的地方。

So, Installation Guide is not just for installation, it's also a training material。

### 5. 到 SWDC 下載必要的安裝檔，以及工具 (SWPM)
舊版的 SAP 系統的安裝，是利用光碟片上的 sapinst 程式來執行安裝動作。在新的(現在)版本，你必須要使用 Software Provisioning Manager (SWPM) 來做安裝。你可以到 http://service.sap.com/sltoolset 去下載它。通常下載最新的版本，並選用你要安裝的作業系統平台。

只有 SWPM 是不夠的。SWPM 只是個導引你進行安裝的 java-based 介面。在安裝過程中，它會問你很多的問題，包括其他的安裝檔案放在那裡。所以在執行安裝前，你要先 download 這些檔案，並放在與 SWPM 同一台 server 上。
請參見 Installation Guide Chapter 4.7 Preparing Installation Media，它會告訴你去那裡下載。你看過一次照著下載，久而久之自然就知道新的版本要去那裡下載了。
  
一個重要的概念是，要從零到有的安裝，通常要下載(以 ERP 為例)：
  •	安裝工具 : SWPM
	•	解壓工具 : SAPCAR
	•	SAP Kernel (7.x)
	•	Export files (ERP 的起始資料, 最大的幾個檔)
	•	Language files (安裝完 post-installation 要上語言時要用)
	•	Database 安裝檔
	•	Database Client 安裝檔
	•	有時會需要 SAP Cryptographic Library
  這些檔安都要備齊，並上傳到接下來要準備好的伺服器環境上。
### 6. 準備伺服器環境(或 VM)
  你需要先安裝 Operating System，然後依 Installation Guide 裡說的做必要的 OS 參數調整(如 timezone, swap space, 某些 library, 關掉 firewall等, global directory / transport directory mount)。
某些特殊安裝的狀況下，還要先手動建立 OS user。你也要保留足夠的空間給安裝檔及 SWPM。通常請預留一個獨立的 mount 給 /sapsrc 目錄，把它們放在裡面解壓。 
## 安裝及安裝後設定
你若完成了上面的準備動作，就可以開始進行安裝了。在你解壓的 SWPM 目錄裡，會找到一個叫做 sapinst 的執行檔。
它會跳出一個 GUI 介面，導引你進行安裝。正真的安裝動作開始前，SWPM 介面會問一大堆問題，第一次安裝的人會遇到麻煩，就是不知要如何回答問題。 這個我也沒有快解，總之多裝幾次就知道了。
而且介面會隨 SWPM 更版而有些不同，所以必須隨機應變。

不過，真正困難的不是回答問題。而是安裝過程中跳出的錯誤。錯誤會造成 SWPM 停止，並在畫面上顯示錯誤訊息。
這時你一定要到 SWPM 的 log 目錄去找到最新的 log file，然後找到有ERROR 字眼的地方。這非常不好找，而且眼睛要快。然後去 Google, SAP SCN, 或 SAP Note 裡去找有沒有人遇到類似的問題。根據經驗，很多問題其實已經寫在相關的 SAP Note 裡了，只是在看 Installation Guide 時沒有去看相關的Note。

sapinst 程式對於安裝的過程有詳細的記錄，所以若遇到很麻煩的問題，是可以先把它關掉，處理完後，再重啟 sapinst (要刪它的 log )，他會問你要continue 還是重新開始。這時選 continue 它就會快速的跑到你上次卡關的地方，繼續下去。

請注意，SAP 安裝絕非易事，不是 Windows 安裝 "下一步" "Next" "OK" 就可以搞定的。第一次安裝不能成功是正常的。
重點是在要能決解安裝時遇到的問題，才是顧問應具備的能力。請盡量先自力救濟，求人前先求己，多解決幾次問題，就能成長。前人的 SOP 是死的，版本環境一變，就又不同了。

若你能看到 sapinst 最後 complete 的畫面，那真是恭喜你，已經完成一大部了。

ERP 裝完後，不要忘記做 Post-Installation 設定。通常包括

1.	Logon to SAP System (via SAP GUI)
2.	Import Profile 及調整 Profile
3.	做 SE06/SICK 檢查
4.	建立 TMS 環境
5.	建立一個 BASIS 專用的 user 在 client 000
6.	安裝 Support Package (視狀況，這又是另一個故事了，會花數天)
7.	安裝 Language Pack 及 做 Supplementation
8.	建立工作的 client，並進行 client copy 
9.	SGEN precompile ABAP 程式
10.	申請並安裝 License Key
11.	OSS1 設定 SAP Router 連線
12.	建立 Standard Job
13.	備份設定及排程
14.	其他族繁不及備載的項目不少......

好了，說是一回事，做是另一回事，先動手裝一台吧 !! 
























