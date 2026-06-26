# SocialSyncs Docs — Branding Reference

Mintlify docs have **no `branding.ts`** like the app (`postiz-app/apps/frontend/src/lib/branding.ts`). All brand identity for the docs lives in **`docs.json`** plus the asset files in `logo/`. This file documents where each value lives so a rebrand is a one-file edit, mirroring the app's single-source-of-truth pattern.

## Brand values (kept in sync with the app's `branding.ts`)

| Token | Value | Source in app `branding.ts` |
|---|---|---|
| Brand name | `SocialSyncs` | `BRAND_NAME` |
| Primary color | `#2B7DE9` | `PRIMARY_PALETTE['600']` / `btnPrimary` |
| Light accent | `#4A9AF5` | `PRIMARY_PALETTE['400']` / `accent` |
| Dark shade | `#0B3D8C` | `PRIMARY_PALETTE['700']` / `btnPrimaryHover` |
| Logo / favicon | `logo/socialsyncs-icon.svg` | `/socialsyncs-icon.svg` (frontend `public/`) |
| Cloud / domain | `socialsyncs.co` | `DOMAIN` (note: app uses `.com`, docs use `.co`) |

## Where each lives in `docs.json`

- **Name:** `"name": "SocialSyncs Documentation"`
- **Colors:** `"colors": { "primary": "#2B7DE9", "light": "#4A9AF5", "dark": "#0B3D8C" }`
- **Logo:** `"logo": { "light": "/logo/socialsyncs-icon.svg", "dark": "/logo/socialsyncs-icon.svg" }`
- **Favicon:** `"favicon": "/logo/socialsyncs-icon.svg"`
- **Cloud button:** `"navbar.primary.href": "https://socialsyncs.co"`

## Service URLs used throughout the `.mdx` content

| Purpose | URL |
|---|---|
| API base | `https://app.socialsyncs.co/api` |
| Dashboard / get API key | `https://app.socialsyncs.co` |
| Media / uploads CDN | `https://uploads.socialsyncs.co` |
| Docs | `https://docs.socialsyncs.co` |

## Identifiers that intentionally keep "postiz" (do NOT rebrand)

These are real upstream/technical names — renaming them would make instructions wrong:

- GitHub repos: `gitroomhq/postiz-app`, `gitroomhq/postiz-docker-compose`, `gitroomhq/postiz-helmchart`
- Docker image: `ghcr.io/gitroomhq/postiz-app`
- npm packages: `@postiz/node`, `n8n-nodes-postiz`
- Env vars: `POSTIZ_API_KEY`, `POSTIZ_OAUTH_*`, `POSTIZ_GENERIC_OAUTH`, etc. (the app still reads these)
- Infra placeholders in examples: `postiz-uploads` bucket, `/data/postiz`, `postiz-db-local`, `postiz-user`

> The agent skill published at [github.com/lakshit77/socialsyncs-agent](https://github.com/lakshit77/socialsyncs-agent) uses the rebranded `SOCIALSYNCS_API_KEY` env var, separate from the app's internal `POSTIZ_API_KEY`.

## To re-theme

1. Edit the `colors` block in `docs.json`.
2. Replace `logo/socialsyncs-icon.svg` (and update `logo`/`favicon` paths if renamed).
3. Run `mintlify dev` to preview and `mintlify broken-links` to validate.
