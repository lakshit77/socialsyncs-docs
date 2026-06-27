# SocialSyncs Docs — Session Handoff & Work Log

> **Purpose:** The running record of all work done on `postiz-docs`, so any future
> session can pick up without re-deriving context. When you tell a new session
> *"continue the docs work"*, point it here first.
>
> **How to use:** Read **§1 Current State** for where things stand right now, then
> **§5 Open / Not-Started** for what to do next. **§3 Work Log** is the chronological
> history. Update this file at the end of any session that changes the repo.
>
> Last updated: **2026-05-29**.

---

## 1. Current State (read this first)

- **Branch:** `main`. **17 commits ahead of `origin/main` — NOTHING pushed yet.**
- **Uncommitted working-tree changes:**
  - `docs.json` — the provider-visibility nav curation (see §4). **Not committed.**
  - `PROVIDER-VISIBILITY-STATUS.md` — the predecessor of this file (untracked). Superseded
    by this `SESSION-HANDOFF.md`; safe to delete once this file is committed.
- **Last commit:** `aa09a65` (full rebrand + brand colors/logo).
- **`mintlify broken-links`** passed clean as of last run.
- User is **intentionally pausing**. No work in progress mid-edit.

### Repo facts
- **Mintlify** project — config in `docs.json`, **no `package.json`**. Run with `mintlify dev`
  (preview at http://localhost:3000), validate with `mintlify broken-links`.
- Git remotes: `origin` = `lakshit77/postiz-docs`, `upstream` = `gitroomhq/postiz-docs`.
- Sibling repos under `/Users/lakshitukani/Lakshit/coding/postiz/`: `postiz-app` (the
  rebranded SocialSyncs product), `postiz-docs` (this), `postiz-website`.

---

## 2. Project background

`postiz-docs` is the documentation site for **SocialSyncs**, a rebranded fork of the
open-source **Postiz** social-media scheduler. The work across sessions has been:
(a) tooling to stay in sync with upstream Postiz docs, (b) a full rebrand Postiz→SocialSyncs,
(c) publishing an agent skill, and (d) hiding docs for providers not yet supported.

---

## 3. Work Log (chronological)

### Session A — sync skill, upstream merge, full rebrand, agent skill
Commits: `842be0f`, `5660eeb` (merge), `5d40bfd`, `aa09a65`.

1. **Built a `sync-upstream` skill** at `.claude/skills/sync-upstream/SKILL.md` + a
   `CLAUDE.md` registering it (trigger: `/sync-upstream`). Mirrors the app's sync skill but
   for Mintlify docs (no DB/branding-skill deps). **Conflict rule:** trivial brand-only
   conflicts auto-resolve; non-trivial ones abort and ask the user.
2. **Synced 13 upstream commits** from `gitroomhq/postiz-docs` (merge `5660eeb`). One trivial
   conflict in `mcp/setup.mdx` auto-resolved (a duplicate Claude Code section).
3. **Full rebrand to SocialSyncs** (`5d40bfd` for merged files, `aa09a65` for the rest of the
   repo + branding):
   - "Postiz" → "SocialSyncs"; service URLs → `api/platform/uploads/docs.socialsyncs.co`
     (TLD is **`.co`**, not `.com`).
   - **Preserved technical identifiers (do NOT rebrand these):** `gitroomhq/postiz-app`,
     `gitroomhq/postiz-docker-compose`, `gitroomhq/postiz-helmchart` GitHub repos;
     `ghcr.io/gitroomhq/postiz-app` Docker image; `@postiz/node` & `n8n-nodes-postiz` npm
     packages; all `POSTIZ_*` env vars (the app still reads these — verified `POSTIZ_API_KEY`
     is real in app code).
   - Removed Postiz-only promos (Agent Media/OpenClaw banner, dead `@postizofficial` YouTube)
     and Discord **community** links — but **kept the Discord *provider* feature** docs.
   - Brand colors in `docs.json`: purple `#9900e6` → **blue `#2B7DE9`** (light `#4A9AF5`,
     dark `#0B3D8C`), matching `postiz-app/apps/frontend/src/lib/branding.ts`.
   - Logo + favicon → `logo/socialsyncs-icon.svg` (copied from the app's `public/`).
   - Added `BRANDING.md` — the docs single-source-of-truth brand reference.
   - `mintlify broken-links` passed clean.
4. **Published a public agent skill** at **`github.com/lakshit77/socialsyncs-cli`**
   (install: `npx skills add lakshit77/socialsyncs-cli`; uses env `SOCIALSYNCS_API_KEY`).
   The docs CLI page (`cli/introduction.mdx`) points the install command at this repo.

### Session B — provider visibility (this session)
Change is in the working tree, **uncommitted**. Details in §4.

---

## 4. Provider Visibility feature (current uncommitted work)

**Goal:** SocialSyncs currently offers only **2 providers** to users (product decision):
**Instagram (standalone)** and **YouTube**. Hide the docs for all other providers while
**keeping their `.mdx` files on disk**, so platforms can be re-enabled one by one later.

> ⚠️ **Docs-visibility decision only**, not a code restriction. In the app, *all* providers
> are still registered in
> `postiz-app/libraries/nestjs-libraries/src/integrations/integration.manager.ts`. We are
> only hiding the **docs pages**.

**Approach:** curate the `pages` lists in `docs.json`. `.mdx` files untouched on disk; to
re-enable a platform, re-add its slug to the relevant `pages` array.

### Changes made in `docs.json`

**Documentation tab → "Providers" group:**

| Sub-group        | Before                                                                                                                       | After                          |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| Social Platforms | x-twitter, linkedin, linkedin-page, facebook, **instagram**, threads, bluesky, mastodon, google-my-business, mewe, farcaster | **instagram** only             |
| Video Platforms  | **youtube**, tiktok                                                                                                          | **youtube** only               |
| Other Platforms  | reddit, pinterest, discord, slack, telegram, dribbble, skool, whop                                                          | **removed** (empty → deleted)  |

`providers/overview` and `configuration/create-provider` kept at top of the group.

**Public API tab → "Provider Settings" group:**
- Group label renamed: **"Provider Settings (25 with custom settings)"** → **"Provider Settings"**.
- Kept only `public-api/providers/instagram-standalone` and `public-api/providers/youtube`.
- Hid the other 29 (x, linkedin, linkedin-page, facebook, instagram, threads, bluesky,
  mastodon, warpcast, nostr, vk, tiktok, reddit, lemmy, pinterest, discord, slack, telegram,
  dribbble, medium, devto, hashnode, wordpress, listmonk, gmb, whop, skool, kick, twitch,
  moltbook).

**Validation:** `docs.json` valid JSON; `mintlify broken-links` passed clean.

### Slug mapping nuance (important)
- **Documentation tab:** Instagram page = `providers/instagram` (this page documents BOTH the
  Facebook-Business flow AND the standalone flow — kept as-is, not trimmed).
- **Public API tab:** standalone variant = `public-api/providers/instagram-standalone`. There
  is also a `public-api/providers/instagram.mdx` (FB-linked flow) which is **hidden**.

### How to RE-ENABLE a provider later
1. Edit `docs.json`.
2. **Documentation tab:** add the slug (e.g. `"providers/tiktok"`) to the right sub-group's
   `pages` under "Providers". To re-add an "Other Platforms" provider, recreate the deleted
   sub-group object:
   ```json
   { "group": "Other Platforms", "expanded": true, "pages": [ "providers/reddit" ] }
   ```
3. **Public API tab:** add the slug (e.g. `"public-api/providers/tiktok"`) to "Provider Settings".
4. Run `mintlify broken-links`, then `mintlify dev` to preview.

Full original lists are in git history (commit `aa09a65` had the complete `pages` arrays) and
in the tables above.

---

## 5. Open / Not-Started (next steps)

- [ ] **Commit the provider-visibility change** (`docs.json`) — currently uncommitted.
- [ ] **PUSH everything.** All 17 commits + the working-tree change are **local only**;
      nothing on `lakshit77/postiz-docs` yet. Needs explicit user go-ahead.
- [ ] **(Optional) Trim `providers/instagram.mdx`** to standalone-only (remove Facebook-Business
      flow sections). Currently kept as-is.
- [ ] **Delete `PROVIDER-VISIBILITY-STATUS.md`** once this `SESSION-HANDOFF.md` is committed
      (it's the older, narrower version of this file).
- [ ] **Restart `mintlify dev` + hard-refresh** when previewing — it caches aggressively;
      rebrand/visibility changes won't show until restart.

---

## 6. Reminders for whoever resumes

- This is a **production app with real users**. Per global `CLAUDE.md`:
  **never start implementing without explicit user permission.**
- Rebrand rule of thumb: rebrand product-name prose & service URLs (`*.socialsyncs.co`);
  **never** rebrand the protected identifiers listed in §3.3.
- Instagram slug: Documentation tab = `providers/instagram`; Public API tab =
  `public-api/providers/instagram-standalone`.
- **Update this file** at the end of any session that changes the repo.
