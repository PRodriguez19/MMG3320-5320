
# Visualizing RNA-Seq data

For today's lesson we will be working with RNA-seq data from another recent publication in ImmunoHorizons by [Sabikunnahar et al. (2025)](https://academic.oup.com/immunohorizons/article/9/5/vlaf007/8096490?login=true).

<figure markdown="span">
  ![sab_article](../img/sab_et.al.2025.png){ width="600" }
</figure>

**Summary:** Innate immune cells rapidly respond to microbial threats, but if unchecked, these responses can harm the host, as seen in septic shock. To explore how genetic variation influences these responses, researchers compared standard lab mice (C57BL/6) with genetically diverse wild-derived mice (PWD). 

**Experimental Design:** The authors isolated bone marrow derived dendritic cells from B6, c11.2, and PWD mice and then either left them unstimulated or stimulated with LPS. 

This experiment has:
  
  + Two treatments: `untreated`, `LPS`
  + Three genotypes: `B6`, `c11.2`, and `PWD` 
  
The goal:
  
  + To test how LPS stimulation affects each genotype 
  + Whether the response to LPS differs between genotype 

# Getting Started: Monday, April 7th 

Copy this folder into your home directory:

```bash 
/gpfs1/cl/mmg3320/course_materials/R_tutorials/ENSM_counts
```
  
  + Open the file **RNA-Seq_DESeq2_PartII_EDIT.Rmd** to start. 

### The figures we will create will be: 

  + Figure 2, panel H: LPS treated (PWD vs B6) Volcano Plot
  + Figure 5, panel B: PCA of the three strains used in this experiment

# Getting Started: Wednesday, April 9th 

Copy this folder into your home directory:

```bash
/gpfs1/cl/mmg3320/course_materials/R_tutorials/ENSM_partIII
```

  + Open the file **RNA-Seq_Visualization_PartIII_EDIT.Rmd** to start. 
  + We will need to check if `dds.RDS` is usable. 

### The figures we will create will be: 

  + Figure 3, boxplot representation for IL6 and Nos2 expression 
  + Heatmap of all +5,000 DE genes (PWD_LPS vs B6_LPS)
