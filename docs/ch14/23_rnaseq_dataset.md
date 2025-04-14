
# Visualizing RNA-Seq data

For today's lesson we will be working with RNA-seq data from the publication in ImmunoHorizons by [Sabikunnahar et al. (2025)](https://academic.oup.com/immunohorizons/article/9/5/vlaf007/8096490?login=true).

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

# Getting Started: Monday, April 14th 

Copy this folder into your home directory:

```bash 
/gpfs1/cl/mmg3320/course_materials/R_tutorials/ENSM_partIV
```
  
  + Open the file **RNASeq_Visualizations_PartIV_EDIT.Rmd** to start. 

