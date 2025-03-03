## Learning Objectives

-  Use of RSeQC (a package of scripts) to evaluate the quality of RNA-Seq data

## Recap from last week 

HISAT2 produces multiple output files: 
* `.bam`: unsorted bam file
* `sorted.bam`: sorted bam file
* `.bam.bai`: index for sorted bam file 
* `.log`: log file with containing detailed information about the run. This file is most useful for troubleshooting and debugging. 

## Planning and Organization 

For each experiment you work on and analyze data for, it is considered best practice to get organized by creating a planned storage space (directory structure).

The outputs from HISAT2 were many and looked like this:

```
Irrel_kd_1.subset.bam             Irrel_kd_2.subset_sorted.bam.bai  Mov10_oe_1.subset.bam             Mov10_oe_2.subset_sorted.bam.bai
Irrel_kd_1.subset.log             Irrel_kd_2.subset.txt             Mov10_oe_1.subset.log             Mov10_oe_2.subset.txt
Irrel_kd_1.subset_sorted.bam      Irrel_kd_3.subset.bam             Mov10_oe_1.subset_sorted.bam      Mov10_oe_3.subset.bam
Irrel_kd_1.subset_sorted.bam.bai  Irrel_kd_3.subset.log             Mov10_oe_1.subset_sorted.bam.bai  Mov10_oe_3.subset.log
Irrel_kd_1.subset.txt             Irrel_kd_3.subset_sorted.bam      Mov10_oe_1.subset.txt             Mov10_oe_3.subset_sorted.bam
Irrel_kd_2.subset.bam             Irrel_kd_3.subset_sorted.bam.bai  Mov10_oe_2.subset.bam             Mov10_oe_3.subset_sorted.bam.bai
Irrel_kd_2.subset.log             Irrel_kd_3.subset.txt             Mov10_oe_2.subset.log             Mov10_oe_3.subset.txt
Irrel_kd_2.subset_sorted.bam                                        Mov10_oe_2.subset_sorted.bam
```

These data outputs have been organized into the following folders: 

```
RSeQC_exercise
  ├── bams
  ├── logs
  ├── RSeQC_bed_files
  ├── scripts
  └── sorted_bams
```

+ `logs`: used to keep track of the commands run and the specific parameters used, but also to have a record of any standard output that is generated while running the command.
+ `RSeQC_bed_files`: contain BED12 files created from GTF file 

## RSeQC

Today we will be using RSeQC. RSeQC is a package of scripts designed to evaluate the quality of RNA-seq data. We will run the following: `bam_stat.py`, `infer_experiment.py`, `read_distribution.py`.

The majority of RSeQC scripts generate output files which can be plotted and summarized in the MultiQC report.

### Getting Started 

Last week we installed `RSeQC` using conda. To load RSeQC, we will need to perform:

```bash
conda activate rseqc_env
```
Once activated, your terminal prompt will change from (base) to (rseqc_env), indicating that you are inside the new environment.

To test that everything is in working order we can run:

```bash
infer_experiment.py --help
```

Expected Output: 

```
Usage: infer_experiment.py [options]
Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -i INPUT_FILE, --input-file=INPUT_FILE
                        Input alignment file in SAM or BAM format
  -r REFGENE_BED, --refgene=REFGENE_BED
                        Reference gene model in bed fomat.
  -s SAMPLE_SIZE, --sample-size=SAMPLE_SIZE
                        Number of reads sampled from SAM/BAM file.
                        default=200000
  -q MAP_QUAL, --mapq=MAP_QUAL
                        Minimum mapping quality (phred scaled) for an
                        alignment to be considered as "uniquely mapped".
                        default=30
(rseqc_env) [pdrodrig@vacc-login4 RSeQC_exercise]$ 
```

**Download dataset for today's lesson:**

```bash
/gpfs1/cl/mmg3320/course_materials/RSeQC_exercise
```


### Strand-Specificity 

+ **Purpose:** This script predicts the “strandedness” of the protocol (i.e. unstranded, sense or antisense) that was used to prepare the sample for sequencing by assessing the orientation in which aligned reads overlay gene features in the reference genome. This information is not always available in public datasets. 

+ **Why use it?:**
  + Crucial for differential expression analysis, as strand-specific libraries require correct orientation for accurate read assignment. 
  + Avoids misinterpretation of gene expression levels caused by incorrect strand assignment 

+ **Basic Usage:**

```bash
infer_experiment.py -r ref.bed -i input.bam
```
  + -i input.bam -> specifies the sorted BAM file with aligned RNA-Seq reads 
  + -r ref.bed -> specifies the BED file containing gene annotation (in BED12 format)

+ **Expected Output:**
  + `.infer_experiment.txt`: File containing fraction of reads mapping to given strandedness configurations.

!!! example "Class Exercise #1: `infer_experiment.py`" 

    + Create a directory within RSeQC_exercise called `IKD1_RSeQC` 
    + Move `Irrel_kd_1.subset_sorted.bam` and `Irrel_kd_1.subset_sorted.bam.bai` into `IKD1_RSeQC`
    + Within the `RSeQC_exercise/IKD1_RSeQC` folder run `infer_experiment.py` on `Irrel_kd_1.subset_sorted.bam`. 
    + Use the hg38 bed12 file 
    + **Be sure to redirect the final output to `Irrel_kd_1.subset_infer.log`** You will need this file for our final step.  


### BAM Statistics 

+ **Purpose:** Provides a summary of the alignment results from a BAM file 

+ **Why use it?:** 
  + Quickly checks overall alignment quality
  + Reports metrics such as total reads, mapped reads, properly paired reads (if appropriate), and uniquely mapped reads 
  + Helps identify issues like low mapping rates 

