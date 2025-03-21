# Single-cell RNA-sequencing analysis for Roome et al., 2025

All analysis was done in R using Seurat v4, and code is written in the following Rmd files.
General notes on each notebook follow each link.

Prior to these steps, 30 replicate 10X Chromium 3' GEX runs from cervical, thoracic or lumbar E14.5/E16.5 spinal cords were run through Cellranger mkfastq and count, with 10X Genomics' reference genome package 2020-A (mm10).

[Part 1: QC and filtering](Roome2025_scrnaseq_part1_QC_filtering.Rmd)
- 
- Cellranger outputs (MEX format) were imported to R (Read10X)
- CreateSeuratObject was run on MEX format data (CreateSeuratObject)
- Biological replicates were combined if applicable (merge)
- Data were inspected and filters for nFeature_RNA and percentage of mitochondrial reads were imposed (subset)
- Basic [Seurat clustering vignette](https://satijalab.org/seurat/articles/pbmc3k_tutorial.html) was run to inspect distribution of cell types in each replicate. If the expected spinal cell types were not present in well differentiated clusters (e.g. in the case of poor emulsion quality resulting in many low-quality cells), then filters were raised.
- Biological replicate Seurat objects were saved as .RDS files.

[Part 2: Integration of replicates](Roome2025_scrnaseq_part2_Integrate-replicates.Rmd)
- 
- R script was run in Bash on a local hpc cluster (bash script is commented)
- Biological replicate Seurat objects were loaded
- CCA Integration (Seurat v4) was run on a list of all biological replicate Seurat objects.
- Data was scaled while regressing nCount_RNA and nFeature_RNA for the integrated Seurat object (ScaleData)
- PCA was run using 90 PCs on the integrated Seurat object (RunPCA)
- Integrated Seurat object was saved ('Cervthorlumb_all.RDS')

[Part 3: Extract neurons from all spinal cells](Roome2025_scrnaseq_part3_extract-neurons.Rmd)
- 
- Integrated Seurat object loaded, and metadata is appended.
- Cells are clustered with 90PCs at resolution 2. Cluster markers were run separately on hpc cluster (commented code)
- Cluster identities are examined using FeaturePlot for top DEGs of each cluster.
- Main spinal cord cell types assigned to $maintypes metadata category from resolution 2 clusters. Cluster markers are rerun using $maintypes on hpc cluster. (commented code)
- Visualizations (VlnPlot, DotPlots) are exported to report processing.
- Clusters identified as spinal cord neurons are subsetted from all neurons ('Cervthorlumb_neurons') on hpc cluster, data scaling and PCA (90) are run, and saved (commented code).
