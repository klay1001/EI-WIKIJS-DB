---
title: S1.2.1.6	Change archive log mode / switch
description: 
published: true
date: 2022-12-23T07:00:57.810Z
tags: 
editor: markdown
dateCreated: 2022-12-16T08:36:21.441Z
---

# Change archive log mode / switch
1.查詢目前Archive log mode
SQL> archive log list;

2.切換noarchive log mode
SQL> Shutdown
SQL> Startup mount
SQL> Alter database noarchivelog;
SQL> alter database open;

3.切換archive log mode
SQL> Shutdown
SQL> Startup mount
SQL> Alter database archivelog;
SQL> alter database open;

4.Log Switch
SQL> ALTER SYSTEM SWITCH LOGFILE;
