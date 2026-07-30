# Analysis code — Integrated genomic and methylome profiling in the California mussel (*Mytilus californianus*)

This repository contains the analysis code for the study of genomic diversity, DNA
methylation, and age‑associated methylation in *Mytilus californianus*. It reproduces
the main and supplementary figures from the paired whole‑genome resequencing (WGS),
whole‑genome bisulfite sequencing (WGBS), and public adductor‑muscle RNA‑seq data.

**Raw data**
- WGS + WGBS: NCBI BioProject **PRJNA1313349**
- RNA‑seq (public, unmatched): NCBI SRA **PRJNA1011953** (run SRR29117084)
- Reference genome: **GCF_021869535.1** (Paggeot et al., 2022)

## File overview

Two of the files are **command logs of terminal / cluster steps** (`.txt`). The remaining files are
Jupyter notebooks (`.ipynb`) run in Jupyter.

| File | Type | What it does | Figure |
|---|---|---|---|
| `01_WGS_diversity.txt` | shell command log | Read trimming, mapping, SNP calling, π/θ/Nₑ diversity | Results (diversity) |
| `02_WGBS_bsseq_prep.txt` | shell + R command log | Bismark processing, SNP‑mask construction, builds `bsseq_snp_masked.RData` | Methylation landscape |
| `03_CpGoe_python.ipynb` | Python | Computes CpG o/e for CDS + promoters; overlaps with WGBS methylation | inputs for Fig S1 |
| `04_FigS1_CpGoe_vs_methylation.ipynb` | R | CpG o/e vs. mean methylation density plots | **Fig S1** |
| `05_Fig1_metagene_by_expression.ipynb` | R | Metagene methylation profiles stratified by expression; writes `df_profile.rds` | **Fig 1** |
| `06_Fig2_methylation_states_expression.ipynb` | R | Four promoter/gene‑body methylation states + expression + partial correlations | **Fig 2** |
| `07_Fig3_FigS2_differential_methylation.ipynb` | R | DSS differential methylation by age class (DMLs), trajectories, enrichment | **Fig 3** + **Fig S2** |
| `08_Fig4_shell_age_calibration.ipynb` | R | GAM shell‑length → age calibration | **Fig 4** |

## How to run

`01` and `02` are the upstream terminal steps (Bismark, FreeBayes/bcftools, bedtools, and
an embedded `R --vanilla` block). They were run on the cluster to produce the input files
below; **you do not run them in Jupyter.** With those inputs in place, run the notebooks in
this order:

```
03  →  04  and  05  →  06  →  07          (08 is standalone)
```

- `06` reads `df_profile.rds`, so `05` must run before `06`.
- Each notebook uses **relative paths** and assumes it is launched from the repository root.

## Expected inputs (`data/`)

Produced by the terminal steps (`01`/`02`) or downloaded from the accessions above:

- `bsseq_snp_masked.RData` — SNP‑masked BSseq object (from `02`)
- `GCF_021869535.1_genomic.gff`, `mytilus_californianus_genome.fasta`, `cds_from_genomic.fna`
- `salmon_quant/quant.sf` — Salmon quantification of the RNA‑seq run
- `combined_covariates.csv` — sample → age‑group table (columns: `sample`, `group`) for `07`
- WGBS per‑sample CpG tables under `data/wgbs/` for `03`

Outputs are written to `outputs/figures/` (PDFs) and `outputs/data/` (intermediate RDS/CSV).

## Software

- **R** ≥ 4.3 — key packages: `bsseq`, `DSS`, `GenomicRanges`, `GenomicFeatures`,
  `rtracklayer`, `tidyverse`, `tximport`, `ppcor`, `patchwork`, `pheatmap`, `mgcv`, `Cairo`
- **Python** 3.11 — `pandas`, `numpy`, `biopython`, `pyranges`

Exact package versions are printed by the "Reproducibility" cell at the end of each notebook.

## Notes

- Differential methylation (`07`) uses **FDR ≤ 0.05** throughout (DSS `DMLtest.multiFactor`,
  no smoothing); z‑scoring in the heatmaps is for visualization only.
- The proximal‑promoter window for methylation–expression analyses (`06`) is **150 bp**
  upstream of the TSS; the CpG o/e analyses (`03`/`04`) use a wider window appropriate for
  a sequence‑composition metric.
- `_originals_backup/` holds the pre‑cleanup versions of these files and is not part of the
  analysis pipeline.

 ## Citation
If you use code or analyses from this repository, please cite the PeerJ paper once published:
Hiebert LS, Soesbe A, Hsieh T-F, Cui Q, Yi SV. Integrated Genomic and Methylome Profiling in the California Mussel: Promoter Methylation Patterns and Age-Linked CpGs. PeerJ (in review).
