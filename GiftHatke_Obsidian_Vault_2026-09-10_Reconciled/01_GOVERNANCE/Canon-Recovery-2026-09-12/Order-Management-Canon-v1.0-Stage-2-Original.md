# GiftHatkeOS Enterprise Canon  
## Order Management Canon v1.0 — Stage 2  
### Canonical Data Model and PostgreSQL-Ready Schema

This stage defines the permanent data structure for the Order Management domain.

It is authoritative for:

- GiftHatkeOS Classic reference mapping;
- SMP1 standalone migration;
- PostgreSQL implementation;
- API contracts;
- reporting;
- audit;
- ERP9 and ERP10 expansion;
- future mobile, marketplace, AI, and external integrations.

The schema below preserves the certified business architecture while separating transactional data into stable, auditable records.

---

# 1. Data-Model Principles

The Order data model must follow these permanent rules:

1. Business IDs and internal database IDs are separate.
2. Historical transactional data must not change when master data changes.
3. Financial values use fixed-precision decimal types.
4. Statuses use controlled values, not free text.
5. Records are archived rather than physically deleted.
6. Every significant write records actor and timestamp information.
7. Cross-module relationships use immutable identifiers.
8. Large assets remain outside the relational database.
9. Concurrent updates use revision-based optimistic locking.
10. Reporting fields must be queryable without parsing notes or JSON blobs.

---

# 2. Aggregate Relationship Map

```text
customers
    │
    └── orders
          ├── order_items
          │     └── order_item_assets
          │
          ├── order_addresses
          ├── order_charges
          ├── order_discounts
          ├── order_payment_allocations
          ├── order_notes
          ├── order_activities
          ├── order_attachments
          ├── order_holds
          ├── order_cancellations
          ├── order_status_history
          ├── order_department_history
          ├── order_revision_history
          └── order_audit_events
```

Cross-domain references include:

```text
crm_leads
products
product_variants
personalization_cases
production_jobs
inventory_movements
shipments
invoices
payments
refunds
users
teams
```

The Orders domain stores references to these records but does not absorb their full data ownership.

---

# 3. PostgreSQL Conventions

Recommended conventions:

```text
Internal ID: UUID
Business ID: VARCHAR
Timestamp: TIMESTAMPTZ
Date only: DATE
Money: NUMERIC(18,2)
Quantity: NUMERIC(18,4)
Percentage: NUMERIC(7,4)
Boolean: BOOLEAN
Structured optional metadata: JSONB
Status fields: VARCHAR with CHECK constraint or reference table
```

All timestamps should be stored in UTC.

Business-facing date and time presentation must respect the configured company time zone.

For Gift Hatke operations, the initial business time zone remains:

```text
Asia/Kolkata
```

---

# 4. `orders`

This is the root record of the Order aggregate.

## Canonical Fields

