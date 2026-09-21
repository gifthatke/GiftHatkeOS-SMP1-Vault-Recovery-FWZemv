---
title: "Executive Dashboard PH1 Incident"
status: "certified-historical"
last_verified: "2026-08-25"
source: "retained-conversation-knowledge"
tags:
  - gifthatkeos
  - incident
  - dashboard
---

# Executive Dashboard PH1 Incident

## Symptom
- “Dashboard not loading”
- Try again disabled
- no useful logs initially

## Root cause retained
- legacy `crmDashboard()` duplicate async path;
- stale callbacks;
- null errors;
- hidden UI state issues.

## PH1 corrective work
- `Script.html` controller replacement;
- `executiveDashboardGetWorkspace(forceRefresh)` moved;
- `View_Dashboard` script removed;
- hidden CSS fix;
- retry/refresh bound.

## Outcome
User reported the error was gone and the Dashboard loaded correctly.
