# phagosome-db

A community resource for exploring and cross-referencing phagosome proteomics data, standardized through uniform reprocessing of raw mass spectrometry files.

Modeled after the [Lipid Droplet Knowledge Portal](https://lipiddroplet.org).

> **Status: early development.** This repository currently holds a curated catalog of candidate datasets. Reprocessing and the web interface are in progress. See [Roadmap](#roadmap).

---

## Motivation

Phagosome proteomics studies span more than a decade of instrumentation, search engines, and UniProt releases. Comparing results directly across published supplementary tables is unreliable: gene names drift between UniProt versions, protein grouping logic differs between search engines (MaxQuant, Mascot, Proteome Discoverer), and FDR thresholds are applied inconsistently.

**phagosomeDB addresses this by reprocessing every included dataset from raw files**, using a single pinned MaxQuant version and a single pinned UniProt database release. This ensures that protein identifiers, gene names, and protein grouping are directly comparable across every dataset in the resource.

## Inclusion Criteria

A dataset is included only if:

1. The original `.raw` (or vendor-equivalent) files are deposited in **PRIDE** or **MassIVE**, and
2. The deposit includes sufficient metadata to reconstruct the experimental design (species, cell line, phagosome cargo, treatment, timepoint).

Datasets with only processed/summary tables (no raw files) are not included, regardless of how central they are to the field. This is a deliberate tradeoff in favor of cross-study consistency over completeness.

## Repository Structure

```
phagosomeDB/
├── README.md
├── LICENSE
├── data/
│   └── phagosome_datasets.csv     # Curated catalog of candidate/included datasets
├── docs/                          # Methodology notes, MaxQuant parameter files
└── scripts/
    └── maxquant/                  # Reprocessing scripts and pinned parameter files
```

The web interface (backend + frontend) will live in this same repository once development begins — see [Roadmap](#roadmap).

## Data Dictionary

`data/phagosome_datasets.csv` tracks every dataset considered for inclusion.

| Column | Description |
|---|---|
| `Title` | Publication title |
| `DOI` | Digital Object Identifier of the source publication |
| `PMID` | PubMed ID |
| `Journal` | Journal of publication |
| `Year` | Publication year |
| `First Author` | First author |
| `Last Author` | Corresponding/senior author |
| `Species` | Organism studied (controlled vocabulary, e.g. `Homo sapiens`, `Mus musculus`) |
| `Cell Line` | Cell line or primary cell type used |
| `Phagosome Cargo` | Material internalized (e.g. latex beads, zymosan, bacteria, apoptotic cells) |
| `Quantitation` | LFQ, SILAC, mTRAQ, TMT |
| `Mass Spectrometer` | Instrument used for original acquisition |
| `Search Software` | Software used in the original publication (for reference; all datasets are reprocessed with MaxQuant regardless) |
| `PTMs` | Post-translational modifications considered in the original study |
| `PRIDE ID` | PRIDE repository accession, if applicable |
| `MassIVE ID` | MassIVE repository accession, if applicable |
| `Annotation` | Free-text curation notes |
| `Included in Phagosome-DB?` | Y or N for inclusion |

Additional columns (quantitation type, raw file availability, reprocessing status) are being incorporated as the catalog matures — see Roadmap.

## Methodology

All included datasets are reprocessed using:

- **Search engine:** MaxQuant (version pinned — see `scripts/maxquant/`)
- **Reference database:** A single pinned UniProt release, plus the standard MaxQuant contaminants list
- **Fixed settings across all datasets:** enzyme specificity, fixed/variable modifications, FDR thresholds (see `docs/` for full parameter documentation)
- **Adapted per dataset:** quantitation strategy (label-free, SILAC, TMT), as dictated by the original experimental design

Because quantitation strategies differ across studies, **quantitation values are not directly comparable between datasets** even after reprocessing. The portal treats quantitation as dataset-specific; cross-study comparison is intended at the level of protein identity and presence/absence, not raw intensity values.

## Roadmap

- [x] Define dataset inclusion criteria
- [x] Establish catalog schema
- [ ] Complete PRIDE/MassIVE audit of candidate datasets
- [ ] Define controlled vocabulary for Species, Phagosome Cargo, Search Software
- [ ] Pin MaxQuant version and UniProt release
- [ ] Reprocess all qualifying datasets
- [ ] Build backend (PostgreSQL + FastAPI)
- [ ] Build frontend (React + TanStack Table)
- [ ] Deploy public web tool
- [ ] Register domain

## Contributing

Suggestions for datasets to include, corrections to existing entries, or general feedback are welcome via [Issues](../../issues). If you know of a phagosome proteomics dataset with raw files deposited in PRIDE or MassIVE that isn't yet in the catalog, please open an issue or submit a pull request adding a row to `data/phagosome_datasets.csv`.

## Citation

A citation format (and DOI, via Zenodo) will be added once the resource reaches a stable release. For now, please reference this repository directly if you make use of the catalog.

## License

Code is released under the [MIT License](LICENSE). The curated dataset catalog is intended for open reuse; a data-specific license (e.g. CC-BY 4.0) will be specified explicitly as the catalog matures.

## Maintainer

Ben Allsup - allsup@alum.mit.edu