| Field | Type | Required | Purpose |
|---|---:|:---:|---|
| `id` | UUID | Yes | Internal immutable database ID |
| `order_number` | VARCHAR(32) | Yes | Human-readable business ID |
| `order_type` | VARCHAR(40) | Yes | Retail, Corporate, Marketplace, etc. |
| `source_type` | VARCHAR(40) | Yes | Manual, Lead Conversion, Import, API, etc. |
| `sales_channel` | VARCHAR(40) | Yes | Website, WhatsApp, Amazon, etc. |
| `customer_id` | UUID | Yes | Current customer master reference |
| `lead_id` | UUID | No | CRM lead reference |
| `parent_order_id` | UUID | No | Original order for replacement or derived order |
| `external_order_id` | VARCHAR(120) | No | Marketplace or external system ID |
| `external_source` | VARCHAR(80) | No | Amazon, Shopify, Flipkart, etc. |
| `quotation_id` | UUID | No | Converted quotation reference |
| `order_date` | DATE | Yes | Commercial order date |
| `required_date` | DATE | No | Customer-required completion date |
| `promised_dispatch_date` | DATE | No | Dispatch commitment |
| `expected_dispatch_date` | DATE | No | Current operational estimate |
| `actual_dispatch_at` | TIMESTAMPTZ | No | Actual dispatch timestamp |
| `promised_delivery_date` | DATE | No | Customer delivery commitment |
| `expected_delivery_date` | DATE | No | Current estimated delivery |
| `actual_delivery_at` | TIMESTAMPTZ | No | Actual delivery timestamp |
| `commercial_status` | VARCHAR(40) | Yes | Draft, Confirmed, Delivered, etc. |
| `current_department` | VARCHAR(40) | Yes | Current operational ownership |
| `payment_status` | VARCHAR(40) | Yes | Derived payment position |
| `personalization_status` | VARCHAR(40) | Yes | Current personalization readiness |
| `fulfilment_status` | VARCHAR(40) | Yes | Aggregate fulfilment state |
| `priority` | VARCHAR(20) | Yes | Low, Normal, High, Urgent |
| `currency_code` | CHAR(3) | Yes | Initially INR |
| `subtotal_amount` | NUMERIC(18,2) | Yes | Sum before order-level charges and tax |
| `discount_amount` | NUMERIC(18,2) | Yes | Total recognized discount |
| `taxable_amount` | NUMERIC(18,2) | Yes | Taxable base |
| `tax_amount` | NUMERIC(18,2) | Yes | Total GST or other tax |
| `shipping_amount` | NUMERIC(18,2) | Yes | Customer-facing shipping charge |
| `other_charge_amount` | NUMERIC(18,2) | Yes | Other recognized charges |
| `grand_total` | NUMERIC(18,2) | Yes | Final commercial value |
| `paid_amount` | NUMERIC(18,2) | Yes | Valid allocated payments |
| `credited_amount` | NUMERIC(18,2) | Yes | Credit notes or approved credits |
| `refunded_amount` | NUMERIC(18,2) | Yes | Completed refunds |
| `outstanding_amount` | NUMERIC(18,2) | Yes | Current receivable |
| `assigned_user_id` | UUID | No | Primary owner |
| `assigned_team_id` | UUID | No | Primary responsible team |
| `occasion` | VARCHAR(120) | No | Birthday, anniversary, wedding, etc. |
| `campaign` | VARCHAR(120) | No | Campaign attribution |
| `source_reference` | VARCHAR(255) | No | Referral or acquisition reference |
| `marketplace_fee_amount` | NUMERIC(18,2) | Yes | Recognized marketplace fee |
| `hold_status` | BOOLEAN | Yes | Whether an active hold exists |
| `active_hold_id` | UUID | No | Current hold reference |
| `cancellation_status` | VARCHAR(40) | Yes | Not Cancelled, Partial, Cancelled |
| `customer_notes` | TEXT | No | Customer-facing or provided instructions |
| `internal_notes` | TEXT | No | Internal operational summary |
| `operational_tags` | TEXT[] | No | Searchable structured tags |
| `revision` | INTEGER | Yes | Optimistic-lock revision |
| `record_status` | VARCHAR(20) | Yes | Active or Archived |
| `created_at` | TIMESTAMPTZ | Yes | Creation timestamp |
| `created_by` | UUID | Yes | Creating actor |
| `updated_at` | TIMESTAMPTZ | Yes | Latest modification timestamp |
| `updated_by` | UUID | Yes | Last modifying actor |
| `archived_at` | TIMESTAMPTZ | No | Archive timestamp |
| `archived_by` | UUID | No | Archiving actor |

## Required Defaults

```text
commercial_status       = Draft
current_department      = Sales
payment_status          = Not Requested
personalization_status  = Details Pending
fulfilment_status       = Not Started
priority                = Normal
currency_code           = INR
discount_amount         = 0
tax_amount              = 0
shipping_amount         = 0
other_charge_amount     = 0
paid_amount             = 0
credited_amount         = 0
refunded_amount         = 0
marketplace_fee_amount  = 0
hold_status             = false
cancellation_status     = Not Cancelled
revision                = 1
record_status           = Active
```

## Constraints

```text
order_number must be unique
grand_total >= 0
paid_amount >= 0
credited_amount >= 0
refunded_amount >= 0
outstanding_amount >= 0 unless overpayment is explicitly supported separately
revision >= 1
customer_id must exist
parent_order_id cannot equal id
```

---

# 5. Order Number Sequence

Order numbers should not be generated using row counts.

Recommended dedicated sequence storage:

## `business_sequences`

