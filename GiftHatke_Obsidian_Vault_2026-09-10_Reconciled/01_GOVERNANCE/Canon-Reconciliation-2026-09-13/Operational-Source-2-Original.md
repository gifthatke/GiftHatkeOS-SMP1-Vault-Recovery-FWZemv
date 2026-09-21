# SMP1 Standalone v1.0 DigitalOcean Deployment Target Selection

## Status

SELECTED WITH CONTROL / RESOURCES NOT CREATED

## Certified application baseline

`d7bc314e11f2aacfcf9ece273888d2cecf501c11`

## Parent release-topology governance

`2f8d5fa2e9866f590931715f445d032f14258194`

The certified Standalone v1.0 implementation remains unchanged.

This record selects the operational hosting and PostgreSQL target only.

## Selected hosting provider

DigitalOcean App Platform.

Selected App Platform region:

`blr` — Bangalore.

## Selected PostgreSQL provider

DigitalOcean Managed PostgreSQL.

Selected database region:

`blr1` — Bangalore.

The application and PostgreSQL deployment are intentionally co-located.

A development database is not approved for production.

## Certified application composition

One DigitalOcean App Platform application is the public deployment boundary.

The application contains a static-site frontend component and a Node.js API service component.

The browser continues to see one public GiftHatkeOS application origin.

App Platform ingress routing will route certified API paths to the API component while the frontend remains the default static-site component.

API request paths must be preserved exactly when routed to the service.

Cross-site credentialed API transport remains prohibited.

No CORS redesign is authorized.

## Runtime contract

The API service uses Node.js 24.x.

The certified repository engine constraint `>=24 <25` remains authoritative.

The existing API runtime uses the certified HOST and PORT binding contract.

The production API health check remains `GET /health`.

## Database contract

DigitalOcean Managed PostgreSQL is the shared persistence foundation.

The runtime application identity receives `DATABASE_URL` outside source control.

The migration identity receives `DATABASE_MIGRATOR_URL` outside source control.

Runtime and migration credentials remain separately controllable.

No production database has been created by this governance record.

No database migration has been executed.

## Migration execution model

DigitalOcean deploy-time job capability is approved as the provider mechanism available for a later migration gate.

Automatic migration execution is NOT authorized by this selection.

The exact migrator command, database identity and target must pass the dedicated database migration gate before execution.

## Web build contract

The static frontend build requires `VITE_GOOGLE_AUTH_CLIENT_ID`.

The production Google OAuth authorized JavaScript origin must match the final public GiftHatkeOS origin.

The final public domain is not selected by this record.

DNS mutation is not authorized by this record.

## HTTPS and cookie contract

Public browser traffic must use HTTPS.

Authentication and CSRF cookies remain SameSite=Lax.

Production Secure-cookie behavior remains mandatory.

The explicit `x-csrf-token` contract remains unchanged.

## Ingress ownership

DigitalOcean App Platform ingress is selected for public component routing.

The exact API route-prefix inventory must be reconciled from certified source before an App Platform specification is created.

Path preservation must remain enabled for API ingress rules.

The default root route will belong to the static frontend.

No ingress rule may shadow a certified API route.

## Scaling ownership

The topology may begin with one API instance.

The provider target must retain the ability to run replaceable API instances against shared PostgreSQL.

Correctness must not depend on one process-local instance.

## Secrets ownership

Secrets and credentials are injected through provider environment configuration.

No secret value may be committed to Git.

At minimum, deployment configuration will later resolve DATABASE_URL, DATABASE_MIGRATOR_URL and VITE_GOOGLE_AUTH_CLIENT_ID together with all other certified environment requirements.

## Git ownership

The standalone repository still has no configured `origin` remote.

Git remote selection and push remain a separate gate.

Provider source-repository connection must not be performed until the certified repository remote is established.

## Backup and recovery

Production uses DigitalOcean Managed PostgreSQL rather than an App Platform development database.

Backup retention, recovery verification, trusted-source configuration and production database sizing remain separate infrastructure-readiness controls.

## Explicit non-authorization

This selection does NOT authorize:

- creating a DigitalOcean App Platform application;
- creating a DigitalOcean PostgreSQL cluster;
- creating provider deployment assets;
- executing database migrations;
- changing DNS;
- creating OAuth credentials;
- modifying OAuth authorized origins;
- configuring the Git remote;
- Git push;
- creating a release tag;
- production deployment;
- application-source redesign;
- CORS redesign;
- permission #110;
- Domain 45;
- Version 1.1 / ERP9 / ERP10 work.

## Selected target result

HOSTING:

DIGITALOCEAN APP PLATFORM / BLR

DATABASE:

DIGITALOCEAN MANAGED POSTGRESQL / BLR1

STATUS:

SELECTED WITH CONTROL / RESOURCES NOT CREATED

## Next action

RECONCILE THE EXACT API ROUTE INVENTORY, COMPLETE ENVIRONMENT CONTRACT, BUILD COMMANDS, START COMMAND AND MIGRATION COMMAND BEFORE CREATING THE DIGITALOCEAN APP SPECIFICATION.
