# Project-CAMILLE-TURADURAND-20220771

## Course Information

- **Course:** ST2GEA – Genomics, Epigenetics and Applications
- **Instructor:** Mano Joseph MATHEW
- **Academic Year:** 2025–2026

## RNA-seq Analysis of GSE68719

### Project Overview

This project presents a complete RNA-seq differential expression analysis workflow performed on the public GEO dataset **GSE68719**. The dataset profiles post-mortem prefrontal cortex (Brodmann Area 9) tissue from Parkinson's Disease patients and neurologically normal controls. The goal is to identify differentially expressed genes (DEGs) between the two groups, assess data quality, and perform functional enrichment analysis to link DEGs to known biological pathways.

### Dataset

| Parameter | Details |
|---|---|
| GEO Accession | GSE68719 |
| Organism | *Homo sapiens* |
| Tissue | Post-mortem BA9 brain cortex |
| Parkinson's Disease patients | 29 |
| Controls | 44 |
| Data type | RNA-seq normalized counts |

**Source publication:**
Dumitriu A, Golji J, Labadorf AT, Gao B, Beach TG, Myers RH, Longo KA, Latourelle JC. *"Integrative analyses of proteomics and RNA transcriptomics implicate mitochondrial processes, protein folding pathways and GWAS loci in Parkinson disease."* BMC Medical Genomics, 2016. DOI: [10.1186/s12920-016-0164-y](https://doi.org/10.1186/s12920-016-0164-y)

- GEO dataset: https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE68719
- PubMed reference: https://pubmed.ncbi.nlm.nih.gov/26793951/

### Biological Objectives

- Identify differentially expressed genes (DEGs) between Parkinson's Disease and control samples
- Assess the quality and structure of the transcriptomic data
- Perform functional enrichment to link DEGs to known biological pathways
- Reproduce and extend findings from the original publication (mitochondrial dysfunction, oxidative stress, protein folding, metallothioneins)

---

## Repository Contents

| File | Description |
|---|---|
| `RNA_seq_Parkinson_GSE68719.Rmd` | Main R Markdown analysis script |
| `GSE68719_mlpd_PCG_DESeq2_norm_counts.txt.gz` | Input count matrix (must be placed in the same directory as the `.Rmd` file) |
| `DEG_results_GSE68719.csv` | Full annotated DESeq2 results table (output) |
| `Significant_DEGs_GSE68719.csv` | Significant DEGs only (padj < 0.05, \|log2FC\| > 1) (output) |
| `figure-html/` | All generated figures (PNG, output) |

---

## Analysis Workflow

The R Markdown document follows these steps:

1. **Load packages** – Installs and loads all required R/Bioconductor packages (`DESeq2`, `tidyverse`, `pheatmap`, `RColorBrewer`, `clusterProfiler`, `enrichplot`, `org.Hs.eg.db`, `AnnotationDbi`, `EnhancedVolcano`, `ggrepel`).
2. **Data import** – Loads the gzip-compressed, GEO-provided normalized count matrix.
3. **Metadata construction** – Automatically builds sample group labels (Control vs PD) from sample name prefixes (`C_` = Control).
4. **Count matrix preparation** – Converts and rounds the normalized counts to integers, as required by DESeq2.
5. **DESeq2 object creation** – Builds the `DESeqDataSet` with design `~ condition`, using "Control" as the reference level.
6. **Filtering low-count genes** – Keeps only genes with ≥10 counts in at least 5 samples.
7. **Differential expression analysis** – Runs the full DESeq2 pipeline (size factor estimation, dispersion estimation, Wald test) for the contrast PD vs Control.
8. **Results annotation** – Cleans ENSEMBL IDs (removes version suffixes) and maps them to gene symbols, full names, and Entrez IDs via `org.Hs.eg.db`.
9. **Significant DEG selection** – Applies thresholds: adjusted p-value < 0.05 and \|log2FoldChange\| > 1.
10. **Variance Stabilizing Transformation (VST)** – Used for all downstream visualization (PCA, heatmaps).
11. **Quality control & exploratory visualizations**:
    - Count distribution before/after normalization
    - PCA plot
    - Sample-to-sample distance heatmap
    - Distribution of shrunken log2 fold changes
    - MA plot
    - Volcano plot
    - Heatmap of top 50 DEGs
    - P-value distribution
12. **Functional enrichment analysis (GO – Biological Process)**:
    - Separate enrichment for upregulated and downregulated genes
    - Dotplots and enrichment network (emap) plots for each
13. **Export of results** – Saves the full and significant DEG tables as CSV files.

---

## How to Run

1. Place `GSE68719_mlpd_PCG_DESeq2_norm_counts.txt.gz` in the same directory as `RNA_seq_Parkinson_GSE68719.Rmd`.
2. Open the `.Rmd` file in RStudio.
3. Knit the document (`Knit to HTML`). All required packages are installed automatically via `BiocManager` if missing.
4. Output figures are saved in `figure-html/`, and result tables are exported as `DEG_results_GSE68719.csv` and `Significant_DEGs_GSE68719.csv`.

### Requirements

- R (≥ 4.x recommended)
- RStudio
- Internet access on first run (for automatic package installation via Bioconductor/CRAN)

---

## Summary of Findings

- Samples largely cluster by disease condition on PCA and the sample distance heatmap, confirming a strong biological signal.
- A substantial number of genes were identified as significantly differentially expressed (padj < 0.05, \|log2FC\| > 1) between PD and control samples.
- Upregulated genes in PD are enriched for: mitochondrial metabolism / oxidative phosphorylation, oxidative stress response, protein folding and quality control, and neurodegenerative signalling — consistent with the original publication.
- Downregulated genes in PD are enriched for: synaptic transmission, neuronal signalling, cytoskeletal organisation, and transcriptional regulation of neuronal identity.
- These results are consistent with Dumitriu et al. (2016), confirming that the transcriptomic remodelling observed in the BA9 prefrontal cortex of Parkinson's Disease patients is robust and reproducible.

## Limitations

- The analysis uses pre-normalized counts from GEO rather than raw FASTQ files, preventing quality control at the alignment/quantification level and potentially introducing biases from the original normalization pipeline.
- No covariates (age, sex, post-mortem interval) were included in the DESeq2 model, due to incomplete metadata in the GSE68719 GEO submission. This could inflate false positives if these variables correlate with disease status.

## Possible Extensions

- Re-analysis from raw FASTQ files for full QC control
- Inclusion of biological covariates in the statistical model
- KEGG pathway analysis and Gene Set Enrichment Analysis (GSEA)
- Integration with epigenomic or proteomic data for multi-omics insights

---

## Author

Camille Tura--Durand - Student ID: 20220771
