# Settings SMP1 Family Wave — Company, Organization, and Sequences

Status: IMPLEMENTED / NOT CERTIFIED
Date: 2026-09-11
Frozen Apps Script authority: fd7c754fb1be380e6d3f9b01dd041b97b82f1d87

The first exact Settings family sub-wave now covers:

- ERP8.2 Company: profile, addresses, masked/protected bank accounts, invoice identity, primary uniqueness, versions, and archive semantics.
- ERP8.3 Organization: branches, departments, cost centres, hierarchy/reference checks, code uniqueness, versions, and archive semantics.
- ERP8.4 Document Sequences: definitions, exact vocabularies, preview, financial-year reset, allocation, append-only issuance, and duplicate protection.

Transport uses the authenticated principal, CSRF protects writes, and frozen family permissions are contained by existing Settings manage authority. No permission #110 or other new active catalogue entry is added.

Remaining exact Settings family waves are ERP8.5 Tax/Finance, ERP8.6 Marketplace, ERP8.7 Production configuration, ERP8.8 Inventory/Procurement configuration, and ERP8.9 Notification/Communication. Secure secret-provider wiring, migration execution, deployment, live acceptance, certification, and closure remain pending.

This is a milestone record only.
