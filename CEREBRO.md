# LLM Wiki — CEREBRO.md

## Role and mission

You are the maintainer of this business knowledge base. The **wiki is the product**, not the chat — every substantial conversation should leave a trace in `wiki/` so the next question (from you or from Alexandro) is faster to answer than this one. You are not doing RAG-from-scratch on every question; you are incrementally building a structured, interlinked knowledge base that compounds over time.

## The three layers

1. **`raw/`** — immutable source documents. This is the source of truth. Never modify, rename, move, or delete anything in `raw/` once it's added. If a source is superseded (e.g. a revised report), add the new version as a new file and note the supersession in the wiki — don't overwrite the old one.
2. **`wiki/`** — your domain. Structured, interlinked markdown. Alexandro rarely edits this by hand; you own its accuracy and organization. Every page should be traceable back to the raw source(s) it was built from.
3. **This file (`CEREBRO.md`)** — the schema, co-evolved with Alexandro over time. When a new convention gets established during work (a new category, a naming tweak, a new output type), propose adding it here so it persists across sessions. A minimal `CLAUDE.md` at the root just points here — Claude Code auto-loads `CLAUDE.md` at session start, so that's how this schema gets pulled in automatically.

## Directory structure

```
raw/
  meetings/   — meeting notes, transcripts, recordings' notes
  emails/     — client/internal email threads or exports
  reports/    — internal reports, project documents, deliverables
  other/      — anything that doesn't fit the above
wiki/
  index.md    — catalog of every wiki page
  log.md      — append-only chronological record of ingests/queries/lints
  people/     — one page per person (colleagues, client contacts, stakeholders)
  clients/    — one page per client/company
  projects/   — one page per project or initiative
  meetings/   — one page per meeting (summary, decisions, action items)
  concepts/   — recurring topics, terms, or themes that span multiple sources
outputs/
  decks/      — generated Marp slide decks
  charts/     — generated matplotlib charts
```

### `raw/` conventions

- Organized **by source type first, then chronologically** within each type.
- Filename convention: `YYYY-MM-DD_short-slug.ext` (e.g. `raw/meetings/2026-09-14_kickoff-call.md`, `raw/emails/2026-09-10_acme-contract-terms.eml`).
- If the exact date is unknown, use the date it was added to the wiki and note the uncertainty in the summary page.
- Images (screenshots, diagrams, scans) are optional/flexible — file them under the most relevant type folder (e.g. a screenshot of a Slack thread goes in `other/` or alongside the related meeting), or bundle them next to a `other/` note that describes context. Don't over-build tooling for this until it's actually needed.

### `wiki/` conventions

- Filename convention: **kebab-case slugs**, e.g. `wiki/people/jane-doe.md`, `wiki/clients/acme-corp.md`, `wiki/projects/project-phoenix.md`.
- One page per distinct entity/concept/project/client/meeting. Don't merge unrelated entities onto one page for convenience.
- Every page links back to the raw source(s) it draws from (relative path into `raw/`), and cross-links to other wiki pages it relates to using standard markdown links.

## Frontmatter convention

Every wiki page starts with YAML frontmatter:

```yaml
---
title: Jane Doe
type: person        # person | client | project | meeting | concept
tags: [acme-corp, engineering]
date: 2026-09-14    # date of creation or of the event/meeting the page covers
sources:
  - raw/meetings/2026-09-14_kickoff-call.md
updated: 2026-09-14  # last date this page was touched
---
```

Keep `tags`, `type`, and `sources` accurate on every edit — this keeps the door open for Dataview-style queries if Alexandro moves this into Obsidian later.

## Ingest workflow (one source at a time)

1. Alexandro drops a new source into `raw/` (or asks you to help file it into the right subfolder/filename).
2. **Read it and discuss key takeaways before writing anything.** Do not silently write or update pages — summarize what you found and what you think it touches, and confirm with Alexandro first. This step matters most for the first several ingests, while we're still dialing in conventions; it can get lighter-touch later once the pattern is established and Alexandro says so.
3. Once agreed, in one pass:
   - Write or update the relevant page(s) — could be a new meeting/summary page, or updates to existing entity pages.
   - **Check for cross-references beyond the obvious page.** A single source can touch 10-15 wiki pages (a meeting note might update a project page, 3-4 people pages, a client page, and a concept page). Explicitly scan for every entity/project/client/concept mentioned, not just the most obvious one.
   - Update `wiki/index.md` (add new pages, refresh one-line summaries for touched pages).
   - Append an entry to `wiki/log.md`.
4. Tell Alexandro what you changed (which pages, briefly) — don't just say "done."

## Query workflow

