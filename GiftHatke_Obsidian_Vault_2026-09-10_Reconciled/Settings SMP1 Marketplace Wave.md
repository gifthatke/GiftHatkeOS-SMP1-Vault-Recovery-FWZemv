# Settings SMP1 Marketplace Wave

Status: IMPLEMENTED / NOT CERTIFIED  
Date: 2026-09-11  
Frozen Apps Script authority: fd7c754fb1be380e6d3f9b01dd041b97b82f1d87

Implemented exact ERP8.6 behavior: configuration workspace; channel, SLA, source, payment, and fulfilment list/save operations; frozen validation; channel-reference checks; and order-configuration resolution. Transport is authenticated, mutations are CSRF protected, persistence reuses the admitted Marketplace family tables, and existing Settings view/manage authority is reused without adding a permission entry.

Remaining gates: Tax/Finance, Production, Inventory/Procurement, Notification/Communication, family UI forms, secure secret provider, migration execution, deployment, live acceptance, certification, and closure.
