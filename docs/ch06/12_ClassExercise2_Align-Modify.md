## Class Exercise 3: Modifying the HISAT2 alignment script for PE samples

### Overview 

Everyone will download at least six FASTQ files from the GEO repository for their final project. To ensure that the alignment and downstream processing steps are performed consistently for each sample, it is best to apply the same line of code to all samples simultaneously. To achieve this, we can use loops — a feature of programming languages that allows us to execute a series of commands on multiple files at once.

In addition to saving time, loops make your code more readable and concise. There are various types of loops, but we have focused on the for loop. At the end, you will be "creating" a script that could be used to align paired end samples using HISAT2. 

The final outputs will be in a form of a BAM file. BAM is the binary version of a SAM (Sequence Alignment Map) file. A SAM file is a tab-delimited text file that contains information about each individual read and its alignment to the genome. However, SAM files are not a desirable output due to their large size. Therefore, we will immediately convert them to BAM files using SAMtools.

### For loops

For loops are constructed with 4 basic words: 

| Words |  What it does  |  
|:-----------:|:----------|   
|for | set the loop variable name| 
|in | specify whatever it is we are looping over| 
|do | specify what we want to do with each item | 
|done | tell the computer we are done | 

putting these together, for loop syntax is as follows: 

```bash
for VARIABLE in file1 file2 file3
do
  command1 on $VARIABLE
  command2 
done
```

A basic loop looks something like this when it’s written within a job script:

```bash
for i in A B C
do
  echo $i
done
```

> The VARIABLE could be any letter or word that is meaningful, but is commonly represented by a single letter like "i" for ease. 
> Often the `in` portion will be a directory instead of single files (A, B, C) - i.e. a directory that contains the FASTQ files you want to work with. 


### Variables

The variable portion of a for loop is the item that we are going to iterate over and will change with every “new” loop. **The action is typically being performed on each individual file.** In their simplest form variables can look like this:

```bash
DIRECTORY=PATH/TO/DIRECTORY

DBDIR=/users/p/d/pdrodrig/genome_index
```
> In the above example, this is showing a location to a specific directory 

As a reminder, if we want to make sure this assignment worked, use the `echo` command. This is a command that prints out whatever is provided to it, which can be really useful to test commands or report information back to yourself during a loop.

```bash
echo $DBDIR 
```

### Instructions


Let's start assembling our script. You will be copying and pasting the script in sections. 

    + The purpose of this exercise is the understand how the alignment script was composed. Therefore, take the time to read any of the descriptions provided. 

    + Make a copy of the following folder located here: 

    ```bash
    /gpfs1/cl/mmg3320/course_materials/HISAT2-modify
    ```

    + You will see two FASTQ files inside called `JC1A_R1.fastq.gz` and  `JC1A_R2.fastq.gz`. 
    
    + Open Jupyter Notebooks to "write" your alignment script using the step by step instructions below. Call the script `hisat2-modify-PE.sh`. Keep your terminal open as well. 

1. In lines 1-9, provide the job submission parameters. Copy-and-paste the following section: 

    ```bash
    #!/bin/bash
    #SBATCH --partition=general
    #SBATCH --nodes=1
    #SBATCH --ntasks=2
    #SBATCH --mem=10G
    #SBATCH --time=3:00:00
    #SBATCH --job-name=align_CD8
    # %x=job-name %j=jobid
    #SBATCH --output=%x_%j.out
    ```

2. In lines 11-16, copy-and-paste the next section. The importance of this section is that it will allow you to process all FASTQ files *while* maintaining the file name for each file output.  

    ```bash
    # Iterate through each fastq.gz file in the current directory
    for fastq_file in *fastq.gz; do

        # Extract sample name from the file name
        SAMPLE=$(echo ${fastq_file} | sed "s/.fastq.gz//")
        echo ${SAMPLE}.fastq.gz 
    ```

    **How does this maintain sample naming?**

    + We define the variable `SAMPLE` as the sample name by using the `sed` command to remove `.fastq.gz` from each filename.
        + This allows us to create a variable that effectively stores a list of all sample names.
        + For example, if the filename was `WT-REP1.fastq.gz`:
            + The `sed` command removes `.fastq.gz`, leaving just `WT-REP1`.
            + When we `echo $SAMPLE`, it returns `WT-REP1` instead of the full filename.
    + Using `echo`, we can check that each sample name is correctly extracted.


