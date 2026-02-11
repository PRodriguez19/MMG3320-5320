
## Learning Objectives

* Learn to clean FASTQ reads using Trimmomatic if required

## Introduction 

During the last class, we took a high-level look at the quality of each of our samples using FastQC. We visualized per-base quality graphs showing the distribution of read quality at each base across all reads in a sample and extracted information about which samples fail which quality checks. Some of our samples failed quite a few quality metrics used by FastQC. This does not mean, though, that our samples should be thrown out! It is very common to have some quality metrics fail, and this may or may not be a problem for your downstream application. 

Now, we want to make sure that as many reads as possible map or align accurately to the genome. To ensure accuracy, only a small number of mismatches between the read sequence and the genome sequence are allowed, and any read with more than a few mismatches will be marked as being unaligned.

Therefore, to make sure that all the reads in the dataset have a chance to map/align to the genome, unwanted information can be trimmed off from every read, one read at a time. The types of unwanted information can include one or more of the following:

  * leftover adapter sequences
  * known contaminants (strings of As/Ts, other sequences)
  * poor quality bases 

## Where does this "unwanted information" come from? 

* During the sequencing process, adapter sequences are added to the ends of the reads. These sequences are used to attach the RNA molecules to the sequencing platform but they do not map to regions of the genome. These adapter sequences can interfere with downstream analysis and lead to inaccurate results. 

* RNA-seq data may contain low-quality reads that are caused by sequencing errors, PCR bias, or other factors such as poor sample quality. By removing these low-quality reads researcher can ensure that the data is of high quality. 

## Steps of trimming data 

**Step 1: Quality Control**

The first step in trimming RNA-seq data is to assess the quality of the raw reads. This can be done using software such as FastQC, which generates a report that includes information on read length, GC content, base quality, and sequencing adapter contamination. If the data is of poor quality, it may need to be re-sequenced or excluded from further analysis.

**Step 2: Adapter Trimming**
The next step is to remove adapter sequences from the reads. These sequences are added during library preparation and can interfere with downstream analysis. Many tools (some listed below) can be used to trim adapters from the reads by searching for the adapter sequence and removing it.

**Step 2b: Quality Trimming**
During adapter trimming, we could also remove low-quality reads by setting a minimum quality threshold and removing any bases that fall below this threshold. Trimming tools often have built-in quality trimming options that can be used for this purpose.

**Step 2c: Short read filtering**

Reads that are too short *after trimming* can also be removed. This step is important to ensure that only high-quality reads are used for downstream analysis. Trimming tools also have this built-in option as well. 

## Tools for Trimming

There are a number of tools that can be used for read trimming, some include:

