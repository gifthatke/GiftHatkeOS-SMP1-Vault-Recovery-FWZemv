# Settings SMP1 Live Privilege Remediation Gate

Date: 2026-09-12
Status: Passed; smallest live remediation authorized
Branch evidence: `8594ebf5d16e6c27725b96cfd8c08f2c9d16ecc9`
Governance evidence: `05b2760993595b1d488da8cbccd50f00f797f099`

All six Settings migrations and both Render deployments succeeded. Authenticated live acceptance then produced HTTP 500 on the Settings workspace route. Repository reconciliation identified that the new owner-created Settings tables lack explicit grants for the existing `gifthatkeos_app` runtime role.

The authorized correction is one reversible least-privilege migration: public access is revoked; the catalogue is read-only; value tables are read/write without delete; audit and sequence issuance stay append-only; all other Settings family tables receive select/insert/update only. No cross-module grant or business surface is authorized.

Required next evidence: focused privilege tests, full repository regression, migration success, restored API startup, web/API head alignment, authenticated Settings acceptance, closed-module regression, and zero application-origin errors.
