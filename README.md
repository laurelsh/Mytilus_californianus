# Mytilus californianus Genomic and Methylome Analyses

This repository contains Jupyter notebooks and scripts used in the analyses for:

Hiebert LS, Soesbe A, Hsieh T-F, Cui Q, Yi SV. *Integrated Genomic and Methylome Profiling Reveals Promoter Repression and Age-Linked CpGs in the California Mussel*. PeerJ (in review).

## Contents
- `01_ANALYSIS_Genomic_diversity_SNPs.ipynb` – Variant calling, nucleotide diversity (π, θ), and Ne estimation.
- `02_ANALYSIS_Prep_of_bsseq_object_and_global_ave_calculation.ipynb` – BSseq object construction, SNP masking, and global methylation.
- `03_ANALYSIS-FIG-1_CpGoe_python.ipynb` – CpG observed/expected analysis (Python version).
- `04_ANALYSIS-FIG-1_CpGoe_R.ipynb` – CpG observed/expected analysis (R version).
- `05_ANALYSIS-FIG-2_Methylation_across_gene_regions.ipynb` – Metagene and promoter methylation profiles.
- `06_ANALYSIS-FIG-3_Expression.ipynb` – Promoter vs gene body methylation and expression integration.
- `07_ANALYSIS-FIG-4_Differential_methylation.ipynb` – DSS analysis of age-associated DMLs.
- `08_ANALYSIS-FIG-S1_Shell_length_calibration.ipynb` – Shell length–age calibration model.

## Data
Raw sequencing reads are available at the NCBI SRA:
- **PRJNA1313349** – Whole-genome resequencing and bisulfite sequencing.
- **PRJNA1011953** – RNA-seq (adductor muscle).

Supplementary tables are provided in the published article (TABLES.xlsx).

## Citation
If you use code or analyses from this repository, please cite the PeerJ paper once published:


