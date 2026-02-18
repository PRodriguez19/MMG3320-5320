
## Class Exercise #2: Indexing a Genomic Reference (FASTA)

For this tutorial, we will be using an **Organism-specific biological database** called [Cryptodb](https://cryptodb.org/cryptodb/app). These types of databases are updated more frequently than broader biological databases like Ensembl, especially for non-model organisms. As a result, they tend to be more comprehensive.

If you are working with a non-model organism, it is important to check whether a dedicated database exists for your species, as it may provide more up-to-date and detailed information. 

The steps we will perform include: 

1. Making a directory called `indexed_genomes_example`
2. Grab the correct FASTA file from the Cryptodb website 
3. Load the `hisat2` program using `module load` 
4. Build the genome index using `hisat2-build`

### What is the purpose of building a genome index?

Prior to mapping, most aligners require you to construct and index the genome, so that the aligner can quickly and efficiently retrieve reference sequence information. 

Indexing in general is widely used in bioinformatics to improve performance. This step can take a long time depending on how large the genome is (i.e. hours) so it is a good idea to generate a script. 

For this example, we will be indexing the genome of the parasite *Cryptosporidium parvum*, which is very small in size. 

### Instructions

1.  Make a directory called `indexed_genomes_example`

2. Grab the correct FASTA file from the Cryptodb website and place inside of `indexed_genomes_example`. Ultimately, the goal will be to align using a splice-aware aligner. 

    ```bash 
    https://cryptodb.org/common/downloads/Current_Release/CparvumIOWA-ATCC/fasta/data/
    ```

    + Which file will you select? 
    + Once you make a decision, right-click, select copy-link 

    Use the `wget` command 

    ```
    wget path-you-copied
    ```

    You should see the following: 

    ```bash
    Connecting to cryptodb.org (cryptodb.org)|128.192.21.13|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 9275167 (8.8M) [application/x-fasta]
    Saving to: ‘CryptoDB-67_CparvumIOWA-ATCC_Genome.fasta’

    100%[=================================================================>] 9,275,167   5.93MB/s   in 1.5s   
    ```

    Check the file with `head` once the download is complete. 

    ```bash
    >CP044422 | organism=Cryptosporidium_parvum_IOWA-ATCC | version=2022-10-21 | length=920510 | SO=chromosome
    AAACCCCTAAACCTAAACCTAAACCTAAACCTAAACCCTAAACCTAAACCTAAACCTAAA
    CCTAAAACCTAAACCTAAAACCTAAACCTAAAACCTAAAACCTAAACCTAAACCTAAACC
    CCTAAACCTAAACCTAAACCTAAAAACCTAAACCCTAAACCTAAACCTAAAAAACCTAAA
    CCTAAACCTAAACCTAAACCTAAACCTAAACCTAAACCTAAAAAACCTAAAACCTAAACC
    TAAACCTAAACCTAAACCTAAAACCTAAAAACCTAAACCTAAAAACCTAAAAAACCTAAA
    CCTAAAAAACCTAAACCTAAACCTAAAACCTAAAACCTAAACCTAAAACCTAAAACCTAA
    ACCTAAACTAAACCTAAAAAACCTAAAAACCTAAACCCCTAAACCTAAACCTAAAACCTA
    AACCTAAACCTAAACCTAAACCTAAAACCTAAACCTAAACCTAAAACCTAAACCTAAACC        
    TAAACCTAAACCTCCTAAAAACCTAAACCTAAACCTAAACCTAAACCTAAAAAACCTAAA
    ```

3. Load the `hisat2` program using `module load` 


    ```bash
    module load gcc/13.3.0-xp3epyt 
    module load hisat2/2.2.1-x7h4grf 
    ```


4. Build the genome index using the `hisat2-build` program. `hisat2-build` can index reference genomes of any size. 

    ```bash
    hisat2-build
    ```

    Output: 

    ```
    Usage: hisat2-build [options]* <reference_in> <ht2_index_base>
        reference_in            comma-separated list of files with ref sequences
        hisat2_index_base       write ht2 data to files with this dir/basename
    Options:
        -c                      reference sequences given on cmd line (as
                                <reference_in>)

    ```

    To index a reference genome using hisat2-build, the basic command syntax is:

    ```bash
    hisat2-build input-fasta-file basename
    ```

    + `input-fasta-file`: The reference genome FASTA file you want to index.
    + `basename`: The prefix for the output index files. This is the "name" that will be used for all generated index files.

    **For this case, use `CparvumIOWA_ATCC_v68` as the basename**

### Final Ouput

Small indexes are stored in files with the `.ht2` extension. These files together constitute the index: they are all that is needed to align reads to that reference. The original sequence FASTA files is no longer used by `HISAT2` once the index is built.

The final output should look similar to below once complete: 

```bash
CparvumIOWA_ATCC_v68.1.ht2  CparvumIOWA_ATCC_v68.6.ht2
CparvumIOWA_ATCC_v68.2.ht2  CparvumIOWA_ATCC_v68.7.ht2
CparvumIOWA_ATCC_v68.3.ht2  CparvumIOWA_ATCC_v68.8.ht2
CparvumIOWA_ATCC_v68.4.ht2  
CparvumIOWA_ATCC_v68.5.ht2
```

Check that the file sizes are correct:

```bash
-rw-r--r-- 1 pdrodrig pi-jdragon 7.0M Feb 19 11:45 CparvumIOWA_ATCC_v68.1.ht2
-rw-r--r-- 1 pdrodrig pi-jdragon 2.2M Feb 19 11:45 CparvumIOWA_ATCC_v68.2.ht2
-rw-r--r-- 1 pdrodrig pi-jdragon   80 Feb 19 11:45 CparvumIOWA_ATCC_v68.3.ht2
-rw-r--r-- 1 pdrodrig pi-jdragon 2.2M Feb 19 11:45 CparvumIOWA_ATCC_v68.4.ht2
-rw-r--r-- 1 pdrodrig pi-jdragon 3.9M Feb 19 11:45 CparvumIOWA_ATCC_v68.5.ht2
-rw-r--r-- 1 pdrodrig pi-jdragon 2.3M Feb 19 11:45 CparvumIOWA_ATCC_v68.6.ht2
-rw-r--r-- 1 pdrodrig pi-jdragon   12 Feb 19 11:45 CparvumIOWA_ATCC_v68.7.ht2
-rw-r--r-- 1 pdrodrig pi-jdragon    8 Feb 19 11:45 CparvumIOWA_ATCC_v68.8.ht2
```


