# Umami deployment

This fork runs Umami on Netlify with PostgreSQL hosted by Supabase.

## Production

- Netlify project: `stln-umami-analytics`
- Public URL: `https://stln-umami-analytics.netlify.app`
- Supabase project: `general_analytics` (`jqpaorlkzjoubggzwpqx`)
- PostgreSQL schema: `umami`
- PostgreSQL role: `umami_app`

Umami is isolated from the existing analytics tables by its PostgreSQL schema
and database role. Do not remove the `schema=umami` parameter from either
database URL.

The database password is stored in the local macOS Keychain under service
`umami-supabase-jqpaorlkzjoubggzwpqx` and account `umami_app`. Netlify stores
the database URLs and application secret as secret production environment
variables.

## Required Netlify environment variables

- `DATABASE_URL`
- `DIRECT_DATABASE_URL`
- `APP_SECRET`
- `DISABLE_TELEMETRY=1`
- `PRIVATE_MODE=1`
- `SALT_ROTATION=month`
- `DEFAULT_LOCALE=sv-SE`
- `DEFAULT_CURRENCY=SEK`
- `PNPM_VERSION=12.3.4`

## Updating Umami

```bash
git fetch upstream
git checkout master
git merge --ff-only upstream/master
git push origin master
```

Pushing `master` triggers the Netlify production build. The Umami build runs
Prisma migrations against the isolated `umami` schema before deploying.

## Privacy defaults for app migrations

When adding each application, exclude URL search parameters and fragments,
restrict the tracker to the application's production domains, and keep replay
and heatmaps disabled until they have been reviewed separately.
