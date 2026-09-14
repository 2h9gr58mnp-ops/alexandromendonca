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
