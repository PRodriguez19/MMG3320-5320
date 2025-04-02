
# DESeq2 dataset 

For today's lesson we will be working with RNA-seq data from a recent publication in Nature Communications by [Shan et al. (2021)](https://doi.org/10.1038/s41467-021-26159-1). Before spring break, our final end product was the creation of counts files using HTSeq-count. 

So what does the count data actually represent? The count data used for differential expression analysis represents the number of sequence reads that originated from a particular gene. The higher the number of counts, the more reads associated with that gene, and the assumption that there was a higher level of expression of that gene in the sample. 

<p align="center">
<img src="../img/deseq_counts_overview.png" width="600">
</p>


Our overall goal is to **identify a list of genes that are statistically distinct between the groups being compared**. 

## The publication
 
 T cell identity is established during thymic development, but how it is maintained in the periphery remains unknown. 
 
 <p align="center">
<img src="../img/tcell_dev.jpg" width="500">
</p>
 
 The authors in this paper discover that by ablating Tcf1 and Lef1 transcription factors in mature CD8+ T cells this induces the expression of genes from non-T cell lineages. We will focus on analyzing the Tcf7 dataset. Specifically, Tcf7 fl/fl mice were crossed with hCD2-Cre mice to create mature CD8+ T cells that lacked *Tcf7*. The protein coded by *Tcf7* is Tcf1. 

<p align="center">
<img src="../img/graphical_abstract.png" width="800">
</p>


### The figures we will create and interpret 

* **PCA & correlation heatmap** to show clusters of samples based on their similarity 
* **Box plots** to show expression (normalized counts) of select genes 
* **Heatmap** to show the expression of the significantly up- and down-regulated genes in the Tcf7 knock-out (KO)  versus the wildtype (WT) 
* **Volcano plots** (essentially a scatter plot) to show the significance (adjusted p-value) and magnitude (log2FoldChange) of expression change between the WT samples versus the Tcf7 KO samples 
* **Pathway analysis** to show the relevant pathways that are associated with up and down-regulated genes. 

# Getting Started 

Copy this folder into your home directory:

```bash 
/gpfs1/cl/mmg3320/course_materials/R_tutorials/ENSG_counts
```
+ Open the file **RNA-Seq_DESeq2_tutorial_EDIT.Rmd** to start. 


## Install the following packages if you need to!  

You will see an image like this if you are missing any packages required to run the .Rmd file 

```
#install.packages(c("knitr", "RColorBrewer", "pheatmap"))
BiocManager::install("apeglm") # Update all/some/none? [a/s/n] - type n

library(knitr)
library(DESeq2) 
library(RColorBrewer)
library(pheatmap)
library(ggplot2)
library(tidyverse)
library(apeglm)
```

Install these later on to generate the final image. These will take a bit longer to install. 

```
library(ggplotify)
library(ggpubr)
library(grid)
library(cowplot)
library(magick) # not sure this works on VACC-OOD
```

## Citation 
Shan, Q., Li, X., Chen, X. et al. Tcf1 and Lef1 provide constant supervision to mature CD8+ T cell identity and function by organizing genomic architecture. Nat Commun 12, 5863 (2021). https://doi.org/10.1038/s41467-021-26159-1

