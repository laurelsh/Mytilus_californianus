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
Jupyter notebooks (`.ipynb`) run in Jupyter. Figure and table numbers below follow the published paper.

| File | Type | What it does | Paper |
|---|---|---|---|
| `01_WGS_diversity.txt` | shell command log | WGS trimming, Bowtie2 mapping, duplicate removal, coverage, cohort SNP calling and filtering, π / Watterson's θ / Tajima's D / Nₑ | Table S1 |
| `02_WGBS_bsseq_prep.txt` | shell + R command log | WGBS trimming, Bismark alignment, deduplication, methylation extraction, λ conversion check, CpG strand merging, FreeBayes SNP masks; builds `bsseq_snp_masked.RData` and the global methylation values | Table S4, Table S5 |
| `03_CpGoe_python.ipynb` | Python | CpG o/e for CDS and promoters; overlaps with WGBS methylation | inputs for Fig S1 and Fig S2 |
| `04_FigS1_CpGoe_vs_methylation.ipynb` | R | CpG o/e vs. mean methylation density plots | **Fig S1** |
| `05_Fig1_metagene_by_expression.ipynb` | R | Metagene methylation profiles stratified by expression; writes `df_profile.rds` | **Fig 1** |
| `06_Fig2_FigS2_methylation_states_expression.ipynb` | R | Four promoter/gene‑body methylation states, expression, partial correlations, threshold and per‑sample sensitivity, CpG o/e vs. expression | **Fig 2**, **Fig S2**, Tables S2–S3 |
| `07_Fig4_FigS3_differential_methylation.ipynb` | R | DSS differential methylation by age class (DMLs), trajectories, genomic enrichment | **Fig 4**, **Fig S3** |
| `08_Fig3_shell_age_calibration.ipynb` | R | GAM shell‑length → age calibration | **Fig 3** |

Some PDF names written by the notebooks predate the final figure order:

| Paper figure | Notebook | File written to `outputs/figures/` |
|---|---|---|
| Fig 1 | 05 | `Fig1.pdf` |
| Fig 2 | 06 | `Fig2.pdf` |
| Fig 3 | 08 | `Fig4.pdf` |
| Fig 4 | 07 | `Fig3_<suffix>.pdf` (panel B also as `Fig3B_heatmap_z_<suffix>.pdf`) |
| Fig S1 | 04 | `FigS1.pdf` |
| Fig S2 | 06 | `FigS3_CpGoe_vs_expression.pdf` |
| Fig S3 | 07 | `FigS2_rawbeta_heatmap_<suffix>.pdf` |

## How to run

`01` and `02` are the upstream terminal steps (Trim Galore, Bowtie2, Bismark, samtools, mosdepth,
BCFtools, FreeBayes, bedtools, VCFtools, and an embedded `R --vanilla` block). They were run on the
lab cluster to produce the input files below; **you do not run them in Jupyter.** Paths in the logs
are placeholders (`<...>`); commands are those of the job scripts run on the server, and versions are those
recorded in the tools' report files or installed in the environments used.
With those inputs in place, run the notebooks in this order:

```
03  →  04  and  05  →  06  →  07          (08 is standalone)
```

- `06` reads `df_profile.rds` (from `05`) and the CpG o/e tables (from `03`), so run `03` and `05` first.
- Each notebook uses **relative paths** and assumes it is launched from the repository root.

## Expected inputs (`data/`)

Produced by the terminal steps (`01`/`02`) or downloaded from the accessions above:

- `bsseq_snp_masked.RData` — SNP‑masked BSseq object, six samples (from `02`, section G)
- `GCF_021869535.1_genomic.gff`, `mytilus_californianus_genome.fasta`, `cds_from_genomic.fna`
- `salmon_quant/quant.sf` — Salmon quantification of the RNA‑seq run
- `combined_covariates.csv` — sample → age‑group table (columns: `sample`, `group`) for `07`
- `wgbs/*_merged.cov_4x.tab.gz` — per‑sample CpG tables (≥ 4 reads) for `03` (from `02`, sections E–F)

Outputs are written to `outputs/figures/` (PDFs) and `outputs/data/` (intermediate RDS/CSV).

## Software

| Step | Tool | Version |
|---|---|---|
| Read trimming (WGS, WGBS) | Trim Galore / Cutadapt | 0.6.10 / 2.6 |
| WGBS alignment, deduplication, extraction | Bismark (Bowtie 2) | 0.24.2 |
| WGS alignment | Bowtie2 | 2.4.1 |
| BAM processing | Samtools | 1.13 |
| Coverage | mosdepth | 0.3.3 |
| Cohort SNP calling (diversity) | BCFtools | 1.21 |
| Per‑sample SNP calling (methylation masks) | FreeBayes | 1.3.2 |
| Interval operations | bedtools | 2.31.1 |
| Diversity statistics | VCFtools | 0.1.17 |
| RNA‑seq quantification | Salmon | 1.9.0 |
| Notebooks 04–05 | R | 4.3.3 |
| Notebooks 06–08 | R | 4.4.1 — `DSS` 2.52.0, `bsseq` 1.40.0, `GenomicRanges` 1.56.2, `GenomicFeatures` 1.56.0, `rtracklayer` 1.64.0, `tximport` 1.32.0, `ppcor` 1.1, `mgcv` 1.9.1 |
| Notebook 03 | Python | 3.11.5 — `pandas` 2.1.4, `numpy` 1.26.3, `pyranges` 0.1.4, `biopython` |

The full session and package versions are printed by the "Reproducibility" cell at the end of each notebook.

## Citation
If you use code or analyses from this repository, please cite the PeerJ paper once published:
Hiebert LS, Soesbe A, Hsieh T-F, Cui Q, Yi SV. Integrated Genomic and Methylome Profiling in the California Mussel: Promoter Methylation Patterns and Age-Linked CpGs. PeerJ (in review).
