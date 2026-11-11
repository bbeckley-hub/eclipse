# Changelog

All notable changes to ECLIPSE are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Planned
- **AMR module** — antimicrobial resistance genes and point mutations,
  filterable by drug class, mechanism, and gene family
- **Virulence module** — VFDB gene presence/absence with hierarchical
  VFC classification
- **Plasmid module** — replicon typing with per-genome carriage profile
- **Pathotypes** — inferred pathotype assignment (ExPEC, STEC, ETEC, EAEC, EIEC, EPEC)
- **Lineages catalogue** — full ST profile pages with epidemiological context
- **URL state** — encode filters in the query string for shareable views
- **GitHub Actions** — auto-regenerate all pages on data push
- **Full typing coverage** — extend from 37,054 to 431,592 genomes

---

## [0.3.0] — 2026-11-11

First stable release. Full dashboard rebuild of every page.

### Added

#### Landing page
- Dual-nature pitch ("One genome, many lives")
- **Typed** (37,054) and **Indexed** (431,592) shown separately — honest
  coverage signal at 8.6%
- ST131 prevalence as its own stat (3,351 isolates, 9.0%)
- Animated counter reveals for all live numbers
- Featured Geography card with map preview
- Rotating science quotes (18 quotes, 10s rotation)

#### MLST module
- Sequence type filter as type-to-search datalist (was slow dropdown)
- Exact token matching for `ST<n>` in the main search box
- Allele profile expander
- 2,704 unique STs across 37,054 typed genomes

#### Phylogroup module
- **New Ecotype column** — Commensal (A/B1/C) / Non-commensal (B2/D/E/F/G)
- Canonical phylogroup sort order (A → B1 → B2 → C → D → E → F → G)
- Statistical cards showing commensal vs non-commensal breakdown

#### Serotype module
- O-type / H-type / Serotype filters as type-to-search datalists
- Partial serotype rendering (`O157:?`, `?:H7`) — honest over guessing
- O/H columns sort numerically (`O1, O2, O10, O157, …`)

#### CHTyper module
- **Fixed `build_ch_type()`**: partial alleles render as `CH11-?` / `CH?-24`
  (was `CH11-Unknown`)
- Amber pill styling for partial CH types, purple for full
- Numeric sorting across fumC and fimH allele names

#### Master Table
- Coverage column (`n/4`) counting resolved typings per genome
- **Bug fix:** `Unknown:H5` no longer counted as fully serotyped
- BioSample and BioProject columns (hidden by default)
- Removed unused Lineage and Serogroup columns

#### Statistics dashboard
- **ST131 Focus panel** — three donuts (phylogroup, ecotype, serotype) +
  comparison callout vs non-ST131
- **Year × Phylogroup stacked area** — lineage drift over time
- **ST × Phylogroup heatmap** — top 15 STs crossed with 8 phylogroups
- **4-level alluvial** — ST → Phylogroup → Serotype → CH flow diagram
- **Three crosstab heatmaps** — Country × Source, Country × Host,
  Source × Host
- **Combo Explorer** — crosstab any two dimensions live, choose count /
  % row / % column, top-N selection
- Click any slice or bar to filter the whole page

#### Geography
- **New: Leaflet world map** — bubble markers sized by genome count,
  colored by continent, click for details
- **New: Country × Host heatmap**
- **New: Source × Host heatmap**
- **New: Top 20 strains** horizontal bar
- **New: Assembly level** donut (data quality signal)
- **New: Enhanced country table** with top source, top host, year range
  per country
- 146 country centroids for the map (up from 119), covering 146 of 173
  countries (27 skipped — mostly sub-national divisions)
- Graceful fallback message if Leaflet CDN is unavailable

#### Downloads
- **New: `mutation_all.tsv`** — 524,339 AMR point mutations (gyrA, parC,
  rpoB, pmrB, etc.)
- **New: `ecoli_accessions.txt`** — plain-text accession list, one per line,
  ready for `datasets download genome accession --inputfile`
- **Fixed:** `count_rows()` handles `.txt` files correctly (no header to
  subtract)
- Bulk ZIP now includes 13 files
- All files gzip-compressed at level 6 with deterministic mtime

#### About
- Full narrative pitch with **Eclipse metaphor panel** — three eclipses
  happening in E. coli surveillance (commensal face, diversity by ST131,
  single locus by whole genome)
- ARGUS family panel with all seven ESKAPE pathogens
- **Fixed:** SAURUS links to `saurus-db.org` (was github.io subdomain)
- **Fixed:** EzClermont replaces ClermonTyping (nickp60 repo)
- **Fixed:** Nick Waters credited for EzClermont (was Julie Beghain)
- Downstream tools as clickable cards, grouped by workflow stage
- Rotating science quotes (39 quotes, 10s rotation)

#### Landing page (index)
- **Fixed:** ABYSS status shown as `LIVE` (was `PLANNED`)
- **Fixed:** ABYSS links to `bbeckley-hub.github.io/abyss/`
- **Fixed:** ECLIPSE shown as `LIVE` (was `IN PROGRESS`)
- **Fixed:** "Nine interactive views" → "Eight interactive views" (matched
  actual module count)
- **Fixed:** SAURUS links to `saurus-db.org`
- Removed dead `LINEAGE_LOOKUP` code

#### Infrastructure
- **`dedupe_tsvs.py`** — removes 329 duplicate accessions per input TSV
  (GCA↔GCF collapse), preserving last occurrence
