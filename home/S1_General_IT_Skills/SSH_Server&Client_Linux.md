---
title: S1.1.1.6	SSH Server & Client
description: 
published: true
date: 2023-01-05T03:00:43.393Z
tags: 
editor: markdown
dateCreated: 2022-12-19T01:31:57.750Z
---

# SSH Server & Client
1.在 SUSE 任何版本中， openSSH預設都會被安裝，但沒有啟用服務。若要啟用只需到 Firewall設定中將 SSH納入設定即可。

2.範例
SSH server 架設
安裝 openssh 套件：不過一般 Linux 安裝時都已安裝。
`[root@kvm8 ~]# yum install -y openssh-server openssh-clients`
編輯 /etc/ssh/sshd_config 設定檔：限制或允許 ssh 用 root 帳號登入。
`[root@kvm8 ~]# vim /etc/ssh/sshd_config `
PermitRootLogin yes
編輯 /etc/ssh/sshd_config 設定檔：限制或允許 ssh 使用密碼登入，或只能用金鑰登入，yes 表示可以使用密碼登入。
`[root@kvm8 ~]# vim /etc/ssh/sshd_config 
PasswordAuthentication yes`
編輯 /etc/ssh/sshd_config 設定檔：修改 ssh 的 port，預設為 22。
`[root@kvm8 ~]# vim /etc/ssh/sshd_config `
`Port 22`
啟動 sshd 服務
`[root@kvm8 ~]# /etc/init.d/sshd restart
Shutting down sshd:                                      [FAILED]
Starting vsftpd for sshd:                                [  OK  ]`
設定開機啟動 sshd 服務
`[root@kvm8 ~]# chkconfig sshd on`

3.putty使用教學 https://blog.xuite.net/brown168168/blog/20571618-PUTTY+%E5%85%A5%E9%96%80+%E6%95%99%E5%AD%B8