* [Cutadapt](https://cutadapt.readthedocs.io/en/stable/)
* [Trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic)
* [fastp](https://github.com/OpenGene/fastp)
* [Trim Galore](https://github.com/FelixKrueger/TrimGalore/tree/master) 

They have a varying range of clipping and trimming features, but for the most part they all work similarly. 

## Trimming is not always required 

* There are some aligners that are available which will "soft-clip" low-quality bases or adapter sequences *during* alignment. If you are working with short reads (<50 bp), trimming can actually prevent the aligner from discarding poor-quality reads. 

*We will compare some aligners next week* 

In the class exercises below, we will be using Trimmomatic and Trim Galore. 

# Option 1: Trimmomatic

```bash
module load gcc/13.3.0-xp3epyt
```

Search for the module with: 

```bash
module avail
```

Use the following to check that the program was loaded

```bash
module list
```

Trimmomatic has a variety of options to trim your reads. If we run the following command, we can see some of our options.

```bash
trimmomatic
```

```bash
Usage: 
       PE [-version] [-threads <threads>] [-phred33|-phred64] [-trimlog <trimLogFile>] [-summary <statsSummaryFile>] [-quiet] [-validatePairs] [-basein <inputBase> | <inputFile1> <inputFile2>] [-baseout <outputBase> | <outputFile1P> <outputFile1U> <outputFile2P> <outputFile2U>] <trimmer1>...
   or: 
       SE [-version] [-threads <threads>] [-phred33|-phred64] [-trimlog <trimLogFile>] [-summary <statsSummaryFile>] [-quiet] <inputFile> <outputFile> <trimmer1>...
```

This output shows us that we must first specify whether we have paired end (PE) or single end (SE) reads. Next, we specify what flag we would like to run. For example, you can specify threads to indicate the number of processors on your computer that you want Trimmomatic to use. In most cases using multiple threads (processors) can help to run the trimming faster. These flags are not necessary, but they can give you more control over the command. The flags are followed by positional arguments, meaning the order in which you specify them is important. In paired end mode, Trimmomatic expects the two input files, and then the names of the output files. These files are described below. While, in single end mode, Trimmomatic will expect 1 file as input, after which you can enter the optional settings and lastly the name of the output file.

| option       | meaning                                                                                   |
|:------------ | :---------------------------------------------------------------------------------------- |
| inputFile1 | Input reads to be trimmed. Typically the file name will contain an _1 or _R1 in the name. |
| inputFile2 | Input reads to be trimmed. Typically the file name will contain an _2 or _R2 in the name. |
| outputFile1P | Output file that contains surviving pairs from the _1 file. |
| outputFile1U | Output file that contains orphaned reads from the _1 file. |
| outputFile2P | Output file that contains surviving pairs from the _2 file. |
| outputFile2U | Output file that contains orphaned reads from the _2 file. |

In addition, trimmomatic expects to see is the trimming parameters:

| step         | meaning                  |
|:------------ |:------------------------ |
| ILLUMINACLIP | Perform adapter removal. |
| SLIDINGWINDOW | Perform sliding window trimming, cutting once the average quality within the window falls below a threshold. |
| LEADING | Cut bases off the start of a read, if below a threshold quality. |
| TRAILING | Cut bases off the end of a read, if below a threshold quality. |
| CROP | Cut the read to a specified length. |
| HEADCROP | Cut the specified number of bases from the start of the read. |
| MINLEN | Drop an entire read if it is below a specified length. |
| TOPHRED33 | Convert quality scores to Phred-33. |
| TOPHRED64 | Convert quality scores to Phred-64. |

We will use only a few of these options and trimming steps in our analysis. It is important to understand the steps you are using to clean your data. For more information about the Trimmomatic arguments and options, see the Trimmomatic manual.

However, a complete command for Trimmomatic will look something like the command below. This command is an example and will not work, as we do not have the files it refers to:

```bash
trimmomatic PE SRR_1056_1.fastq SRR_1056_2.fastq  \
        SRR_1056_1.trimmed.fastq SRR_1056_1un.trimmed.fastq \
        SRR_1056_2.trimmed.fastq SRR_1056_2un.trimmed.fastq \
        SLIDINGWINDOW:4:20 ILLUMINACLIP:SRR_adapters.fa 
```

In this example, we have told Trimmomatic:

|code | meaning                                          |
|:---|:------------------------------------------------- |
| PE | that it will be taking a paired end file as input |
| SRR_1056_1.fastq | the first input file name |
| SRR_1056_2.fastq | the second input file name |
| SRR_1056_1.trimmed.fastq | the output file for surviving pairs from the _1 file |
| SRR_1056_1un.trimmed.fastq | the output file for orphaned reads from the _1 file |
| SRR_1056_2.trimmed.fastq | the output file for surviving pairs from the _2 file | 
| SRR_1056_2un.trimmed.fastq | the output file for orphaned reads from the _2 file |
| SLIDINGWINDOW:4:20 | to use a sliding window of size 4 that will remove bases if their phred score is below 20 |  
| ILLUMINACLIP:SRR_adapters.fa | to clip the Illumina adapters from the input file using the adapter sequences listed in SRR_adapters.fa | 

!!! note
    Some of the commands we ran in this lesson are long! When typing a long command into your terminal, you can use the \ character to separate code chunks onto separate lines. This can make your code more readable.
    
## Running Trimmomatic

!!! example "Class Exercise #1" 

    **Estimated Time:** ~20 minutes
    
    **Collaboration:** Please work with your neighbor if you have questions. I will begin answering questions at the 5 minute mark. 

    **Learning goal:** By the end of this exercise, you should be able to:
        
        * Understand what each Trimmmomatic argument does 
        * Successfully trim FASTQ files
        * Verify trimming success using FastQC results

    **Part A:** Make a copy of the exercise materials into your current working directory. The folder can be found in the location below. Confirm that the folder is copied into your VACC working directory before continuing. 
    
      ```bash
      /gpfs1/cl/mmg3320/course_materials/trimmomatic_exercise
      ```
    
    **Part B:** Modify the `trim.sh` script using Jupyter Notebook, Nano, or your preferred text editor. Your goal is to run `trimmomatic` on the provided FASTQ files. 

    **Important reminder:** Trimmomatic arguments must be provided in the correct order starting with input files, output files, and remaining arguments. 

    | argument to add    | meaning |
    |:------------------ | :------------------------------------------------ |
    | SE or PE           | single end or paired end file as input |
    | outputFile1P       | Output file that contains surviving pairs from the _1 file. |
    | outputFile1U       | Output file that contains orphaned reads from the _1 file. |
    | outputFile2P       | Output file that contains surviving pairs from the _2 file. |
    | outputFile2U       | Output file that contains orphaned reads from the _2 file. |
    | SLIDINGWINDOW:4:20 | Uses a sliding window of size 4; trims when average Phred score <20> |  
   
    
    You will need to identify two additional arguments to:  
    
    | argument to add    | meaning |
    |:------------------ | :------------------------------------------------ | 
    | filter short reads | drop an entire read if it is below 25 |
    | remove adapter  | Identify which adapter to remove using FastQC output | 

    *Note: We will be adding additional parameters the adapter file specified. Once you reach this point, please ask me for help. These parameters control how aggressively adapters are detected and removed.*
      
      - `2:40:15` 
        - `2` : Seed mismatch threshold (maximum number of mismatches allowed in the adapter sequence match).
        - `40` : Palindrome mode threshold (minimum match length for identifying adapter sequences when the paired-end reads overlap).
        - `15` : Simple adapter trimming threshold (minimum length of a match required to trigger adapter removal).
    
    Once your script is complete, run the script using:

      ```bash
      sh trim.sh
      ```

    *Expected Output:* You will know that your script is working correctly if you see output similar to the following,

    ```bash
    TrimmomaticPE: Started with arguments:
    SRR2589044_1.fastq.gz SRR2589044_2.fastq.gz SRR2589044_1.trim.fastq.gz SRR2589044_1un.trim.fastq.gz SRR2589044_2.trim.fastq.gz SRR2589044_2un.trim.fastq.gz SLIDINGWINDOW:4:20 MINLEN:25 ILLUMINACLIP:trimmomatic_adapters/NexteraPE-PE.fa:2:40:15
    Using PrefixPair: 'AGATGTGTATAAGAGACAG' and 'AGATGTGTATAAGAGACAG'
    Using Long Clipping Sequence: 'GTCTCGTGGGCTCGGAGATGTGTATAAGAGACAG'
    Using Long Clipping Sequence: 'TCGTCGGCAGCGTCAGATGTGTATAAGAGACAG'
    Using Long Clipping Sequence: 'CTGTCTCTTATACACATCTCCGAGCCCACGAGAC'
    Using Long Clipping Sequence: 'CTGTCTCTTATACACATCTGACGCTGCCGACGA'
    ILLUMINACLIP: Using 1 prefix pairs, 4 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
    Quality encoding detected as phred33

    Input Read Pairs: 1107090 Both Surviving: 885220 (79.96%) Forward Only Surviving: 216472 (19.55%) Reverse Only Surviving: 2850 (0.26%) Dropped: 2548 (0.23%)
    TrimmomaticPE: Completed successfully
    ```

    **Part C:** Check output file sizes

    ```bash
    total 453M
    -rw-r--r-- 1 pdrodrig pi-jdragon 124M Feb 11 14:30 SRR2589044_1.fastq.gz
    -rw-r--r-- 1 pdrodrig pi-jdragon  94M Feb 11 18:32 SRR2589044_1.trim.fastq.gz
    -rw-r--r-- 1 pdrodrig pi-jdragon  18M Feb 11 18:32 SRR2589044_1un.trim.fastq.gz
    -rw-r--r-- 1 pdrodrig pi-jdragon 128M Feb 11 14:30 SRR2589044_2.fastq.gz
    -rw-r--r-- 1 pdrodrig pi-jdragon  91M Feb 11 18:32 SRR2589044_2.trim.fastq.gz
    -rw-r--r-- 1 pdrodrig pi-jdragon 271K Feb 11 18:32 SRR2589044_2un.trim.fastq.gz
    ```

    **Part D: Quality control with FASTQC** Run FASTQC on all the `.gz` files (raw and trimmed), then open the HTML reports. When reviewing the FASTQC results, focus on: 
        
        * Per base sequence quality
        * Adapter content 
    

The following questions will be included in HW#6. Please be sure you are able to answer them. 

1) How many reads were trimmed in `SRR2589044_1` and `SRR2589044_2`, respectively. State which files were used to answer this question. 

2) Did the per base sequence quality improve after trimming? If so, at which positions was the biggest improvement? 

3) Was adapter content reduced or eliminated after trimming? In your answer, please state the name of the adapter that was altered. 


