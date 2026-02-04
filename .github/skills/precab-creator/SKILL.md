---
name: precab-creator
description: Derive a precab release template from recent Confluence precab pages via MCP Atlassian, then draft and publish a new precab release page with the filled template.
allowed-tools: Bash Read Write
---

# Precab Creator (Confluence MCP)

## Overview
Use the MCP Atlassian server to mine recent precab release pages in Confluence, infer a canonical template, fill it with provided release metadata, and publish a new precab page (or return a draft) in the target space/parent.

## Workflow
1) Preconditions: MCP Atlassian server is configured with `ATLASSIAN_SITE`, `ATLASSIAN_USER_EMAIL`, and `ATLASSIAN_API_TOKEN` (kept outside the repo). Collect inputs: `spaceKey`, optional `parentId`, labels, release name/version/service/envs/dates/owner/JIRA links/artifacts/runbooks, and how many past pages to sample (default 3).
2) Discover sources: run `confluenceSearch` with CQL like `title ~ "precab" AND space = <SPACE> ORDER BY lastmodified DESC` (limit N). Skip pages that are incomplete or clearly off-template.
3) Ingest examples: for each candidate page, call `confluenceGetPage` (expand `body.storage,version`). Capture headings, tables, status macros, and recurring fields.
4) Derive template: normalize section order (e.g., Overview, Scope, Preconditions, Change Log, Validation, Rollback, Owners, Links). Unify field names; preserve useful macros; identify required vs optional fields.
5) Fill draft: merge user-supplied metadata; insert `[TODO]` placeholders for missing required fields; keep Confluence storage/ADF formatting (tables/headings). Suggest labels (e.g., `precab`, `release`, `<service>`).
6) Publish: by default perform a dry-run and return the storage body. If the user approves or requests publish, call `confluenceCreatePage` with `spaceKey`, `title`, `body`, `parentId` (if provided), and labels. Optionally, use `confluenceUpdatePage` if editing an existing draft.
7) Report: summarize what pages were sampled, what template was derived, and where the draft/published page lives.

## Tools & Assets
- MCP Atlassian tools: `confluenceSearch`, `confluenceGetPage`, `confluenceCreatePage`, `confluenceUpdatePage`, optional `confluenceListAttachments`.
- Configuration is supplied via MCP env (not stored in the repo). Ensure the MCP host is restarted after editing the config.

## Rules (Must/Must Not)
- Must not echo or store API tokens, emails, or site domains in skill output or repo files.
- Must not create pages without explicit `spaceKey` and a confirmed parent (or explicit acceptance of no parent) to avoid orphan pages.
- Must keep existing macros/tables intact when deriving the template; prefer additive normalization over destructive rewrites.
- Must add labels for discoverability when publishing; use sensible defaults (`precab`, `release`, service name).
- Must place `[TODO]` markers where required fields are missing rather than inventing data.

## Safety Protocol
- Never write credentials to disk or logs; rely on MCP env configuration only.
- Default to dry-run (return draft body) unless the user explicitly says to publish.
- Validate `spaceKey`/`parentId` before creation; fail closed if uncertain.
- Avoid duplicating titles; if a page with the same title exists, confirm update vs new.
- Do not alter git config or run destructive git commands.

## Golden Examples
- "Find the last 3 precab release pages in space XYZ, derive a template, and show me a draft for `Service Alpha v1.4.0` (dry-run)."
- "Using parent page 123456 in space XYZ, publish a precab release page for `Payments v2.3.1` with labels `precab,releases,payments`; use the derived template from recent precab pages."