| Field | Type | Purpose |
|---|---:|---|
| `sequence_key` | VARCHAR(80) | Example: `ORDER-2026` |
| `prefix` | VARCHAR(30) | Example: `GH-ORD-2026-` |
| `current_value` | BIGINT | Last allocated number |
| `padding_length` | INTEGER | Example: 6 |
| `updated_at` | TIMESTAMPTZ | Last allocation |
| `revision` | INTEGER | Concurrency control |

Allocation must execute inside a database transaction with row locking.

Example:

```text
GH-ORD-2026-000184
```

---

# 6. `order_items`

Each row represents one ordered product or service line.

| Field | Type | Required | Purpose |
|---|---:|:---:|---|
| `id` | UUID | Yes | Internal item ID |
| `order_id` | UUID | Yes | Parent order |
| `line_number` | INTEGER | Yes | Stable display order |
| `item_number` | VARCHAR(40) | Yes | Human-readable line reference |
| `product_id` | UUID | No | Current product reference |
| `product_variant_id` | UUID | No | Current variant reference |
| `sku_snapshot` | VARCHAR(100) | No | SKU at sale time |
| `product_name_snapshot` | VARCHAR(255) | Yes | Product name at sale time |
| `variant_snapshot` | VARCHAR(255) | No | Variant details at sale time |
| `description_snapshot` | TEXT | No | Commercial description |
| `hsn_code_snapshot` | VARCHAR(20) | No | Tax classification snapshot |
| `quantity` | NUMERIC(18,4) | Yes | Ordered quantity |
| `fulfilled_quantity` | NUMERIC(18,4) | Yes | Completed quantity |
| `cancelled_quantity` | NUMERIC(18,4) | Yes | Cancelled quantity |
| `unit_of_measure` | VARCHAR(30) | Yes | Piece, set, box, etc. |
| `unit_price` | NUMERIC(18,2) | Yes | Price per unit |
| `gross_amount` | NUMERIC(18,2) | Yes | Quantity × unit price |
| `discount_amount` | NUMERIC(18,2) | Yes | Line discount |
| `taxable_amount` | NUMERIC(18,2) | Yes | Line tax base |
| `tax_rate` | NUMERIC(7,4) | Yes | Applicable tax percentage |
| `cgst_amount` | NUMERIC(18,2) | Yes | Central GST |
| `sgst_amount` | NUMERIC(18,2) | Yes | State GST |
| `igst_amount` | NUMERIC(18,2) | Yes | Integrated GST |
| `cess_amount` | NUMERIC(18,2) | Yes | Cess if applicable |
| `tax_amount` | NUMERIC(18,2) | Yes | Total line tax |
| `line_total` | NUMERIC(18,2) | Yes | Final line value |
| `material_snapshot` | TEXT | No | Primary material description |
| `personalization_required` | BOOLEAN | Yes | Whether customization is required |
| `personalization_instructions` | TEXT | No | Item-specific customer brief |
| `production_recipe_id` | UUID | No | Recipe reference |
| `production_recipe_version` | INTEGER | No | Recipe version frozen at release |
| `fulfilment_status` | VARCHAR(40) | Yes | Item fulfilment state |
| `personalization_status` | VARCHAR(40) | Yes | Item personalization state |
| `production_status` | VARCHAR(40) | Yes | Item production readiness |
| `cancellation_status` | VARCHAR(40) | Yes | Item cancellation state |
| `required_date` | DATE | No | Item-specific requirement |
| `record_status` | VARCHAR(20) | Yes | Active or Archived |
| `created_at` | TIMESTAMPTZ | Yes | Creation timestamp |
| `created_by` | UUID | Yes | Creator |
| `updated_at` | TIMESTAMPTZ | Yes | Latest update |
| `updated_by` | UUID | Yes | Last updater |
| `revision` | INTEGER | Yes | Item-level optimistic locking |

## Item Invariants

```text
quantity > 0
fulfilled_quantity >= 0
cancelled_quantity >= 0
fulfilled_quantity + cancelled_quantity <= quantity
unit_price >= 0
discount_amount >= 0
line_total >= 0
line_number unique within one order
item_number unique within one order
```

---

# 7. `order_item_assets`

