---
title: crm_ers
description: 
published: true
date: 2023-01-05T01:50:07.414Z
tags: 
editor: markdown
dateCreated: 2023-01-05T01:50:05.433Z
---

primitive rsc_fs_HA1_ERS10 Filesystem \
params device="/dev/sdb3" directory="/usr/sap/HA1/ERS10" fstype=xfs \ 
op start timeout=60s interval=0 \
op stop timeout=60s interval=0 \
op monitor interval=20s timeout=40s
primitive rsc_ip_HA1_ERS10 IPaddr2 \ 
params ip=192.168.21.216 \
op monitor interval=10s timeout=20s
primitive rsc_sap_HA1_ERS10 SAPInstance \
operations $id=rsc_sap_HA1_ERS10-operations \
op monitor interval=11 timeout=60 on_fail=restart \ 
params InstanceName=HA1_ERS10_sapha1er \
START_PROFILE="/sapmnt/HA1/profile/HA1_ERS10_sapha1er" \
AUTOMATIC_RECOVER=false IS_ERS=true \ 
meta priority=1000

group grp_HA1_ERS10 \
rsc_ip_HA1_ERS10 rsc_fs_HA1_ERS10 rsc_sap_HA1_ERS10