+ **Basic Usage:**

```bash
bam_stat.py -i input.bam
```
  + -i input.bam -> specifies the sorted BAM file with aligned RNA-Seq reads 

+ **Expected Output:**

  + Summary statistics of the BAM file, including:
    + Total number of reads
    + Mapped reads
    + Properly paired reads
    + Uniquely mapped reads
    + Reads mapped to multiple loci

+ **Interpretation:**
  + High mapping rates (~95%) are expected for well-prepared RNA-Seq libraries.
  + A low mapping rate (<80%) could indicate issues with genome annotation or contamination.

!!! example "Class Exercise #2: `bam_stat.py`" 

    + Within the `RSeQC_exercise/IKD1_RSeQC` folder run `bam_stat.py` on `Irrel_kd_1.subset_sorted.bam`. 
    + **Be sure to redirect the final output to `Irrel_kd_1.subset_bamstat.log`** You will need this file for our final step. 

### Splice Junction Detection 

+ **Purpose:** Used to assess the sequencing depth and completeness of splice junction detection in RNA-Seq data. It helps to determine whether an RNA-Seq experiment has sufficient coverage to detect splice junctions reliably. 

+ **Why use it?:** 
  + Aids in determining if additional sequencing would improve splice junction discovery
  + Ensures that the experiment is capturing enough splice junctions for meaningful downstream analysis

+ **Basic Usage:**

```bash
junction_saturation.py -i input.bam -r reference.bed -o output_prefix
```

  + -i input.bam -> specifies the sorted BAM file with aligned RNA-Seq reads 
  + -r reference BED file with gene annotation
  + -o output_prefix -> prefix of output files

+ **Expected Output:**

  + PDF file of Junction Saturation Plot; helps you assess whether sequencing depth is sufficient or if more reads are needed to discover new junctions 
  + The .r (R script) used to generate the PDF file 

+ **Interpretation:**
  + X-axis -> Read depth (number of reads supporting a junction).
  + Y-axis ->  Number of detected splice junctions.
  + Curve shape:
    + If the curve plateaus -> You have sufficient sequencing depth; additional reads won’t improve junction detection.
    + If the curve keeps increasing -> More sequencing might still reveal new junctions.

!!! example "Class Exercise #3: `junction_saturation.py`" 

    + Within the `RSeQC_exercise/IKD1_RSeQC` folder run `junction_saturation.py` on `Irrel_kd_1.subset_sorted.bam`. 
    + Use `IKD1-output` as the output_prefix
    + **Do not generate a .log file for this step**


### Splice Junction Annotation (Class Exercise #4)

+ **Purpose:** Identifies and annotates splice junctions by comparing detected junctions to known junctions in a reference genome annotation (GTF/GFF)

+ **Why use it?:** 
  + Validates detected splice sites against known annotations 
  + Detects novel splice junctions, which may indicate alternative splicing or *sequencing errors*  
  + Identifies unexpected splicing patterns 

+ **Basic Usage:**

```bash
junction_annotation.py -i input.bam -o output -r reference.bed
```

  + -i input.bam -> specifies the sorted BAM file with aligned RNA-Seq reads 
  + -r reference.bed -> specifies the BED file containing gene annotation (in BED12 format)
  + -o output_prefix -> prefix of output files

+ **Expected Output:** 

  + A summary of detected splice junctions, categorized into:
    + Known (found in the reference)
    + Novel (newly identified)
    + Unannotated (potential mapping issues)

+ **Interpretation:**
  + A high fraction of novel junctions may suggest alternative splicing events.
  + Too many unannotated junctions may indicate alignment problems.

!!! example "Class Exercise #4: `junction_annotation.py`"   

    + Within the `RSeQC_exercise/IKD1_RSeQC` folder run `junction_annotation.py` on `Irrel_kd_1.subset_sorted.bam`. 
    + Use `IKD1-output` as the output_prefix
    + **Be sure to redirect the final output to `Irrel_kd_1.subset_janno.log`** You will need this file for our final step. 


### Read Distribution Across Genomic Features (Class Exercise #5)

+ **Purpose:** Analyzes how reads are distributed across different genomic features (e.g., exons, introns, UTRs, intergenic regions).

+ **Why use it?:** 
  + Ensures the expected proportion of reads falls within exons for RNA-Seq experiments

+ **Basic Usage:**

```bash
read_distribution.py -i input.bam -r ref.bed
```

  + -i input.bam -> specifies the sorted BAM file with aligned RNA-Seq reads 
  + -r reference.bed -> specifies the BED file containing gene annotation (in BED12 format)

+ **Expected Output:** 

  + The fraction of reads mapping to different genomic regions, including:
    + Exons
    + Introns
    + Intergenic regions
    + 5' and 3' UTRs

+ **Interpretation:**
  + For RNA-Seq, most reads (>70%) should align to exons.
  + A high proportion of intronic reads (>30%) may indicate rRNA contamination or unprocessed RNA.

!!! example "Class Exercise #5: `read_distribution.py`" 

    + Within the `RSeQC_exercise/IKD1_RSeQC` folder run `read_distribution.py` on `Irrel_kd_1.subset_sorted.bam`. 
    + **Be sure to redirect the final output to `Irrel_kd_1.subset_read.log`** You will need this file for our final step. 


### Final Step: Run MultiQC 

1. **Navigate to top `RSeQC_exercise` folder.** Make sure you are not in any of the subdirectories folders. 

2. Load `py-multiqc/1.15-fmpaaj7`

3. Run multiqc. Be sure to name the output file `RSeQC_stats-multiqc`

4. Take a look at the generated HTML file. 

