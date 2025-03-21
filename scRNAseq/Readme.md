# Single-cell RNA-sequencing analysis for Roome et al., 2025

All analysis was done in R using Seurat v4, and code is written in the following Rmd files.
General notes on each notebook follow each link.

[Part 1: QC and filtering](Roome2025_scrnaseq_part1_QC_filtering.Rmd)
- Cellranger outputs (MEX format) were imported to R (Read10X)
- CreateSeuratObject was run on MEX format data (CreateSeuratObject)
- Biological replicates were combined if applicable (merge)
- Data were inspected and filters for nFeature_RNA and percentage of mitochondrial reads were imposed (subset)
- Basic [Seurat clustering vignette](https://satijalab.org/seurat/articles/pbmc3k_tutorial.html) was run to inspect distribution of cell types in each replicate. If the expected spinal cell types were not present in well differentiated clusters (e.g. in the case of poor emulsion quality resulting in many low-quality cells), then filters were raised.
- Biological replicate Seurat objects were saved as .RDS files.

Part 2: Integration of replicates
