---
title: crm_col
description: 
published: true
date: 2023-01-05T01:49:36.520Z
tags: 
editor: markdown
dateCreated: 2023-01-05T01:49:34.543Z
---

colocation col_sap_HA1_no_both -5000: grp_HA1_ERS10 grp_HA1_ASCS00 
location loc_sap_HA1_failover_to_ers rsc_sap_HA1_ASCS00 \
         rule 2000: runs_ers_HA1 eq 1
order ord_sap_HA1_first_start_ascs Optional: rsc_sap_HA1_ASCS00:start \ 
rsc_sap_HA1_ERS10:stop symmetrical=false