- **`build_ecoli_accessions.py`** — generates plain-text accession list from
  typed TSVs; supports `--full` (all metadata accessions) and
  `--prefer-gca` (GCF→GCA translation via `ncbi_dataset.tsv`)
- **`.nojekyll`** — prevents Jekyll from mangling underscore-prefixed paths
- Idempotent generators with explicit `Shell written` confirmation lines
- All patches preserve `.pre-<patch>.bak` backups

### Changed

- **DATA split out of shell HTML** across all typing modules
  - `pages/mlst.html` — shell ~200 KB, data in `data/mlst.json`
  - `pages/serotypes.html` — shell ~200 KB, data in `data/serotype.json`
  - `pages/chtyper.html` — shell ~200 KB, data in `data/chtyper.json`
  - `pages/phylogroups.html` — shell ~200 KB, data in `data/phylogroup.json`
  - `master.html` — shell ~50 KB, data in `data/master.json`
  - `statistics.html` — shell ~60 KB, data in `data/statistics.json`
  - `geography.html` — shell ~200 KB, data split into 4 files:
    - `data/geography.json` — aggregates (~40 KB)
    - `data/geography_genomes.json` — retrieval table (~85 MB, cached)
    - `data/geography_retrieval.js` — retrieval block (~30 KB)
- **Precomputed search blob** per row — O(1) token filtering instead of
  scanning every column per keystroke
- **Year filters** sort numerically ascending (was frequency-descending)
- **Country / Source filters** sort alphabetically
- **ST / Phylogroup / BioProject filters** as datalists — keyboard-first
- **Select-all checkbox** now page-scoped (was global across filtered rows)
- **Export scope label** — "Filtered rows" replaces misleading
  "Visible rows"
- **CH partial allele** — `CH11-?` / `CH?-24` replaces `CH11-Unknown`

### Fixed

- **Empty-state colspan** uses visible column count (was total column count)
- **Datalist filters** no longer silently no-op on partial input
- **`.txt` row counting** — no longer subtracts 1 for a nonexistent header
- **`geography.html` shell size** — was 105 MB when genomes were inlined
  for the retrieval block; now 200 KB with 85 MB of genomes fetched async
- **Missing `retrieval_css` variable** in `generate_geography.py`
- **Double-init of retrieval block** in `generate_geography.py`

### Accessibility

- `aria-sort` on every sortable column
- `aria-label` on all checkboxes (`Select GCA_XXXXXXX.Y`)
- `aria-live="polite"` on row counts and selection counters
- `aria-expanded` + `aria-controls` on column-visibility panel
- `:focus-visible` outlines (3px, accent-colored, 2px offset)
- `prefers-reduced-motion: reduce` — disables all transitions and animations
- `<caption class="sr-only">` on data tables
- `<button>` inside sortable `<th>` for keyboard navigability
- `sr-only` utility class for visually-hidden text

### Verified

Live counts as of 2026-11-11:

| Metric | Value |
|---|---|
| Accessions indexed | 431,592 |
| Typed genomes | 37,054 (8.6%) |
| Countries | 173 |
| Unique sequence types | 2,704 |
| ST131 isolates | 3,351 (9.0% of typed) |
| Phylogroups | 8 |
| Serotypes | ~700 |
| CH types | ~1,500 |
| Source categories | 2,351 |
| Host species | 1,173 |
| Collection years | 1900–2026 |
| GCA↔GCF pairs resolved | 67,886 |

---

## [0.2.0] — 2025-12-01 (internal)

Initial working prototype. Not publicly released.

### Added
- MLST, serotype, CHTyper, phylogroup loaders
- GCA↔GCF translation via `ncbi_dataset.tsv`
- BioSample metadata integration (country, source, host, year)
- Master table merging all four typings
- Basic statistics page (doughnut + bar)
- Basic geography page (country + source bars)
- Eclipse nav bar and branding

---

## [0.1.0] — 2025-10-15 (internal)

Data ingestion pipeline. Not publicly released.

### Added
- `merge_batches.sh` — concatenate typing results from `batches/`
- `eclipse_metadata.tsv` — BioSample metadata extract
- `ncbi_dataset.tsv` — GCA↔GCF pair map
- Initial NCBI assembly fetch (RefSeq GCF preferred)

---

## Versioning policy

ECLIPSE uses [Semantic Versioning](https://semver.org/):

- **Major (`1.0.0`)** — full data coverage (all 431K typed) + stable API,
  citable release
- **Minor (`0.N.0`)** — new modules, significant new features, data updates
- **Patch (`0.N.M`)** — bug fixes, accessibility improvements, corrections

Data updates that don't change schema will be patch releases.
New typing modules or major UI changes will be minor releases.

---

## How to report changes

Found a bug? Have a suggestion?

- **GitHub Issues:** https://github.com/bbeckley-hub/eclipse/issues
- **Email:** brownbeckley94@gmail.com

---

## See also

- [EcoliTyper](https://github.com/bbeckley-hub/EcoliTyper) — the typing pipeline
  that powers ECLIPSE
- [ARGUS family](https://saurus-db.org/) — SAURUS (S. aureus) is the flagship;
  ABYSS (A. baumannii) and others follow the same architecture
- [NCBI Assembly](https://www.ncbi.nlm.nih.gov/datasets/genome/?taxon=562) —
  source of all genomes
- [NCBI BioSample](https://www.ncbi.nlm.nih.gov/biosample/) — source of all
  metadata

---

[Unreleased]: https://github.com/bbeckley-hub/eclipse/compare/v0.3.0...HEAD
[0.3.0]: https://github.com/bbeckley-hub/eclipse/releases/tag/v0.3.0
