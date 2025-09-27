# Mytilus californianus Genomic and Methylome Analyses

This repository contains Jupyter notebooks and scripts used in the analyses for:

Hiebert LS, Soesbe A, Hsieh T-F, Cui Q, Yi SV. *Integrated Genomic and Methylome Profiling Reveals Promoter Repression and Age-Linked CpGs in the California Mussel*. PeerJ (in review).

## Contents
- `01_BSseq_object_creation.ipynb` – Construction of SNP-masked BSseq objects from WGBS data.
- `02_Global_methylation_profiles.ipynb` – Calculation of genome-wide mCG and CpG o/e correlations.
- `03_Expression_integration.ipynb` – Integration of WGBS methylation with Salmon-derived RNA-seq expression.
- `04_Partial_correlation_analysis.ipynb` – Promoter vs gene body methylation and expression.
- `05_Metagene_profiles.ipynb` – Gene body/promoter methylation profiles across expression strata.
- `06_DML_analysis.ipynb` – Age-class differential methylation testing (DSS).
- `07_DML_annotation_visualization.ipynb` – Annotation of DMLs by genomic region and figure generation.

## Data
Raw sequencing reads are available at the NCBI SRA:
- **PRJNA1313349** – Whole-genome resequencing and bisulfite sequencing.
- **PRJNA1011953** – RNA-seq (adductor muscle).

Supplementary tables are provided in the published article (TABLES.xlsx).

## Citation
If you use code or analyses from this repository, please cite the PeerJ paper once published:


