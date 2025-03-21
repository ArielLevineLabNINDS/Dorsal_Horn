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

[Part 4: Refine neurons](Roome2025_scrnaseq_part4_refine_neurons.Rmd)
-
- Three iterative clustering and subsetting rounds are done to remove cells which are either low quality or not neurons (noted in code comments).

[Part 5: Split dorsal and late-born neurons from early-born and ventral neurons](Roome2025_scrnaseq_part5_dorsoventral-split_examine_ventral.Rmd)
-
- Dorsal and late-born neurons are extracted from all neurons, with the main criterion being expression of Zic1 among Pax2/Lmx1b neurons. Each portion of the total spinal neurons dataset are saved as separate Seurat objects.
- Early-born and ventral neurons (cardinal groups dI1-6, v0-v3, MN, HB9-interneurons, CSF-contacting neurons, etc) are clustered at 180PCs at resolution 32 and annotated. These annotations were not analyzed in this work, but must be reported as they are the basis for the annotations of the broader cardinal neuron classes. It is possible that these annotations result from over or under-clustering of early-born / ventral neurons, and should not be considered discrete cell types at this stage.
- Due to the high diversity here, occasional groups were subsetted and re-clustered to help refine annotations.
- Annotations were exported to be subsequently added to the metadata of the dataset containing all neurons.

[Part 6: Analysis and annotation of dorsal late-born interneurons (dIL)](Roome2025_scrnaseq_part6_annotation-of-dIL.Rmd)
-
- dIL neurons are clustered using 150 PCs and annotated based on resolution 32 clusters. Six families of dILA and dILB correspond roughly to dorsal horn neurons previously identified, with logically-progressing temporal transcription factor expression profiles. Furthermore, adjacent clusters are annotated with either progressive numbers or progressive letters based on their relationships with each other. For dILB, clusters are largely sequential, varying with TTF expression, and are lettered. For dILA the same relationship exists, but also cluster trajectories which vary based on Zic expression. As Zic expression is a dominating second tier of variation, letters here denote low-to-high Zic expression.
- Each subsequent family is reclustered alone, annotated, and the resulting annotations mapped back to the total spinal neurons object. Top level clusters which are evidently from adjacent families are included when reclustering each family in order to provide context for what features define a useful point of demarcation between each family.
- Cardinal neuron annotations are also remapped onto the total neurons object here as well, with the caveat that they have not been validated.

[Part 7: Final annotations clean-up](Roome2025_scrnaseq_part7_final_annotations_cleanup.Rmd)
- 
- Reappraise motor neuron classifications prior to Xenium label transfer (not analyzed further).
- Some potential DRG clusters and low-quality clusters are removed.
- Some annotations are merged for use with RCTD for annotating Xenium neurons (see appropriate section).
- Metadata classes are defined to produce dendrograms and to define biological replicates.
- A reappraisal of neuron annotations is done. Harmonization of the letter and number subtype nomenclature is done to avoid confusion. Some Zic-variant relationships are reclassified after reappraisal of Zic levels in comparison to closest neighbor clusters.
- PC loadings of dILA, dILB and constituent families are exported for examination - Zic genes are prominent in all dIL families.
- Figure plots and dIL neuron marker lists are exported.
- Export contingency tables for refined neuron types and biological replicates
- End of single-cell RNA-sequencing analysis for Roome et al., 2025.