This table links ordered items to assets without storing binary files.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Link record |
| `order_id` | UUID | Order reference |
| `order_item_id` | UUID | Item reference |
| `asset_id` | UUID | Asset-domain reference |
| `asset_role` | VARCHAR(50) | Customer Photo, Artwork, Production File, etc. |
| `asset_version` | INTEGER | Linked version |
| `approval_status` | VARCHAR(40) | Current approval position |
| `locked_for_production` | BOOLEAN | Production lock |
| `linked_at` | TIMESTAMPTZ | Link timestamp |
| `linked_by` | UUID | Actor |
| `record_status` | VARCHAR(20) | Active or Archived |

An asset must not be silently replaced after being locked for production.

A later file must create a new asset version or link.

---

# 8. `order_addresses`

Orders must store address snapshots.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Address snapshot ID |
| `order_id` | UUID | Parent order |
| `address_type` | VARCHAR(30) | Billing or Shipping |
| `contact_name` | VARCHAR(150) | Recipient |
| `company_name` | VARCHAR(200) | Company if applicable |
| `phone` | VARCHAR(30) | Contact number |
| `email` | VARCHAR(255) | Email |
| `address_line_1` | VARCHAR(255) | Primary address |
| `address_line_2` | VARCHAR(255) | Additional address |
| `landmark` | VARCHAR(255) | Landmark |
| `city` | VARCHAR(120) | City |
| `district` | VARCHAR(120) | District |
| `state_code` | VARCHAR(10) | GST/state code |
| `state_name` | VARCHAR(120) | State |
| `postal_code` | VARCHAR(20) | PIN/postal code |
| `country_code` | CHAR(2) | ISO country code |
| `gstin` | VARCHAR(20) | Transactional GSTIN |
| `place_of_supply_code` | VARCHAR(10) | Tax place of supply |
| `is_primary` | BOOLEAN | Primary snapshot of this type |
| `created_at` | TIMESTAMPTZ | Snapshot creation |
| `created_by` | UUID | Actor |

## Constraint

Only one primary billing and one primary shipping address may exist per order.

---

# 9. `order_charges`

This table stores charges separately from product lines.

Examples:

- shipping;
- gift wrapping;
- urgent production;
- installation;
- insurance;
- COD;
- marketplace-specific surcharge.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Charge ID |
| `order_id` | UUID | Parent order |
| `charge_type` | VARCHAR(50) | Shipping, Packaging, etc. |
| `description` | VARCHAR(255) | Charge explanation |
| `amount` | NUMERIC(18,2) | Pre-tax or final amount |
| `tax_rate` | NUMERIC(7,4) | Tax percentage |
| `tax_amount` | NUMERIC(18,2) | Tax |
| `total_amount` | NUMERIC(18,2) | Total charge |
| `included_in_grand_total` | BOOLEAN | Whether financially included |
| `source` | VARCHAR(40) | Manual, Rule, Courier, Marketplace |
| `created_at` | TIMESTAMPTZ | Creation |
| `created_by` | UUID | Actor |
| `record_status` | VARCHAR(20) | Active or Reversed |

Charges must be reversed, not physically deleted, after financial posting.

---

# 10. `order_discounts`

Discounts must remain explainable.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Discount ID |
| `order_id` | UUID | Parent order |
| `order_item_id` | UUID | Optional line-level reference |
| `discount_type` | VARCHAR(40) | Percentage, Fixed, Coupon, Manual |
| `discount_code` | VARCHAR(100) | Coupon or scheme |
| `description` | VARCHAR(255) | Reason |
| `rate` | NUMERIC(7,4) | Percentage where applicable |
| `amount` | NUMERIC(18,2) | Applied value |
| `authorized_by` | UUID | Approver |
| `authorization_reason` | TEXT | Required for controlled discounts |
| `created_at` | TIMESTAMPTZ | Creation |
| `created_by` | UUID | Actor |
| `record_status` | VARCHAR(20) | Active or Reversed |

Discount limits should eventually come from policy configuration.

---

# 11. `order_payment_allocations`

The Payment domain owns payment transactions.

