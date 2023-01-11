---
title: crm_ascs
description: 
published: true
date: 2023-01-05T01:48:52.343Z
tags: 
editor: markdown
dateCreated: 2023-01-05T01:48:17.736Z
---

primitive rsc_fs_HA1_ASCS00 Filesystem \
params device="/dev/sdb2" directory="/usr/sap/HA1/ASCS00" \
fstype=xfs \
op start timeout=60s interval=0 \ 
op stop timeout=60s interval=0 \
op monitor interval=20s timeout=40s
primitive rsc_ip_HA1_ASCS00 IPaddr2 \ 
params ip=192.168.21.215 \
op monitor interval=10s timeout=20s
primitive rsc_sap_HA1_ASCS00 SAPInstance \
operations $id=rsc_sap_HA1_ASCS00-operations \
op monitor interval=11 timeout=60 on_fail=restart \ 
params InstanceName=HA1_ASCS00_sapha1as \
START_PROFILE="/sapmnt/HA1/profile/HA1_ASCS00_sapha1as" \
AUTOMATIC_RECOVER=false \
meta resource-stickiness=5000 failure-timeout=60 \
migration-threshold=1 priority=10

group grp_HA1_ASCS00 \
rsc_ip_HA1_ASCS00 rsc_fs_HA1_ASCS00 rsc_sap_HA1_ASCS00 \
meta resource-stickiness=3000