## Option 2: Trim Galore 

In the next example we will be using Trim Galore. Trim Galor is a wrapper tool that combines Cutadapt (adapter and quality trimming) and FastQC.  

**BASIC USAGE:** `trim_galore [options] <filename(s)>`

Trim Galore automatically detects adapters by default, but for this exercise we will explicitly specify Illumina adapters.

### Some general options: 

**More options** can be found [here](https://github.com/FelixKrueger/TrimGalore/blob/master/Docs/Trim_Galore_User_Guide.md)

* `-h/--help`
    * Print this help message and exits.
 
* `-q/--quality <INT>`
    * Trim low-quality ends from reads in addition to adapter removal. For RRBS samples, quality trimming will be performed first, and adapter trimming is carried in a second round. Other files are quality and adapter trimmed in a single pass. The algorithm is the same as the one used by BWA (Subtract INT from all qualities; compute partial sums from all indices to the end of the sequence; cut sequence at the index at which the sum is minimal).
    * Default Phred score: `20`
  
* `--phred33`
    * Instructs Cutadapt to use `ASCII+33` quality scores as Phred scores (Sanger/Illumina 1.9+ encoding) for quality trimming.
    * Default: `ON`
  
* `--fastqc`
    * Run FastQC in the default mode on the FastQ file once trimming is complete.
  
* `--fastqc_args "<ARGS>"`
    * Passes extra arguments to FastQC. If more than one argument is to be passed to FastQC they must be in the form `arg1 arg2 [..]`.
    * An example would be: `--fastqc_args "--nogroup --outdir /home/"`.
    * Passing extra arguments will automatically invoke FastQC, so `--fastqc` does not have to be specified separately.

* `--illumina`
    * Adapter sequence to be trimmed is the first 13bp of the Illumina universal adapter `AGATCGGAAGAGC` instead of the default auto-detection of adapter sequence.

* `--gzip`
    * Compress the output file with `gzip`.
    * If the input files are gzip-compressed the output files will be automatically gzip compressed as well.

* `--dont_gzip`
    * Output files won't be compressed with gzip. This overrides `--gzip`.

* `-o/--output_dir <DIR>`
    * If specified all output will be written to this directory instead of the current directory. If the directory doesn't exist it will be created for you.

## Running Trim Galore

!!! example "Class Exercise #2" 

    **Estimated Time:** ~10 minutes
    
    **Collaboration:** Please work with your neighbor if you have questions.

    **Learning goal:** By the end of this exercise, you should be able to:
        
        + Use Trim Galore to perform adapter and quality trimming
        + Verify successful trimming using post-trimming FastQC results
    
    
    **Part A:** Make a copy of the exercise materials into your current working directory. The folder can be found in the location below. Confirm that the folder is copied into your VACC working directory before continuing. 

    ```bash
    /gpfs1/cl/mmg3320/course_materials/trim_galore_exercise
    ```

    **Part B:** Run Trim Galore on `Test_adapter_contamination.fq.gz`. Before running Trim Galore, make sure you:
        
        * Load the Trim Galore module
        * Load the Cutadapt module
        * Load the FastQC module

    **Trimming Instructions** Your trim-galore command should:  
        
        * Use the `--illumina` option
        * Run FASTQC automatically on the trimmed FASTQ file

    Run the command from the directory containing the FASTQ file.


The following questions will be included in HW#6. Please be sure you are able to answer them. 

1) Comparing the FASTQC reports before and after trimming, report how the sequence length changes. 

2) Which FastQC modules showed the most improvement after trimming? Be sure to report which adapter was reduced/eliminated after trimming. 

3) If you examine the Overrepresented Sequences table, do any contaminants remain? 

*You may notice that some overrepresented sequences persist even after trimming. In practice, this can be addressed by modifying or extending the adapter FASTA file to include additional contaminant sequences.*


