# SMP1 Phase 3 — Authenticated Dashboard Observation

**Date:** 2026-09-13  
**Wave:** SMP1 Canon Recovery and Final Reconciliation  
**Phase:** 3 — fresh authenticated ERP acceptance  
**Evidence status:** PARTIAL FOUNDATION  
**Acceptance status:** DASHBOARD RENDERING OBSERVED / APPLICATION-ORIGIN ERROR CHECK OPEN  
**Finding linkage:** SMP1-GAP-001; blocking findings 001, 002, 005, 006, 009 and 011 remain open

## Source and provenance

- Operator-supplied file: `Screenshot 2026-09-13 at 15.05.48.png`
- Screenshot SHA-256: `604675353cfdb396df0b0218d8635b0fba33f80bbe2c897b2fe3e46abc28de24`
- Filename time label: `15:05:48`; timezone is not independently verified.
- Intended target: `https://erp.gifthatke.in/`
- The browser address bar is outside the supplied image, so the target URL and deployed commit are not independently proven by this screenshot.

## Directly observed

The image shows a signed-in-looking GiftHatkeOS Employee workspace. A principal
label and `Sign out` control are visible. The `Dashboard` workspace is selected.

Visible Dashboard content:

| Surface | Displayed value |
|---|---:|
| Total Orders | 1 |
| Open Orders | 1 |
| Overdue | 0 |
| Due Today | 0 |
| Revenue | ₹0 |
| Outstanding | ₹0 |
| Production Jobs | 0 |
| Connection badge | `Live data connected` |

The order summary and manufacturing pulse sections render below the command
center. This proves that the captured page rendered these Dashboard elements;
it does not prove complete business parity, every provider response, or a
server-side authenticated session independently.

## Console observation

The Console shows two red entries with the same visible message:

```text
Cross-Origin-Opener-Policy policy would block the window.postMessage call.
```

Both entries are linked to `client:395`. The screenshot does not show the
expanded stack, source hostname, source path, response headers, or whether the
messages occurred during Google sign-in, session restoration, or another
operation. They remain unresolved application-origin observations.

Read-only source evidence at deployed-source baseline
`ffde4c20c15f1630dfc822a073cb94d82b504acf` shows that the web client loads
`https://accounts.google.com/gsi/client` through `apps/web/src/google-identity.ts`.
Google's [Identity Services COOP guidance](https://developers.google.com/identity/gsi/web/guides/get-google-api-clientid#cross_origin_opener_policy)
documents that opener policy can interfere with sign-in popup communication.
This makes the sign-in client a plausible source, but the screenshot does not
establish attribution or impact.

## Next read-only observation

Retain this screenshot. In the open DevTools Console:

1. Expand one of the two red entries.
2. Hover the blue `client:395` source link.
3. Record only the hostname and path shown; omit query parameters and tokens.
4. Capture the expanded message and source link in a follow-up screenshot.

Do not paste credentials, cookies, tokens, secret values, or full query strings.
Do not submit forms, create or edit records, change Settings, or alter browser
headers. A source URL alone will not authorize a code or deployment change.

## Evidence boundary and disposition

This card records one live Dashboard observation. It does not certify the
thirteen employee workspaces, the 44 Canon domains, database and persistence
coverage, deployment topology, backup/restore, rollback, regression, or
permissions. Empty states, read-only restrictions, denied access and actual
application errors must remain distinct in later evidence.

No finding is resolved, removed, merged, downgraded or silently accepted. User
Management remains read-only and Settings remains certified/live/closed at
100%. No application code, dependency, migration, route, permission, API, UI,
deployment, database, secret or business-behavior change is authorized by this
observation.

**Progress:** Canon Recovery 100% · User Management 100% · Settings 100% ·
Overall SMP1 <100% / handover blocked.

## Shape-only JSON inspection — operator probe

**Probe result:** `READ_ONLY_SHAPE_PROBE_COMPLETE 14`  
**Request method:** GET only  
**Response parsing:** JSON parsed for all 14 responses  
**Logged data:** top-level keys, array counts, and safe booleans only  
**Settings:** deliberately excluded

Every row returned HTTP 200, `ok=true`, and `json=true`. The safe flags were:

| Path | Safe flag |
|---|---|
| `/auth/session` | `authenticated: true` |
| `/reports/workspace` | `readOnly: true` |
| `/users/workspace` | `currentSessionAuthenticated: true` |

Observed response shapes and counts:

| Path | Top-level keys / array counts |
|---|---|
| `/dashboard/workspace` | `generatedAt, inventory, orders, production, workQueue`; `workQueue:0` |
| `/orders/workspace` | `dashboard, generatedAt, lookups, orders`; `orders:1` |
| `/production/workspace` | `jobs, loadedAt, machines, operators, priorities, qcChecklist, stages, stats`; `stages:9, jobs:0, machines:1, operators:0, priorities:4, qcChecklist:7` |
| `/inventory/workspace` | `goodsReceipts, materials, movements, purchaseOrders, purchaseRequisitions`; `materials:1, movements:2, purchaseRequisitions:1, purchaseOrders:1, goodsReceipts:1` |
| `/crm/workspace` | `config, dashboard, leads`; `leads:0` |
| `/customers/workspace` | `customers`; `customers:1` |
| `/personalization/workspace` | `cloudinary, intakes, readiness, sources, stats, statuses, templates`; `intakes:1, templates:0, statuses:7, sources:3, readiness:3` |
| `/shipping/workspace` | `couriers, generatedAt, metrics, readyOrders, shipments, statuses, zones`; `shipments:0, couriers:0, readyOrders:0, statuses:17, zones:6` |
| `/finance/workspace` | `configuration, controls, core, generatedAt, operations, reporting`; no top-level arrays reported |
| `/reports/workspace` | `alerts, meta, modules, navigation, summary`; `modules:8, navigation:8, alerts:1` |
| `/reports/operational` | `generatedAt, sources, workspace`; no top-level arrays reported |
| `/reports/financial` | `executiveFinanceIntelligence, generatedAt, reporting, statements`; no top-level arrays reported |
| `/users/workspace` | `currentSession, meta, permissions, rolePermissions, roles, summary, users`; `users:1, roles:1, permissions:109, rolePermissions:14` |

