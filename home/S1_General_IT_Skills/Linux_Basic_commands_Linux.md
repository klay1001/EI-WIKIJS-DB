---
title: S1.1.1.1 Linux Basic Commands
description: 
published: true
date: 2023-01-05T03:00:15.675Z
tags: 
editor: markdown
dateCreated: 2022-12-16T01:49:29.921Z
---

# Linux Basic Commands

1.	pwd(找尋目前所在的路徑)
![image1.png](/s1111/image1.png)
2.	cd(變更至指定路徑)，常與 pwd搭配使用
![image2.png](/s1111/image2.png)
cd .. 回至上一層目錄
cd  回至根目錄 ( / )
cd -  回至上一個指定的目錄
![image3.png](/s1111/image3.png)
3.	ls(目錄的內容，預設是當前目錄)
ls -l 顯示目錄明細
ls -t 依時間後前排序，搭配 r 則依時間前後排序
ls -a 顯示隱藏的檔案
ls -R 顯示指定目錄之中包含子資料夾的內容
![image4.png](/s1111/image4.png) 
4.	cat顯示檔案內容
cat file1 file2 > file3 將 file1, file2合併成新檔案 file3
![image5.png](/s1111/image5.png) 
5.	cp 複製檔案
cp -p 複製檔案包含原本時間、權限
cp -r 複製目錄時包含子資料夾
![image6.png](/s1111/image6.png)  
6.	mv 移動檔案(目錄)或重新命名
![image7.png](/s1111/image7.png)  
![image8.png](/s1111/image8.png) 
7.	mkdir, rmdir建立資料夾及刪除資料夾，建立資料夾後搭配權限設定。刪除資料夾前要先確認為空資料夾才能被刪除
![image9.png](/s1111/image9.png)  
8.	rm刪除檔案或目錄
rm file1
rm -rf folder1刪除 folder1目錄(與 rmdir相比較為簡易)
![image10.png](/s1111/image10.png)  
9.	touch 新增檔案，新增完為空白檔案，依情況可進行編輯
![image11.png](/s1111/image11.png)  
10.	sudo切換至 SuperUser身份執行命令，與 su指令不同， su 切換時要輸入欲切換身份之密碼， sudo切換時只需輸入目前帳號之密碼
![image12.png](/s1111/image2.png)     
11.	df顯示 file system大小
df -k以KB為單位顯示
df -m以MB為單位顯示
df -g以GB為單位顯示(AIX support)
![image13.png](/s1111/image13.png)     
12.	du顯示目錄內空間大小
du -k, du -m
![image14.png](/s1111/image14.png)  
13.	tail顯示檔案最後顯示之資訊，預設顯示最後10列內容(空白列亦算)
tail -n 30顯示最後 30列資訊
![image15.png](/s1111/image15.png)  
14.	chmod變更檔案或目錄可執行的權限，檔案類型與權限常分為drwx，群組依序為檔案擁有者權限、權限所屬群組權限、其他人權限
![image16.png](/s1111/image16.png) 
![image17.png](/s1111/image17.png)  
![image18.png](/s1111/image18.png)  
15.	chown變更檔案或資料夾擁有者及檔案群組
![image19.png](/s1111/image19.png)  
16.	kill刪除系統中 process，常搭配 -9(force)使用
kill -9 PID
![image20.png](/s1111/image20.png)  
17.	uname查詢系統基本資訊
uname -[option]
-s, --kernel-name 輸出核心名稱
-n, --nodename 輸出網路節點上的主機名
-r, --kernel-release 輸出核心發行號
-v, --kernel-version 輸出核心版本
-m, --machine 輸出主機的硬體架構名稱
-p, --processor 輸出處理器型別或"unknown"
-i, --hardware-platform 輸出硬體平臺或"unknown"
-o, --operating-system 輸出作業系統名稱

18.	top工作管理員介面，顯示 CPU, Memory, PID資訊
![image21.png](/s1111/image21.png) 
19.	echo常用於回應環境變式參數
![image22.png](/s1111/image22.png)  
20.	zip, unzip壓縮與解壓縮程式，相關語法請參考 https://www.opencli.com/linux/linux-zip-unzip
21.	clear清除畫面
22.	mount 掛載裝置 http://linux.vbird.org/linux_basic/0230filesystem/0230filesystem-fc4.php#mount
mount 顯示所有掛載點的資訊
mount /dev1 /fs1掛載dev1至 /fs1
mount -a 會依照 /etc/fstab內容將所有裝置掛載
umount /fs1將 /fs1進行卸載
23.	ln建立連結指令
ln -s [target] [link name]
![image23.png](/s1111/image23.png)  