The Order domain owns how valid payments are allocated against an order.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Allocation ID |
| `order_id` | UUID | Order |
| `payment_id` | UUID | Payment-domain reference |
| `allocation_type` | VARCHAR(30) | Payment, Credit, Adjustment |
| `allocated_amount` | NUMERIC(18,2) | Applied amount |
| `allocation_status` | VARCHAR(30) | Pending, Confirmed, Reversed |
| `allocated_at` | TIMESTAMPTZ | Allocation time |
| `allocated_by` | UUID | Actor |
| `reversed_at` | TIMESTAMPTZ | Reversal time |
| `reversed_by` | UUID | Reversing actor |
| `reversal_reason` | TEXT | Reversal reason |

Confirmed, non-reversed allocations determine `paid_amount`.

---

# 12. `order_notes`

Notes are permanent operational records.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Note ID |
| `order_id` | UUID | Parent order |
| `note_type` | VARCHAR(40) | Internal, Customer, Production, Finance, etc. |
| `visibility` | VARCHAR(30) | Internal, Team Restricted, Customer Visible |
| `note_text` | TEXT | Content |
| `is_pinned` | BOOLEAN | Highlighted note |
| `created_at` | TIMESTAMPTZ | Creation |
| `created_by` | UUID | Author |
| `updated_at` | TIMESTAMPTZ | Update |
| `updated_by` | UUID | Editor |
| `record_status` | VARCHAR(20) | Active or Archived |

Editing a note must preserve revision history or create an audit event.

---

# 13. `order_activities`

Activities represent business interactions and operational actions.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Activity ID |
| `order_id` | UUID | Parent order |
| `activity_type` | VARCHAR(50) | Call, WhatsApp, Email, Status Update, etc. |
| `activity_outcome` | VARCHAR(120) | Outcome |
| `details` | TEXT | Description |
| `occurred_at` | TIMESTAMPTZ | Business occurrence time |
| `actor_id` | UUID | Responsible user |
| `source` | VARCHAR(40) | Manual, API, Automation, System |
| `external_reference` | VARCHAR(255) | Provider reference |
| `correlation_id` | UUID | Cross-system trace |
| `created_at` | TIMESTAMPTZ | System record time |

Activities may be user-created or system-generated.

---

# 14. `order_attachments`

This stores document links and metadata.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Attachment record |
| `order_id` | UUID | Order |
| `order_item_id` | UUID | Optional line |
| `file_id` | UUID | File-storage reference |
| `file_name` | VARCHAR(255) | Display name |
| `file_category` | VARCHAR(50) | PO, Invoice, Artwork, Receipt, etc. |
| `mime_type` | VARCHAR(120) | File type |
| `storage_provider` | VARCHAR(50) | Cloud Storage, Drive, Cloudinary |
| `storage_key` | VARCHAR(500) | Provider path/key |
| `secure_url` | TEXT | Controlled URL where appropriate |
| `checksum_sha256` | CHAR(64) | Integrity checksum |
| `version_number` | INTEGER | File version |
| `description` | TEXT | Context |
| `uploaded_at` | TIMESTAMPTZ | Upload time |
| `uploaded_by` | UUID | Actor |
| `record_status` | VARCHAR(20) | Active, Superseded, Archived |

Sensitive assets should normally use expiring signed URLs rather than permanent public URLs.

---

# 15. `order_holds`

Each hold is a distinct lifecycle record.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Hold ID |
| `order_id` | UUID | Parent order |
| `hold_category` | VARCHAR(50) | Payment, Material, Approval, etc. |
| `hold_reason` | TEXT | Full reason |
| `started_at` | TIMESTAMPTZ | Hold start |
| `started_by` | UUID | Actor |
| `expected_resolution_at` | TIMESTAMPTZ | Expected resolution |
| `customer_notification_status` | VARCHAR(30) | Not Required, Pending, Sent |
| `resolution_notes` | TEXT | Resolution |
| `released_at` | TIMESTAMPTZ | Hold release |
| `released_by` | UUID | Releasing actor |
| `status` | VARCHAR(20) | Active or Released |
| `revision` | INTEGER | Concurrency |

## Constraint

An order may have only one active aggregate-level hold unless a future policy explicitly allows concurrent holds.

Historical released holds remain queryable.

---

# 16. `order_cancellations`

