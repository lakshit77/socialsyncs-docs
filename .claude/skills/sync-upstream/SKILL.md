---
name: sync-upstream
description: Syncs the local fork of postiz-docs with the upstream gitroomhq/postiz-docs repository. Use this skill whenever the user says "sync upstream", "pull from upstream", "update from upstream", "check for upstream changes", "sync with original repo", "sync docs", or invokes /sync-upstream. Also trigger when the user asks if there are new changes from the original postiz-docs repo.
---

# sync-upstream (docs)

Sync this fork of the documentation with the upstream `gitroomhq/postiz-docs` repository. Follow these steps exactly, in order.

> **What's different from the app sync:** This is a Mintlify docs repo — there is **no database, no Prisma schema, and no `fix-postiz-branding` skill** here. The one fork-specific concern is **rebranding**: parts of these docs have been rebranded from "Postiz" to "SocialSyncs" (e.g. `mcp/setup.mdx`). A merge can reintroduce upstream "Postiz" strings or overwrite rebranded pages, so Step 5 surfaces those for **manual** review — it does NOT auto-replace anything.

## Step 1 — Ensure upstream remote exists

```bash
git remote -v
```

If `upstream` is not listed, add it:

```bash
git remote add upstream https://github.com/gitroomhq/postiz-docs.git
```

## Step 2 — Fetch latest upstream changes

```bash
git fetch upstream
```

## Step 3 — Check for new commits

```bash
git log HEAD..upstream/main --oneline
```

If this output is empty, tell the user: "Your docs fork is already up to date with upstream. No new commits to merge." and stop.

If there are commits, show the user the list so they know what's incoming.

## Step 4 — Dry-run merge to detect conflicts

```bash
git merge --no-commit --no-ff upstream/main
```

### If the merge succeeds (no conflicts):

The output will say something like "Automatic merge went well; stopped before committing as requested."

Complete the merge:

```bash
git merge --ff upstream/main
```

Do NOT push to origin yet, and do NOT tell the user "synced successfully" yet. The rebrand check (Step 5) must run first, because new upstream pages may carry "Postiz" branding that needs to be reconciled with the SocialSyncs rebrand. Continue to Step 5.

### If the merge fails (conflicts detected):

The output will mention "CONFLICT" and list affected files.

Abort immediately:

```bash
git merge --abort
```

Tell the user clearly:
- Which files have conflicts (parse the CONFLICT lines from the merge output).
- That they need to resolve these manually before syncing. Conflicts here are most likely in pages you have already rebranded (e.g. `mcp/setup.mdx`) or in `docs.json` navigation.
- Suggested next steps: open the conflicting files, look for `<<<<<<<` markers, resolve (keeping your SocialSyncs wording), then run `git add <file>` and `git commit`.

Do NOT attempt to auto-resolve conflicts. Do NOT push anything. Just report and stop.

## Step 5 — Rebrand check after merge (review only)

Upstream content uses the "Postiz" brand and `postiz.com` URLs. Now that new content has landed, scan **only the files this merge touched** for brand strings that may need to be rebranded to "SocialSyncs".

First, list the files the merge changed:

```bash
git diff --name-only 'HEAD@{1}' HEAD
```

Then scan those changed files for brand references:

```bash
git diff --name-only 'HEAD@{1}' HEAD -z \
  | xargs -0 grep -n -i -E 'postiz|api\.postiz\.com|uploads\.postiz\.com|platform\.postiz\.com|docs\.postiz\.com' 2>/dev/null
```

- If there are **no matches**: tell the user "No Postiz brand strings in the synced files — nothing to rebrand." and continue to Step 6.
- If there ARE matches: do NOT edit them automatically. Present the user a grouped list (file → line → matched text) and explain that these came in from upstream and may need to be rebranded to SocialSyncs (matching the existing rebrand in pages like `mcp/setup.mdx`). Ask whether they want you to rebrand them now or leave them as-is. Only edit after the user decides.

> Why review-only: unlike the app (which has a dedicated `fix-postiz-branding` skill), the docs rebrand is partial and intentional in places. Some "Postiz" references may be correct to keep (e.g. links to the upstream GitHub repo `gitroomhq/postiz-app`). A blind replace would break those, so a human decides.

## Step 6 — Verify the docs still build

Remind the user to preview and validate before publishing:

```bash
# from the postiz-docs root
mintlify dev            # local preview at http://localhost:3000 (use --port if 3000 is taken)
mintlify broken-links   # validate internal links, esp. if docs.json navigation changed
```

If `docs.json` was among the changed files (check the Step 5 file list), call this out specifically — navigation/page-slug changes are the most common source of broken links after a docs sync.

## Step 7 — Finalize: push to origin

Reach this step ONLY when the merge succeeded (Step 4) and the rebrand check (Step 5) was reviewed with the user. If rebrand edits were made, commit them together with the merge.

Ask the user to confirm before pushing, then:

```bash
git add -A
git commit -m "Sync upstream docs" || true
git push origin main
```

Then give the user a single closing summary:
- "Synced successfully. Merged N new commits from upstream into the docs and pushed to origin/main."
- Rebrand status — one of:
  - "No Postiz brand strings in the synced files — nothing to rebrand."
  - "Found Postiz brand strings in <files>; rebranded to SocialSyncs as part of this sync."
  - "⚠️ Found Postiz brand strings in <files> — left as-is per your decision. Review before publishing."
- Build reminder: "Run `mintlify dev` to preview and `mintlify broken-links` to validate before publishing."

If Step 4 aborted on a conflict, do NOT push silently as if everything is fine — surface the conflict prominently and stop.