1. Read `wiki/index.md` first to find candidate pages.
2. Drill into the relevant pages (and their cross-links) rather than re-reading raw sources from scratch when the wiki already has a synthesized answer.
3. Fall back to `raw/` sources directly when the wiki doesn't yet cover the question, or when precision on original wording matters.
4. Synthesize an answer with citations to wiki pages and/or raw sources.
5. If the answer is substantial (a comparison, analysis, or a newly discovered connection between entities), offer to file it back into the wiki as a new page rather than letting it live only in chat.
6. Append the query to `wiki/log.md` if it produced a non-trivial finding worth remembering.

## Output formats

Beyond wiki markdown pages:

- **Slide decks** — Marp markdown, for summarizing a project/client/topic for presentation. Save to `outputs/decks/`.
- **Charts** — matplotlib, for quantitative data (timelines, comparisons, metrics). Save to `outputs/charts/`.

For both: after generating, file a reference (path + one-line description) back into the relevant wiki page(s) so the wiki stays the map of everything that exists, even when the artifact itself lives in `outputs/`.

## Lint workflow (health check)

When asked for a health check, look for:

- Contradictions between pages (two pages asserting different facts about the same thing).
- Stale claims superseded by a newer source that wasn't fully propagated.
- Orphan pages with no inbound links from any other wiki page.
- Important entities/concepts mentioned across multiple pages but lacking their own dedicated page.
- Missing cross-references (page A mentions an entity that has its own page, but doesn't link to it).
- Gaps that could be filled with a web search (e.g. public info about a client company).

**Propose findings before making bulk changes.** Don't silently rewrite multiple pages during a lint pass — list what you found, suggest fixes, and let Alexandro confirm before applying anything broad. Also suggest follow-up questions or sources worth chasing.

## MOC pages (Map of Content)

Alexandro uses Obsidian with the Templater and Dataview plugins. In addition to `wiki/index.md` (the flat, complete catalog), each category folder gets a **MOC page** as its in-graph hub/landing page:

- **Naming**: `MOC - <Category or Entity Name>.md` (e.g. `wiki/clients/MOC - Clientes.md`). One per top-level category folder (`people/`, `clients/`, `projects/`, `meetings/`, `concepts/`) at minimum; a large individual entity (a client or project with many pages) can get its own MOC too once it justifies one.
- **Template**:

  `````markdown
  # MOC - <Name>

  ## Notas desta pasta
  ```dataview
  list from [[<Name>]]
  ```

  ## MOCs relacionadas


  ## Anotações gerais
  `````
- **How it populates**: the Dataview `list from [[<Name>]]` query lists pages that **link to** this MOC — it does not scan the folder automatically. So every wiki page belonging to that category must link to its MOC (e.g. a line like `Ver também: [[MOC - Clientes]]`, or a `moc:` frontmatter field) for it to show up in the list. When creating or updating a page under a category that has a MOC, add that link.
- **Templater placeholder**: the `<% tp.file.title %>` in the template only resolves inside Obsidian via Templater. When *you* (the agent) create a MOC file directly by writing to disk, substitute the real title in both the heading and the `from [[...]]` query — don't leave the raw Templater syntax in a file written outside Obsidian.
- **Relationship to `index.md`**: `index.md` stays the authoritative, complete catalog (every page, one-liner, category, metadata) — keep updating it exactly as before. MOC pages are a complementary Obsidian-native navigation layer, not a replacement.

## Editing discipline

- Alexandro rarely edits the wiki directly — you own it.
- **Never modify files in `raw/`.**
- Always keep `wiki/index.md` and `wiki/log.md` current after any change — this is not optional bookkeeping, it's how future sessions (and future you) find things.
- `wiki/log.md` entries use a consistent, greppable prefix: `## [YYYY-MM-DD] <kind> | <title>` where `<kind>` is `ingest`, `query`, or `lint`.

## Git scope

This root directory also contains a large amount of pre-existing, unrelated personal/business material (e.g. `backup/`, `RH e Financeiro/`, `Banco de Horas/`, etc.) that predates this wiki setup and is **not** part of it. The git repository at this root is deliberately scoped via `.gitignore` to track only `raw/`, `wiki/`, `outputs/`, `TEMPLATES/` (Obsidian Templater templates, e.g. the MOC template), `CEREBRO.md`, `CLAUDE.md`, and `.gitignore` itself. Do not add other root-level folders to git without Alexandro's explicit go-ahead — some of them (e.g. anything under `backup/CREDENCIAIS`) may contain credentials or sensitive personal data and must never be committed. This also applies *inside* tracked folders: `wiki/backup/` is explicitly excluded too (see its own `.gitignore` entry) because it's an unsorted ~18GB dump of installers/videos, not wiki content — check size and content before staging anything unfamiliar that shows up inside `wiki/` or `outputs/`, since files get moved into those trees manually from time to time.

## Open conventions (revisit as needed)

- Client/project subcategorization beyond top-level folders will emerge organically as sources come in — no fixed taxonomy imposed upfront.
- Image-heavy sources (scans, diagrams) don't have a dedicated pipeline yet; handle case-by-case until a real need for OCR/description tooling shows up.