Cancellation must be represented as an explicit transaction.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Cancellation ID |
| `order_id` | UUID | Parent order |
| `order_item_id` | UUID | Optional partial cancellation line |
| `cancelled_quantity` | NUMERIC(18,4) | Quantity cancelled |
| `cancellation_type` | VARCHAR(30) | Full, Partial, Replacement |
| `reason_code` | VARCHAR(50) | Controlled reason |
| `reason_detail` | TEXT | Explanation |
| `requested_by_type` | VARCHAR(30) | Customer, Internal, Marketplace |
| `requested_at` | TIMESTAMPTZ | Request time |
| `approved_at` | TIMESTAMPTZ | Approval |
| `approved_by` | UUID | Approver |
| `production_impact` | TEXT | Operational effect |
| `inventory_impact` | TEXT | Material effect |
| `shipping_impact` | TEXT | Shipment effect |
| `financial_impact` | TEXT | Financial effect |
| `refund_required` | BOOLEAN | Refund requirement |
| `refund_id` | UUID | Refund-domain reference |
| `status` | VARCHAR(30) | Requested, Approved, Rejected, Completed |
| `completed_at` | TIMESTAMPTZ | Completion |
| `created_by` | UUID | Creator |

Cancellation approval rules must be permission-controlled.

---

# 17. `order_status_history`

This provides a query-efficient commercial lifecycle history.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | History ID |
| `order_id` | UUID | Parent order |
| `from_status` | VARCHAR(40) | Previous commercial status |
| `to_status` | VARCHAR(40) | New commercial status |
| `reason_code` | VARCHAR(50) | Controlled reason |
| `reason_detail` | TEXT | Explanation |
| `changed_at` | TIMESTAMPTZ | Transition time |
| `changed_by` | UUID | Actor |
| `source` | VARCHAR(40) | UI, API, Automation, Import |
| `correlation_id` | UUID | Trace ID |

The latest status in this table must match `orders.commercial_status`.

---

# 18. `order_department_history`

Commercial status and operational ownership remain independent.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | History record |
| `order_id` | UUID | Parent order |
| `from_department` | VARCHAR(40) | Previous department |
| `to_department` | VARCHAR(40) | New department |
| `assigned_user_id` | UUID | New owner |
| `assigned_team_id` | UUID | New team |
| `reason` | TEXT | Transfer reason |
| `transferred_at` | TIMESTAMPTZ | Transfer time |
| `transferred_by` | UUID | Actor |
| `correlation_id` | UUID | Trace |

This supports workload analysis and process bottleneck reporting.

---

# 19. `order_revision_history`

Revision history records meaningful snapshots or patches.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Revision record |
| `order_id` | UUID | Parent order |
| `revision_number` | INTEGER | Resulting revision |
| `change_type` | VARCHAR(40) | Create, Update, Correction |
| `changed_fields` | JSONB | Field-level before and after values |
| `change_reason` | TEXT | Required for controlled changes |
| `changed_at` | TIMESTAMPTZ | Change time |
| `changed_by` | UUID | Actor |
| `correlation_id` | UUID | Request trace |

The full order does not need duplication for every trivial change, but financially significant revisions may require a complete immutable snapshot.

---

# 20. `order_audit_events`

This is the permanent audit ledger for the domain.

| Field | Type | Purpose |
|---|---:|---|
| `id` | UUID | Audit event ID |
| `order_id` | UUID | Order |
| `order_item_id` | UUID | Optional line reference |
| `event_type` | VARCHAR(80) | Canonical event |
| `event_version` | INTEGER | Event schema version |
| `event_at` | TIMESTAMPTZ | Event occurrence |
| `actor_id` | UUID | User or service actor |
| `actor_type` | VARCHAR(30) | User, System, Integration |
| `source` | VARCHAR(40) | Web, Mobile, API, Import, Worker |
| `previous_values` | JSONB | Before-state subset |
| `new_values` | JSONB | After-state subset |
| `reason` | TEXT | Business reason |
| `request_id` | UUID | Individual request trace |
| `correlation_id` | UUID | End-to-end workflow trace |
| `ip_address` | INET | Security trace where allowed |
| `user_agent` | TEXT | Client trace where allowed |
| `metadata` | JSONB | Structured additional context |
| `integrity_hash` | CHAR(64) | Optional event integrity hash |

Audit events are append-only.

