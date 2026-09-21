# Settings SMP1 Frozen Workspace UI

Date: 2026-09-12
Status: Implemented; pre-deployment
Authority: frozen Apps Script commit `fd7c754fb1be380e6d3f9b01dd041b97b82f1d87`

The Standalone Settings workspace now surfaces the exact nine frozen tabs: Configuration, Company Profile, Addresses, Banking, Invoice Identity, Organization, Document Sequences, Tax & Finance, and Marketplaces.

Implemented behaviour:

- core values can be saved and reset;
- company profile, address, and masked bank-account forms use the frozen field sets;
- invoice identity remains derived and read-only;
- branches, departments, and cost centres use the frozen hierarchy contracts;
- document sequences support definition administration and non-allocating preview;
- all eight Tax & Finance families and all five marketplace families are surfaced;
- every mutation bootstraps a same-origin CSRF token and sends the required request proof.

Evidence commits:

- `21be236ed7e50600f4dda4d2ea5c5cfe4af401d5`
- `6ff7345e1aa4b5ec71548c7b5e13dd65ca546070`
- `ac8052626b06db20db35c2c64e48d5f911eabd17`
- `1b9da32e1d1c5a29eab8864ff1657f20e8b0c29a`
- governance milestone: `e78dcfaed200eb5fbeeaad92dd9e4322c086d232`

Focused validation passed: strict TypeScript, syntax, exact nine-tab scope, frozen fields, same-origin CSRF, and prohibited-scope exclusions.

Pending before closure: repository-wide regression, deployment gate, migration/deployment execution, authenticated live acceptance, and final certification. Sensitive banking persistence remains fail-closed until the deployment topology supplies an evidence-authorized secret provider.
