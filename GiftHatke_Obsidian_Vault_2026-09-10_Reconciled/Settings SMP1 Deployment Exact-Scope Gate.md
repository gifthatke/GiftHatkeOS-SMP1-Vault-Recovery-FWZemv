# Settings SMP1 Deployment Exact-Scope Gate

Date: 2026-09-12
Status: Passed; deployment authorized
Branch evidence evaluated: `216ed499843465fb58a41e3e8decf03976316ef4`
Governance evidence: `5fe93c7de9f220178c2c92b6e40f278131807edc`

The Settings implementation passed the pre-deployment exact-scope gate with 1,149 tests and zero failures, the full workspace typecheck, the exact Render build order, and the prohibited-scope scan.

Authorized migration scope is limited to the two frozen Settings foundation migrations and the four exact ERP8.5, ERP8.7, ERP8.8, and ERP8.9 default-seed migrations already committed on `smp1/production-parity`. The certified migrator must use the dedicated direct Neon migrator connection. No ad hoc SQL or pooled application connection is authorized.

Authorized deployment scope is limited to the existing Render API and static web services at one immutable branch head. Automatic deployment stays disabled and the existing same-origin topology stays unchanged.

No closed module is reopened. User Management remains read-only. Secret-backed banking writes remain fail closed unless separate frozen evidence identifies an already-authorized provider.

Next evidence: migration result, API and web deployment identities, authenticated nine-tab Settings acceptance, closed-module regression, application-origin error check, and Settings certification/closure.
