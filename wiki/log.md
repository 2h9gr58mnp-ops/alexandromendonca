# Log

Append-only chronological record of every ingest, query, and lint pass. See [CEREBRO.md](../CEREBRO.md) for the workflow this supports.

Entry format: `## [YYYY-MM-DD] <ingest|query|lint> | <title>`

## [2026-09-14] setup | Wiki initialized

Initial scaffold created: `raw/`, `wiki/`, `outputs/` directories, `CLAUDE.md` schema, git repo scoped to wiki-related paths only. No sources ingested yet.

## [2026-09-14] setup | Manual reorg reconciled, sensitive files removed from git tree

`CLAUDE.md` renamed to `CEREBRO.md` (root schema file — links updated accordingly). Several pre-existing root folders were manually filed into the wiki structure: `RH e Financeiro`, `Projetos e Obras`, `Manuais e Documentos` (→ `wiki/clients/`), `INDICADORES OFICINA` (→ `wiki/concepts/`), `monique`, and `backup` (→ `wiki/backup`).

Before linking/committing, found sensitive material sitting inside the git-tracked tree and moved it back out to the (gitignored) root:
- `wiki/backup/CREDENCIAIS/` and `wiki/backup/Senhas do Microsoft Edge.csv` (exported browser passwords) → `backup/`
- `outputs/charts/monique/` (CNH, CNPJ, comprovante de residência — personal ID docs) → `monique/`
- `wiki/RH e Financeiro/` (payroll/salary/termination data) → `RH e Financeiro/`

Also found `wiki/backup/` is ~18GB (software installers, videos, unsorted files) — added `/wiki/backup/` to `.gitignore` per Alexandro's decision: kept on disk for Obsidian browsing, excluded from git.

Linked the remaining filed folders into `wiki/index.md` as unprocessed document catalogs (not yet synthesized into proper entity/project/concept pages): `wiki/clients/Manuais e Documentos/`, `wiki/projects/Projetos e Obras/` (moved here from directly under `wiki/` for consistency), `wiki/concepts/INDICADORES OFICINA/`.

Also noted, not yet acted on: a loose `raw/PROPOSTA LOCAÇÃO ESCAV. ANFIB.pdf` (not filed into a dated subfolder yet) and a small Obsidian `.base` view file at `raw/emails/base nova.base`.

## [2026-09-14] setup | CLAUDE.md pointer + MOC convention adopted

Added a minimal `CLAUDE.md` at root that just points to `CEREBRO.md`, so Claude Code auto-loads the schema again while `CEREBRO.md` stays the real file. Fixed a stray Obsidian embed that had landed inside `CEREBRO.md`'s frontmatter example, and updated its self-references.

Adopted Alexandro's Templater/Dataview MOC (Map of Content) pattern as an official convention — documented under a new "MOC pages" section in `CEREBRO.md`: one `MOC - <Name>.md` page per category folder (and per large entity as needed), populated via Dataview backlinks, complementary to `wiki/index.md`. No MOC pages created yet for existing folders — convention documented for future ingests; existing folders can get theirs on request.

## [2026-09-14] setup | Untangled live folder-drag collisions in wiki/

While preparing a commit, found the wiki tree had been reorganized concurrently (Obsidian folder drags, likely a folder-note plugin) into a nested, non-conventional shape: `wiki/people/concepts/`, `wiki/people/meetings/` (with an 18GB `backup/` re-nested inside it), `wiki/projects/clients/`, plus stray `concepts/` and `projects/` folder-note artifacts at the repo root and `wiki/.obsidian/` (a second vault root). `CLAUDE.md` had also been deleted. Confirmed via `git status`/`git diff --cached` that nothing had actually been committed or corrupted — only the working tree was affected.

Alexandro confirmed to fix the layout to match CEREBRO.md's convention. Actions taken:
- `wiki/people/concepts/INDICADORES OFICINA` → `wiki/concepts/INDICADORES OFICINA`
- `wiki/projects/clients/Manuais e Documentos` → `wiki/clients/Manuais e Documentos`
- `wiki/people/meetings/backup/` (18GB, re-drifted from its excluded location) → merged back into the gitignored root `backup/` via `robocopy /MOVE` (PowerShell's `Move-Item` can't handle some of the very long installer paths inside it). Verified `backup/CREDENCIAIS` and `backup/Senhas do Microsoft Edge.csv` were untouched and not duplicated elsewhere.
- Removed the now-empty leftover shells (`wiki/people/concepts/`, `wiki/people/meetings/`, `wiki/projects/clients/`, each holding only a stray `.gitkeep`) and restored a proper empty `wiki/meetings/` at the top level.
- Recreated the root `CLAUDE.md` pointer file (deleted during the drag episode).
- `wiki/index.md` links already pointed at the correct final paths (no change needed there beyond dropping the now-inapplicable "excluded from git" note about `wiki/backup/`, since backup is back at root, covered by the normal root `.gitignore` rule like before).

