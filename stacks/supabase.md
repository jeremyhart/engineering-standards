# Supabase

**Detect:** `supabase/` directory, `supabase/config.toml`, `@supabase/*` dependencies

Stack-specific guidance per standard. Section names match standard names in `standards.md` exactly.

## Migrations

`supabase migration new` / versioned SQL in `supabase/migrations/`; no schema edits via Studio in staging/prod.

## Migrations in pipeline

`supabase db push` (or migration apply) runs in the deploy pipeline before code deploy.

## Migrations rehearsed on staging

Branch databases or a staging project; every migration applied there first.

## Seeding script

`supabase/seed.sql` (or a seed script) populates local/branch DB with representative data.

## Least-privilege access

Row Level Security enabled on all tables; policies reviewed. `service_role` key server-side only.

## Runtime secrets management

`service_role` and DB credentials in the platform secret store; `anon` key only in clients.

## Backup & restore procedure

PITR enabled on paid projects; restore procedure documented and tested.

## Prod access control

Studio/dashboard access limited and reviewed; direct DB access via defined roles only.

## Auth gates deny by default

RLS on every table with no permissive default policies; new tables get deny-all until policies added.

## Personal data handling

PII columns documented; `ON DELETE CASCADE` from the user row (or a delete function) gives a working delete-user path; retention via `pg_cron` cleanup jobs.

## Audit logging

Audit triggers (or `pgaudit`) on security-relevant tables; Supabase auth logs retained.

## Integration testing

Tests run against a local `supabase start` stack or branch database — real Postgres with RLS active, not mocks.
