## Learning Objectives

By the end of this lesson, students will be able to:

  - Explain why strandedness matters for RNA-Seq quantification
  - Use RSeQC scripts to evaluate RNA-Seq alignment quality
  - Interpret the output of `infer_experiment.py` and `read_distribution.py`
  - Assess whether an RNA-Seq library behaves as expected biologically

## Recap from last week 

HISAT2 produces multiple output files: 

  * `.bam`: unsorted BAM file (output directly from HISAT2)
  * `sorted.bam`: coordinate-sorted BAM file (required for downstream analyses)
  * `.bam.bai`: index for sorted BAM file
  * `.log`: log file containing alignment statistics and run details 

## Planning and Organization 

For each experiment you work on and analyze data for, it is considered best practice to get organized by creating a planned storage space (directory structure). Organizing outputs early prevents downstream confusion when multiple tools generate similar file types.

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

```bash
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

RSeQC is a collection of Python scripts that evaluate whether aligned RNA-Seq reads behave as expected relative to known gene annotations.
We will run the following scripts: `infer_experiment.py`, `read_distribution.py`.

The majority of RSeQC scripts generate output files which can be plotted and summarized in the MultiQC report.

### Getting Started 

  1. Create a working directory called `rseqc`. This will keep all scripts and output files organized in this location. 

  2. Navigate into this directory. 

  3. Create a SLURM script by making a copy of the script shown below and save it `rseqc.sh`. This script will:
      
      + Load the required software module
      + Loop through all BAM files
      + Run RSeQC on each file
      + Summarize results with MultiQC 

4. Make the script executable. Before submitting a script, you must make it executable by carrying out the following command. The `chmod` command changes file permissions in Linux. The `-x` add execute permissions, which allows the operating system to run the file as program rather than treating it as a plain text. 
      
      ```bash
       `chmod +x rseqc.sh
      ``` 

5. Submit the job to the SLURM job submission system.  

```bash
#!/bin/bash
#SBATCH --partition=general
#SBATCH --nodes=1
#SBATCH --ntasks=8
#SBATCH --mem=20G
#SBATCH --time=24:00:00
#SBATCH --job-name=rseqc-loop
#SBATCH --output=run-%x_%j.out

module load apptainer/1.3.4

CONTAINER_PATH="/gpfs1/cl/mmg3320/course_materials/containers/rseqc.sif"
MULTIQC_PATH="/gpfs1/cl/mmg3320/course_materials/containers/multiqc-1.20.sif"

BAM_DIR="/gpfs1/cl/mmg3320/course_materials/RSeQC_exercise/bams"
BED_FILE="/gpfs1/cl/mmg3320/course_materials/RSeQC_exercise/RSeQC_bed_files/refseq.hg38.bed12"
OUTPUT_DIR="rseqc_data"

mkdir -p "$OUTPUT_DIR"

for BAM_FILE in "$BAM_DIR"/*.bam; do
    NAME=$(basename "$BAM_FILE" .bam)

    echo "Processing: $NAME"

    apptainer exec --cleanenv "$CONTAINER_PATH" \
        infer_experiment.py \
        -r "$BED_FILE" \
        -i "$BAM_FILE" \
        > "$OUTPUT_DIR/${NAME}.infer_experiment.txt"
done

# Run MultiQC
apptainer exec --cleanenv "$MULTIQC_PATH" \
    multiqc "$OUTPUT_DIR"
```

**More information about `infer_experiment.py` is provided below.**

## Strand-Specificity (`infer_experiment.py`)

**Purpose:** This script predicts the “strandedness” of the protocol (i.e. unstranded, sense or antisense) that was used to prepare the sample for sequencing by assessing the orientation in which aligned reads overlay gene features in the reference genome. This information is not always available in public datasets. *If strandedness is mis-specified during quantification (e.g., in htseq-counts, featureCounts, or Salmon), reads may be assigned to the wrong genes, especially when genes overlap on opposite strands.*

**Why use it?:**
  
  + Crucial for differential expression analysis, as strand-specific libraries require correct orientation for accurate read assignment. 
  + Avoids misinterpretation of gene expression levels caused by incorrect strand assignment 

**Basic Usage:**

  ```bash
  infer_experiment.py -r ref.bed -i input.bam
  ```

  + -i input.bam -> specifies the sorted BAM file with aligned RNA-Seq reads 
  + -r ref.bed -> specifies the BED file containing gene annotation (in BED12 format)

```bash
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

**Interpretation:**

  - ~0.5 / 0.5 fractions → likely unstranded
  - >0.9 in one orientation → stranded protocol
  - Very low assignment fractions → potential annotation mismatch

## Read Distribution Across Genomic Features (Class Exercise)

**Purpose:** Analyzes how reads are distributed across different genomic features (e.g., exons, introns, UTRs, intergenic regions).

**Why use it?:** 
  
  + Ensures the expected proportion of reads falls within exons for RNA-Seq experiments

**Basic Usage:**

```bash
read_distribution.py -i input.bam -r ref.bed
```

  + -i input.bam -> specifies the sorted BAM file with aligned RNA-Seq reads 
  + -r reference.bed -> specifies the BED file containing gene annotation (in BED12 format)

**Expected Output:** 

  + The fraction of reads mapping to different genomic regions, including:
    + Exons
    + Introns
    + Intergenic regions
    + 5' and 3' UTRs

  + For ribosomal-depleted RNA-Seq libraries (mRNA only) the majority of the reads should be in exons and will have low intronic signal. 

**Interpretation:**
  
  + For RNA-Seq, most reads (>70%) should align to exons.
  + A high proportion of intronic reads (>30%) may indicate rRNA contamination or genomic DNA contamination.

!!! example "Class Exercise: `read_distribution.py`" 

    Modify the SLURM script provided above so that `read_distribution.py` runs for each BAM file. To modify the script, open it with Juptyer Notebooks. This script will take ~5 minutes to run once submitted. Be sure to open the multiQC file to interpret. Turn in the "homework-mini-2" assignment posted on Brightspace. Once you are finished, you are free to go. 

