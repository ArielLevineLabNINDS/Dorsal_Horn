# Pixel-count histograms

Processing of pixel-count histograms exported from Fiji (using outputs of Plot Profile) are described in the [R markdown](Pixel_count_histograms.Rmd). The pipeline is paraphrased below:

- Part 1: Histograms are normalized to 80 bins, metadata is added, and data is collated in a database.
- Part 2: Measurement replicates are averaged across sections/hemisections for each embryo, and a revised database is produced.
- Part 3: Genotype metadata is extracted from filename and appended to the database.
- Part 4: Data is exported by marker set and genotype for Ptf1a WT and KO (4.1), Gsx1/2 WT and KO (4.2) and Pax3:Cre / Isl2:DTA (control or sensory neuron-ablation) (4.3).