They must not be updated or deleted through normal application operations.

---

# 21. Recommended Reference Tables

Controlled values may initially be enforced by application constants and database checks.

The durable standalone design should eventually support configurable reference tables:

```text
order_types
sales_channels
order_source_types
order_statuses
order_status_transitions
order_departments
payment_statuses
personalization_statuses
fulfilment_statuses
order_priorities
hold_categories
cancellation_reason_codes
activity_types
attachment_categories
discount_types
charge_types
```

Each configurable value should support:

```text
code
display_name
description
active
display_order
effective_from
effective_to
metadata
```

ERP9 can extend these into full administrator-managed master data.

---

# 22. Unique Constraints

Recommended unique constraints:

```text
orders.order_number
orders(external_source, external_order_id) where external_order_id is not null
order_items(order_id, line_number)
order_items(order_id, item_number)
order_addresses(order_id, address_type) where is_primary = true
order_item_assets(order_item_id, asset_id, asset_version)
order_payment_allocations(order_id, payment_id, allocation_type)
```

Marketplace uniqueness must account for environments or seller accounts if the same external ID could exist across accounts.

---

# 23. Recommended Indexes

## `orders`

```text
customer_id
lead_id
parent_order_id
commercial_status
current_department
payment_status
personalization_status
sales_channel
order_date
required_date
promised_dispatch_date
expected_dispatch_date
created_at
updated_at
record_status
assigned_user_id
assigned_team_id
external_order_id
```

Composite indexes:

```text
(record_status, commercial_status, order_date desc)
(current_department, commercial_status, required_date)
(customer_id, order_date desc)
(sales_channel, order_date desc)
(payment_status, outstanding_amount)
(hold_status, expected_dispatch_date)
```

## Child tables

Each child table must index `order_id`.

Frequently queried child tables should also index:

```text
status
created_at
event_at
occurred_at
order_item_id
correlation_id
```

---

# 24. Foreign-Key Behaviour

Recommended relationship behaviour:

| Parent | Child | Delete Behaviour |
|---|---|---|
| `orders` | `order_items` | Restrict |
| `orders` | `order_addresses` | Restrict |
| `orders` | `order_notes` | Restrict |
| `orders` | `order_audit_events` | Restrict |
| `orders` | `order_status_history` | Restrict |
| `customers` | `orders` | Restrict |
| `crm_leads` | `orders` | Set null only under exceptional migration rules |
| `products` | `order_items` | Set null while preserving snapshots |
| `users` | audit actor fields | Restrict or preserve deactivated user |

Hard deletion of an order should not exist in ordinary business operations.

---

# 25. Transaction Boundaries

The following operations must run inside one database transaction.

## Create Order

```text
Allocate order number
Create order
Create address snapshots
Create order items
Create charges and discounts
Calculate totals
Create status history
Create audit event
Commit
```

## Update Order

```text
Lock or revision-check order
Validate change
Update aggregate values
Increment revision
Write revision history
Write audit event
Commit
```

## Confirm Order

```text
Validate required fields
Validate customer
Validate items and totals
Validate payment/policy conditions
Update commercial status
Create status history
Create domain event
Commit
```

## Cancel Order

```text
Validate cancellation eligibility
Record cancellation
Update affected items
Update aggregate status
Create financial/recovery requirements
Write history and audit
Commit
```

No partially completed aggregate update should be visible.

---

# 26. Derived Values

The application should derive, not manually maintain, these values wherever possible:

```text
subtotal_amount
discount_amount
taxable_amount
tax_amount
grand_total
paid_amount
credited_amount
refunded_amount
outstanding_amount
payment_status
aggregate fulfilment status
aggregate personalization status
hold_status
cancellation status
```

For performance, derived values may be persisted on `orders`, but they must be recalculated only through authoritative domain services.

Direct database clients must not alter them independently.

---

# 27. Data Snapshot Rules

The Order record and its items must retain sale-time snapshots of:

```text
customer name
customer contact
billing address
shipping address
product name
SKU
variant
description
HSN
tax rate
unit price
material description
commercial terms
```

Later edits to Customer, Product, Tax, or Address master data must not rewrite historical transactional records.

Controlled correction workflows may update snapshots only with:

- elevated permission;
- reason;
- revision increment;
- audit event.

