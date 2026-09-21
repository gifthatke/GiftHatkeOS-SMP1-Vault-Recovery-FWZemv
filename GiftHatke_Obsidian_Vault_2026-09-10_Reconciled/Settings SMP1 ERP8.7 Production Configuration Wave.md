# Settings SMP1 ERP8.7 Production Configuration Wave

Status: IMPLEMENTED — pre-deployment validation complete  
Date: 2026-09-11  
Frozen authority: `GiftHatkeOS@fd7c754fb1be380e6d3f9b01dd041b97b82f1d87`

Frozen ERP8.7 Production configuration behavior is implemented on the existing
workshop, machine, stage, priority, QC-rule, and default repositories. The
surface includes listing, record lookup, six save families, archive behavior,
typed default and plan resolution, workspace summaries, and edit options.

The exact seed gate authorizes sixteen defaults: one workshop, one machine,
six stages, four priorities, and four QC rules. No Production default row is
invented. The migration is seed-only and does not touch operational jobs.

Reads and writes stay within the existing Settings permission bridge; writes
are CSRF protected. No new permission, Production job workflow, operational
Production route, dashboard, User Management write path, ERP9/ERP10, Version
1.1, or Domain 45 scope was introduced.

Focused syntax, behavior, route, seed, migration, and closed-module boundary
gates pass. Full regression and deployment remain pending; neither Settings
nor Production is recertified or closed by this milestone.