3. Next, copy-and-paste the following section into lines 18-21. Make sure to use the `tab` key to indent. 

	```bash
    # Set database directory, genome, and processor count
    DBDIR="/gpfs1/cl/mmg3320/course_materials/genome_index/hisat2_index_mm10"
    GENOME="GRCm39"
    p=2
    ```
	
	**All of these are examples of variables. Each variable is assigned a specific value:**

    + `DBDIR` stores a file path to the genome index directory.
    + `GENOME` stores a character string representing the genome version (GRCm39).
    + `p` is set to a numeric value (2), which represents the number of processors to use.

    *Variables help make the script more flexible and easier to modify by centralizing important parameters.*

4. Next, copy-and-paste the following section into lines 23-26. The script will not run unless the required modules are loaded. 

	```bash
    # Load required modules
    module load gcc/13.3.0-xp3epyt
    module load hisat2/2.2.1-x7h4grf
    module load samtools/1.19.2-pfmpoam
    ```

5. Next, copy-and-paste the following section into lines 28-33. 

	```bash
    # Align reads to the reference genome
    hisat2 \
        -p ${p} \
        -x ${DBDIR}/${GENOME} \
        -U ${SAMPLE}.fastq.gz \
        -S ${SAMPLE}.sam &> ${SAMPLE}.log
    ```

6. Move into your terminal tab. Do not close your script, we will return to it shortly. 

7. Run the following command in your terminal. 

	```bash
	hisat2 --help
	```
	
	+ What does `-U` mean? 
	+ What does `-S` mean? 
	+ What does `-x` mean? 

	*Its really important that you take some time to understand what these parameters do!*
	
8. **When you are ready, modify the script you copied-and-pasted to accommodate paired end reads** 
    
    The paired-end files in this directory are: 

    ```
    JC1A_R1.fastq.gz  JC1A_R2.fastq.gz
    ```

    **Hints:**

    + The loop should iterate only over `_1.fastq.gz` files to ensure proper pairing. If the script loops over all `*.fastq.gz` files, it will process both `_1.fastq.gz` and `_2.fastq.gz` files individually.
    + If the `SAMPLE` variable is set to `.fastq.gz` only, the samples will **not** distinguish between `_1.fastq.gz` and `_2.fastq.gz`
    + Use `_1.fastq.gz` for forward reads and `_2.fastq.gz` for reverse reads.

9. Copy-and paste the last section starting in line 36. When you are done, submit the script. 

    ```bash
        # Convert SAM to BAM
        samtools view ${SAMPLE}.sam \
            --threads 2 \
            -b \
            -o ${SAMPLE}.bam

        # Remove SAM file
        rm ${SAMPLE}.sam

        # Generate alignment statistics
        samtools flagstat ${SAMPLE}.bam > ${SAMPLE}.txt

        # Sort the BAM file by coordinates
        samtools sort ${SAMPLE}.bam -o ${SAMPLE}_sorted.bam

        # Index the sorted BAM file
        samtools index ${SAMPLE}_sorted.bam

        echo "Sample ${SAMPLE} processing complete."
    done
    ```

    The final files output should include the following: 

    ```bash
    align_CD8_60836.out  JC1A.log          JC1A_R2.fastq.gz  JC1A_sorted.bam.bai  
    JC1A.bam             JC1A_R1.fastq.gz  JC1A_sorted.bam   JC1A.txt
    ```

    We will review these outputs during next class. 