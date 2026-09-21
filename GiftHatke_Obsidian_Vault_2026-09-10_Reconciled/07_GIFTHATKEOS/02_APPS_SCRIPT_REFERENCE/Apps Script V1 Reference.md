---
title: "Apps Script GiftHatkeOS v1.0 Reference"
status: "certified-historical"
last_verified: "2026-08-25"
source: "retained-conversation-knowledge"
tags:
  - gifthatkeos
  - apps-script
  - reference
---

# Apps Script GiftHatkeOS v1.0 Reference

The Apps Script system is the authoritative working/behavioural reference for standalone parity.

## Technology
- Google Sheets database
- Apps Script server-side `.gs`
- HTML/CSS/JS app shell
- originally launched from bound spreadsheet menu: `🎁 GiftHatke OS → Launch ERP`

## Historical UX target
“HubSpot + Zoho Inventory + Shopify Admin.”

## Early structure retained
- `App.html`
- `Script.html`
- `View_Dashboard.html`
- `View_CRM.html`
- `Code.gs`
- `Router.gs`
- `Helpers.gs`

## Performance motivation for standalone
Apps Script became too slow for the desired enterprise workflow; standalone migration was chosen while preserving behaviour.
