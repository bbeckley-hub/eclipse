# ECLIPSE

![ECLIPSE banner](images/eclipse_banner.png)

**E. coli Library for Integrated Pathogen Surveillance and Epidemiology**

[![Live site](https://img.shields.io/badge/live-bbeckley--hub.github.io%2Feclipse-7c3aed?style=flat-square)](https://bbeckley-hub.github.io/eclipse)
[![License: MIT](https://img.shields.io/badge/license-MIT-d4af37?style=flat-square)](LICENSE)
[![Part of ARGUS](https://img.shields.io/badge/part%20of-ARGUS-0a1628?style=flat-square)](#the-argus-family)

> An interactive, accession-first genomic resource for *Escherichia coli* surveillance.
> Filter by sequence type, phylogroup, serotype, or gene content — and export a clean
> accession list in seconds. No login. No install. No account.

**🌑 Live site:** <https://bbeckley-hub.github.io/eclipse>

---

## What is ECLIPSE?

ECLIPSE integrates typing, lineage, and epidemiological metadata for publicly
available *E. coli* genomes into a single queryable resource. It exists because
the surveillance data is already public — but almost unusable without weeks of
setup. ECLIPSE closes that gap.

Every page in ECLIPSE ends with the same one-click deliverable:
**a plain-text accession list** ready for

```bash
datasets download genome accession --inputfile list.txt
```

## Quick start

1. **Browse a module** — [MLST](https://bbeckley-hub.github.io/eclipse/pages/mlst.html),
   [Phylogroups](https://bbeckley-hub.github.io/eclipse/pages/phylogroups.html),
   [Serotypes](https://bbeckley-hub.github.io/eclipse/pages/serotypes.html),
   [CHTyper](https://bbeckley-hub.github.io/eclipse/pages/chtyper.html),
   [AMR](https://bbeckley-hub.github.io/eclipse/pages/amr.html),
   [Virulence](https://bbeckley-hub.github.io/eclipse/pages/vfdb.html),
   [Plasmid](https://bbeckley-hub.github.io/eclipse/pages/plasmid.html), or
   the combined [Master Table](https://bbeckley-hub.github.io/eclipse/master.html).
2. **Stack filters** — e.g. ST131 + B2 + blood + Ghana + 2015–2020.
3. **Click 🧬 Accessions** — download a text file with one accession per line.
4. **Download the genomes** — feed the list to the `datasets` CLI.

The site runs entirely in your browser. Works on a phone. Works offline after first load.

## Data & modules

| Module | What it shows |
|---|---|
| **[Master Table](https://bbeckley-hub.github.io/eclipse/master.html)** | All typings side-by-side with metadata and a coverage indicator |
| **[MLST](https://bbeckley-hub.github.io/eclipse/pages/mlst.html)** | Achtman 7-gene sequence types |
| **[Phylogroups](https://bbeckley-hub.github.io/eclipse/pages/phylogroups.html)** | Clermont classification with ecological bins |
| **[Serotypes](https://bbeckley-hub.github.io/eclipse/pages/serotypes.html)** | O:H serotyping (SerotypeFinder) with STEC/ExPEC association classes |
| **[CHTyper](https://bbeckley-hub.github.io/eclipse/pages/chtyper.html)** | fumC + fimH allele typing |
| **[AMR](https://bbeckley-hub.github.io/eclipse/pages/amr.html)** | AMRFinderPlus + point mutations |
| **[Virulence](https://bbeckley-hub.github.io/eclipse/pages/vfdb.html)** | VFDB gene catalogue |
| **[Plasmid](https://bbeckley-hub.github.io/eclipse/pages/plasmid.html)** | PlasmidFinder replicon catalogue |
| **[Statistics](https://bbeckley-hub.github.io/eclipse/statistics.html)** | Interactive dashboard with crosstab explorer |
| **[Geography](https://bbeckley-hub.github.io/eclipse/geography.html)** | Country · source · host · year, with per-genome retrieval |
| **[Lineages](https://bbeckley-hub.github.io/eclipse/lineages.html)** | Literature-curated ST catalogue with live counts |
| **[Downloads](https://bbeckley-hub.github.io/eclipse/downloads.html)** | Every file as gzipped TSV + bulk ZIP |

**Current release (v0.1):** typing annotations for **1,816 pilot genomes**,
epidemiological metadata for **431,592 accessions**, and all raw tables
available for download as gzipped TSVs.

## Methods

Typing annotations are produced by **[EcoliTyper](https://github.com/bbeckley-hub/EcoliTyper)** —
a species-optimized computational pipeline for comprehensive genotyping and
surveillance of *Escherichia coli*. EcoliTyper wraps community-standard tools:

| Analysis | Tool | Reference database |
|---|---|---|
| MLST (Achtman 7-gene) | `mlst` (Seemann) | PubMLST |
| Phylogroup | ClermonTyping | Clermont scheme |
| Serotype (O:H) | SerotypeFinder (CGE, DTU) | CGE reference |
| CHTyper | CHTyper | Roer et al. 2017 |
| AMR genes | AMRFinderPlus | NCBI Reference Gene Catalog |
| Virulence factors | ABRicate | VFDB |
| Plasmid replicons | ABRicate | PlasmidFinder |

Epidemiological metadata (country, isolation source, host, collection year) is
extracted from **NCBI BioSample** records attached to each genome assembly.

### Data sources

- **NCBI Assembly** — genome accessions and assembly-level metadata
- **NCBI BioSample** — country, source, host, year, strain
- **EcoliTyper** — typing annotations generated for this resource

### Deduplication

Where both a GenBank (`GCA_`) and RefSeq (`GCF_`) version of the same assembly
exist, ECLIPSE prefers the RefSeq version. The GCA↔GCF mapping is preserved so
that metadata retrievable by either accession is joined transparently.

## The ARGUS family

ECLIPSE is one resource in **ARGUS** — *Antimicrobial Resistance Genomic
Unified Surveillance* — a family of independent, open genomic surveillance
sites, each dedicated to one of the WHO priority ESKAPE pathogens. Every
resource shares the same architecture: accession-first, metadata-integrated,
no login, no install, MIT-licensed.

Named for **Argus Panoptes** — the hundred-eyed giant of Greek myth who never
slept — the umbrella is a bet that AMR surveillance has to be *unified* to be
useful.

| Resource | Pathogen | Status |
|---|---|---|
| 🦖 **SAURUS** | *Staphylococcus aureus* | [Live](https://bbeckley-hub.github.io/saurus/) |
| 🌑 **ECLIPSE** | *Escherichia coli* | **This repo** |
| 🌊 **ABYSS** | *Acinetobacter baumannii* | Planned |
| 🏆 **KLEOS** | *Klebsiella pneumoniae* | Planned |
| 💧 **PSEUDA** | *Pseudomonas aeruginosa* | Planned |
| 👑 **EMPIRE** | *Enterococcus faecium* | Planned |
| 🌀 **ENTROPY** | *Enterobacter* spp. | Planned |

## Repository structure

```
eclipse/
├── docs/              → GitHub Pages root (the live site)
│   ├── index.html
│   ├── about.html
│   ├── master.html
│   ├── statistics.html
│   ├── geography.html
│   ├── lineages.html
│   ├── downloads.html
│   ├── pages/         → typing + gene module pages
│   ├── data/          → gzipped TSVs + JSON indexes
│   └── assets/        → chart.min.js
├── images/            → banner, logo, screenshots
├── LICENSE            → MIT
└── README.md
```

Only `docs/`, `images/`, `LICENSE`, and this README are tracked. The pipeline
that generates the site (`merged/`, `batches/`, `scripts/`) lives outside the
repo — see the maintainer for access.

## Citation

If you use ECLIPSE in your research, please cite the pipeline that produced
its annotations:

> Beckley B, Amarh V. **EcoliTyper: a species-optimized computational pipeline
> for comprehensive genotyping and surveillance of *Escherichia coli*.**
> *BMC Bioinformatics*. 2026.
> [doi:10.1186/s12859-026-06529-6](https://doi.org/10.1186/s12859-026-06529-6)

Please also cite the specific downstream tools you used — the full list is in
the [EcoliTyper repository](https://github.com/bbeckley-hub/EcoliTyper).

For metadata provenance, cite **NCBI BioSample** and **NCBI Assembly**.

## License

MIT — see [LICENSE](LICENSE). You are free to use, modify, and redistribute.
Attribution appreciated.

## Contact

**Brown Beckley**
Department of Medical Biochemistry, University of Ghana Medical School
✉️ [brownbeckley94@gmail.com](mailto:brownbeckley94@gmail.com)
🐙 [github.com/bbeckley-hub](https://github.com/bbeckley-hub)

---

*If you find ECLIPSE useful, please star the repository — it helps other
researchers find it.*