Not touched, flagged only: `wiki/people/TESTE DIAS/` (empty folder, likely a test), and the stray root-level `concepts/INDICADORES OFICINA.md` / `projects/Projetos e Obras.md` / `projects/Projetos e Obras 1.md` folder-note files (outside git's tracked scope, harmless, but probably leftover from the same drag episode — Alexandro may want to clean these up in Obsidian directly).

## [2026-09-14] setup | Dataview + Templater installed and verified live

Installed and enabled the Dataview and Templater community plugins on the root Obsidian vault (previously running with community plugins fully off). Set Templater's template folder to `TEMPLATES/` (already holding `TEMPLATES/MOC.md`, saved earlier from Alexandro's pasted template). Verified end-to-end by running "Templater: Create new note from template" → MOC, which correctly substituted `<% tp.file.title %>` and rendered the `dataview` query block (no errors, just an empty result since nothing links to it yet).

Renamed the test note to `MOC - Clientes` and moved it to `wiki/clients/MOC - Clientes.md` as the category's first real MOC hub page, and linked it from `wiki/index.md`. Confirmed working via computer-use control of the Obsidian desktop app (screen-share granted for this task).

## [2026-09-14] setup | MOC hub pages created for all remaining categories

Created the remaining four category MOC pages the same way (Templater template, renamed, dataview query fixed to point at the note's own title, moved into place): `wiki/people/MOC - Pessoas.md`, `wiki/projects/MOC - Projetos.md`, `wiki/meetings/MOC - Reuniões.md`, `wiki/concepts/MOC - Conceitos.md`. All five category folders now have a hub page. Linked all four from `wiki/index.md`. None of them show any results yet since no other notes link back to them — that's expected until real content gets ingested and cross-linked.

## [2026-09-14] setup | Demonstrated MOC backlink mechanism

Created `wiki/projects/projeto-exemplo.md`, a minimal demo page linking back to `[[MOC - Projetos]]`, to show Alexandro how the Dataview `list from [[...]]` query populates. Confirmed in the Obsidian desktop app (found the vault had opened on the second monitor after some vault-switching confusion) that `MOC - Projetos` now lists both `index` and `projeto-exemplo` under "Notas desta pasta". This page is explicitly a demo, not a real project — noted as such in its own body text, safe to delete or replace once the first real project is ingested.

## [2026-09-14] setup | Folder-note pages linking the three raw document dumps to their MOCs

Alexandro asked for the three raw document folders (`Manuais e Documentos`, `Projetos e Obras`, `INDICADORES OFICINA`) to show up linked in their MOCs too. Since they hold binary files (PDF/DOCX/PPTX/JPEG) that can't contain `[[links]]`, created a homonymous "folder note" alongside each (same pattern already used elsewhere in this vault): `wiki/clients/Manuais e Documentos.md`, `wiki/projects/Projetos e Obras.md`, `wiki/concepts/INDICADORES OFICINA.md`. Each briefly describes the folder's contents and links back to its category MOC. Verified in Obsidian that all three now appear under their MOC's "Notas desta pasta" list. Updated `wiki/index.md` accordingly.

While doing this, found the `monique/` folder (CNH, CNPJ, comprovante de residência — personal ID docs) had drifted back into `wiki/clients/` again, presumably during more manual reorganizing in Obsidian. Moved it back out to the gitignored root before touching anything else. Also found `CLAUDE.md` deleted at root yet again — recreated it. Noted but not touched: more folder-note duplicates accumulating at the root (`projects/Projetos e Obras 1/2/3`, `concepts/INDICADORES OFICINA` duplicate) — outside git's tracked scope, but Alexandro may want to clean these up in Obsidian directly at some point.

## [2026-09-14] setup | Individual files linked so the graph shows full traceability

Alexandro asked why the individual documents inside each raw folder didn't show up connected in the Obsidian graph — the three folder notes only *named* the files in prose, which doesn't create a real link, so each file sat as an unconnected orphan with no visible trail. Rewrote all three folder notes (`Manuais e Documentos.md`, `Projetos e Obras.md`, `INDICADORES OFICINA.md`) to `[[link]]` every file individually instead of describing them in text. Confirmed in the Obsidian graph view that each folder note now fans out to its files, and the whole chain (`index` → MOC → folder note → individual file) is visible. Documented this as a standing convention in `CEREBRO.md` (`wiki/` conventions section): always use real `[[wikilinks]]` for referenced files, never plain-text filenames, specifically so nothing gets filed without a visible trail in the graph.