This confirms that the browser received parseable JSON with an authenticated
session flag and the expected high-level workspace shapes for the probed
surfaces. The response values themselves were not logged. Counts and keys do
not prove business correctness, persistence parity, permission semantics,
deployment topology, or complete Canon requirement coverage. The Settings
workspace remains unqueried by design.

Disposition: **shape-only authenticated read-only evidence PASS (14/14)**;
**full Phase 3 acceptance OPEN**. The COOP source attribution is confirmed, but
its runtime impact remains unresolved. No finding is resolved or reclassified.

**Progress:** Canon Recovery 100% · User Management 100% · Settings 100% ·
Overall SMP1 <100% / handover blocked.

## Source attribution update — second screenshot

**Date:** 2026-09-13  
**Evidence status:** SOURCE ATTRIBUTION CONFIRMED / RUNTIME IMPACT OPEN

- Operator-supplied file: `Screenshot 2026-09-13 at 15.15.30.png`
- Screenshot SHA-256: `afece719705b7e8ce175ad988f6eb4282a50cb3a67ca64c455c70c3ecdcaf242`
- The Sources panel visibly contains the `erp.gifthatke.in` page and an
  `accounts.google.com` tree with `gsi/client` selected.
- The selected resource is positioned at line 395, column 191, matching the
  `client:395` source marker shown with the earlier Console messages.
- The Dashboard remains rendered in the same signed-in-looking Employee
  workspace while this source is selected.

The operator also supplied `Pasted text(20260913-131541).txt`, an extracted
454,537-byte, 10,345-line copy of the selected Google Identity Services client.
Its SHA-256 is
`9406ae6b6d032d056b2ce8862a6110c1a01c07a09bc2e86b66d7881d3da63af5`. The
extracted text contains Google Identity Services `postMessage` calls and the
Google endpoints used by that client, including `/gsi/log`, `/gsi/button`,
`/gsi/status`, and OAuth endpoints. It also contains a Google warning about
cross-origin permission policy at source line 9148. This corroborates the
attribution of the Console message to the third-party sign-in client.

This update raises the source-attribution evidence from **plausible** to
**directly observed**. It does not prove that the COOP messages block sign-in,
invalidate the authenticated session, break any workspace, or represent an ERP
application defect. The Dashboard continuing to render is evidence against a
current total sign-in failure, but it is not a complete acceptance result.

No response headers were changed, no browser policy was bypassed, and no
application source or deployment was modified. GAP-001 remains open because
fresh acceptance across the required surfaces and requirement-level Canon
parity remain incomplete. All blocking findings and closed-module decisions
remain unchanged.

**Next read-only check:** in the Console, switch to the page context and run
the status-only GET probe supplied in the next instruction. It checks the
authenticated session and the read-only workspace endpoints without printing
customer, employee, order or settings values. Do not run any POST, PUT, PATCH
or DELETE request.

## Authenticated endpoint reachability — operator probe

**Probe result:** `READ_ONLY_GET_PROBE_COMPLETE 14`  
**Request method:** GET only  
**Credentials:** `include`  
**Response bodies:** discarded with `response.body?.cancel()`  
**Settings:** deliberately excluded

The operator's Console table reports `status=200`, `ok=true`, and
`application/json; charset=utf-8` for all fourteen requested paths:

| Path | Status | Content type |
|---|---:|---|
| `/auth/session` | 200 | `application/json; charset=utf-8` |
| `/dashboard/workspace` | 200 | `application/json; charset=utf-8` |
| `/orders/workspace` | 200 | `application/json; charset=utf-8` |
| `/production/workspace` | 200 | `application/json; charset=utf-8` |
| `/inventory/workspace` | 200 | `application/json; charset=utf-8` |
| `/crm/workspace` | 200 | `application/json; charset=utf-8` |
| `/customers/workspace` | 200 | `application/json; charset=utf-8` |
| `/personalization/workspace` | 200 | `application/json; charset=utf-8` |
| `/shipping/workspace` | 200 | `application/json; charset=utf-8` |
| `/finance/workspace` | 200 | `application/json; charset=utf-8` |
| `/reports/workspace` | 200 | `application/json; charset=utf-8` |
| `/reports/operational` | 200 | `application/json; charset=utf-8` |
| `/reports/financial` | 200 | `application/json; charset=utf-8` |
| `/users/workspace` | 200 | `application/json; charset=utf-8` |

This is strong evidence that the current authenticated browser can reach the
session endpoint and these employee workspace GET routes. It does not inspect
the JSON shape or values, prove business behavior, prove permission semantics,
certify Settings, establish database or deployment parity, or clear the COOP
messages. It is therefore recorded as **authenticated read-only endpoint
reachability: PASS (14/14)** and **full Phase 3 acceptance: OPEN**.

The Console output contains no network error for these requests. It does not
constitute a complete Console-clean capture. No finding is resolved or
reclassified; GAP-001 and the other blocking findings remain open.

**Progress:** Canon Recovery 100% · User Management 100% · Settings 100% ·
Overall SMP1 <100% / handover blocked.
