# sync-upstream
- **sync-upstream** (`.claude/skills/sync-upstream/SKILL.md`) - syncs this docs fork with upstream `gitroomhq/postiz-docs` (fetch → check for new commits → dry-run merge → rebrand review → build check → push). Trigger: `/sync-upstream`
When the user types `/sync-upstream`, invoke the Skill tool with `skill: "sync-upstream"` before doing anything else.

# postiz-docs — Project Guide

This is the SocialSyncs documentation site, forked from `gitroomhq/postiz-docs`. It is a **Mintlify** project (config in [docs.json](docs.json)) — there is no `package.json`; the Mintlify CLI runs it.

## Run locally
```bash
mintlify dev            # local preview at http://localhost:3000 (use --port if taken)
mintlify broken-links   # validate internal links before publishing
```

## Structure
- Content is `.mdx` files (e.g. `cli/`, `mcp/`, `providers/`, `public-api/`).
- [docs.json](docs.json) defines navigation, tabs, theme, and the page slug list — broken-link issues after a sync usually originate here.
- `snippets/` holds reusable `.mdx` fragments.

## Rebrand note
Parts of these docs have been rebranded from "Postiz" → "SocialSyncs" (e.g. [mcp/setup.mdx](mcp/setup.mdx)). When syncing from upstream, new content may reintroduce "Postiz" strings — the `sync-upstream` skill surfaces these for manual review rather than auto-replacing them (some "Postiz" references, like the upstream GitHub repo link `gitroomhq/postiz-app`, are correct to keep).