---

# 28. Apps Script-to-Standalone Mapping

The current Apps Script implementation uses broad order objects and sheet rows.

The standalone migration should map conceptually as follows:

| Current Apps Script Concept | Standalone Destination |
|---|---|
| Orders sheet row | `orders` |
| Serialized items or order-item sheet | `order_items` |
| Customer details copied into order | `order_addresses` plus order snapshots |
| Payment fields on order | `order_payment_allocations` plus order summary |
| Notes field | `order_notes` |
| Activities | `order_activities` |
| Attachment records | `order_attachments` |
| Hold reason fields | `order_holds` |
| Cancellation fields | `order_cancellations` |
| Status text changes | `order_status_history` |
| Assigned department changes | `order_department_history` |
| Revision column | `orders.revision` and `order_revision_history` |
| Apps Script logs | `order_audit_events` |
| Product fields copied into order items | sale-time item snapshots |
| External marketplace reference | `external_source` and `external_order_id` |

The migration must not simply copy every sheet column into one wide SQL table.

It must preserve meaning while normalizing repeated and historical records.

---

# 29. Migration Data Classification

During SMP1, every current order field must be classified as one of:

```text
Authoritative
Derived
Snapshot
Reference
Legacy-only
Deprecated
Unknown
```

## Authoritative

Must migrate directly and preserve meaning.

## Derived

Should be recalculated and reconciled.

## Snapshot

Must preserve historical transactional value.

## Reference

Must map to another domain record.

## Legacy-only

Retained in migration evidence but not necessarily exposed in the new application.

## Deprecated

Intentionally removed after documented approval.

## Unknown

Blocks migration until resolved.

No field should be silently discarded.

---

# 30. Reconciliation Requirements

For every migrated order:

```text
Legacy Order ID = Standalone Order Number or preserved legacy reference
Item quantity totals reconcile
Subtotal reconciles
Tax reconciles
Grand total reconciles
Paid amount reconciles
Outstanding reconciles
Status maps explicitly
Customer reference resolves
External references remain intact
Audit source records migration origin
```

Migration discrepancies must be classified as:

```text
Exact Match
Accepted Normalization
Data Quality Correction
Unresolved Difference
Migration Failure
```

---

# 31. Privacy and Security

Order data may contain:

- names;
- phone numbers;
- addresses;
- email addresses;
- customer photographs;
- payment references;
- business GST details.

Therefore:

- access must be role-controlled;
- logs must avoid unnecessary personal data;
- files should use controlled URLs;
- database backups must be encrypted;
- production data must not be copied into test environments without masking;
- archived orders must remain protected;
- export actions must be auditable.

---

# 32. Retention

Initial canonical retention recommendation:

```text
Commercial order records: Permanent or statutory/business retention
Invoices and tax records: Per applicable legal retention
Audit events: Long-term append-only
Customer assets: Per approved business and privacy policy
Temporary processing files: Short-lived
Operational logs: Time-bounded according to observability policy
```

Legal and tax retention must be finalized separately during SMP1 compliance design.

---

# 33. Stage 2 Certification Result

The Order Management canonical data model now defines:

- root Order storage;
- Order Items;
- item assets;
- transactional address snapshots;
- charges;
- discounts;
- payment allocations;
- notes;
- activities;
- attachments;
- holds;
- cancellations;
- commercial status history;
- departmental history;
- revision history;
- audit events;
- constraints;
- indexes;
- transaction boundaries;
- migration mapping;
- reconciliation rules.

This model preserves the business architecture while making the domain suitable for PostgreSQL, multi-user concurrency, reliable reporting, and long-term platform growth.

# Next Canon Stage

## Order Management Canon v1.0 — Stage 3  
### State Machines, Transition Rules and Operational Gates

Stage 3 will define:

```text
Commercial Order State Machine
Payment State Machine
Personalization State Machine
Fulfilment State Machine
Hold State Machine
Cancellation and Refund State Machine
Department Ownership Transitions
Production Release Gates
Dispatch Release Gates
Closure and Archival Gates
```

It will specify:

- permitted transitions;
- blocked transitions;
- required permissions;
- required fields;
- automatic side effects;
- generated events;
- rollback rules;
- exceptional override procedures.