
## Genomes Available for MMG3320 (Spring 2025) 

For the *alignment stage*, you will need to specify the location of an *indexed* reference genome. Then during the *counting stage* you will need to specify the location of the GTF annotation file. The locations to these files can be found on the `mmg3320/course_materials` folder in the paths specified below. 

### Information about the Indexed Reference Genomes and GTF 
The reference genome have been *indexed* and are available for the following organisms: human, mouse, thale cress, and rice. The FASTA files for these organisms were downloaded from either [Gencode](https://www.gencodegenes.org/) or [Ensembl](https://useast.ensembl.org/info/data/ftp/index.html). 

Below is a depiction of the folder structure and location for the reference indexes. 

**YOU WILL NOT NEED TO MAKE A COPY OF ANY REFERENCE INDEXES IN YOUR HOME DIRECTORY**

**Simply specify the location of the reference genome/GTF file of interest in your script!**


### File Paths 

GTF & FASTA files

```bash
├── Arabidopsis_thaliana.TAIR10.60.gtf
├── Arabidopsis_thaliana.TAIR10.dna.toplevel.fa
├── Homo_sapiens.GRCh38.113.gtf
├── Homo_sapiens.GRCh38.dna.primary_assembly.fa
├── Mus_musculus.GRCm39.113.gtf
├── Mus_musculus.GRCm39.dna.primary_assembly.fa
├── Oryza_sativa.IRGSP-1.0.60.gtf
└── Oryza_sativa.IRGSP-1.0.dna.toplevel.fa
```

Location of GTF & FASTA files
```
/gpfs1/cl/mmg3320/course_materials/genome_index_mmg3320/genome_reference
```

Human Reference Index, GRCh38

```bash
├── Homo_sapiens.GRCh38.1.ht2
├── Homo_sapiens.GRCh38.2.ht2
├── Homo_sapiens.GRCh38.3.ht2
├── Homo_sapiens.GRCh38.4.ht2
├── Homo_sapiens.GRCh38.5.ht2
├── Homo_sapiens.GRCh38.6.ht2
├── Homo_sapiens.GRCh38.7.ht2
└── Homo_sapiens.GRCh38.8.ht2
```

Location of Human Reference Index, GRCh38

```bash
/gpfs1/cl/mmg3320/course_materials/genome_index_mmg3320/HISAT2_indexes/Ensembl/Hsapiens_GRCh38
```

Mouse Reference Index, GRCm39

```bash
├── Mus_musculus.GRCm39.1.ht2
├── Mus_musculus.GRCm39.2.ht2
├── Mus_musculus.GRCm39.3.ht2
├── Mus_musculus.GRCm39.4.ht2
├── Mus_musculus.GRCm39.5.ht2
├── Mus_musculus.GRCm39.6.ht2
├── Mus_musculus.GRCm39.7.ht2
└── Mus_musculus.GRCm39.8.ht2
```

Location of Mouse Reference Index, GRCm39

```bash
/gpfs1/cl/mmg3320/course_materials/genome_index_mmg3320/HISAT2_indexes/Ensembl/Mmus_GRCm39
```

Rice Reference Index, OSativa

```bash
├── Osativa_v60.1.ht2
├── Osativa_v60.2.ht2
├── Osativa_v60.3.ht2
├── Osativa_v60.4.ht2
├── Osativa_v60.5.ht2
├── Osativa_v60.6.ht2
├── Osativa_v60.7.ht2
└── Osativa_v60.8.ht2
```

Location of Rice Reference Index, OSativa

```bash
/gpfs1/cl/mmg3320/course_materials/genome_index_mmg3320/HISAT2_indexes/Ensembl/Osativa_IRGSP
```

Thale Cress Reference Index, TAIR10

```bash
├── TAIR10-v60.1.ht2
├── TAIR10-v60.2.ht2
├── TAIR10-v60.3.ht2
├── TAIR10-v60.4.ht2
├── TAIR10-v60.5.ht2
├── TAIR10-v60.6.ht2
├── TAIR10-v60.7.ht2
└── TAIR10-v60.8.ht2
```

Location of Thale Cress Reference Index, TAIR10

```bash
/gpfs1/cl/mmg3320/course_materials/genome_index_mmg3320/HISAT2_indexes/Ensembl/TAIR10_v60
```


## Information about GTF & FASTA files downloaded from Ensembl 

**Organisms included: Rice, Thale Cress, Human and Mouse**

Website: 
https://ftp.ensemblgenomes.ebi.ac.uk/pub/plants/current/gtf/
https://ftp.ensemblgenomes.ebi.ac.uk/pub/plants/current/fasta/

Files:
	+ Arabidopsis_thaliana.TAIR10.dna.toplevel.fa.gz
    + Arabidopsis_thaliana.TAIR10.60.gtf.gz	
    + Oryza_sativa.IRGSP-1.0.dna.toplevel.fa.gz
    + Oryza_sativa.IRGSP-1.0.60.gtf.gz

File naming for Ensembl: 

Files are consistently named following this pattern:
   <species>.<assembly>.<version>.gtf.gz

<species>:       The systematic name of the species.
<assembly>:      The assembly build name.
<version>:       The version of Ensembl from which the data was exported.
gtf : All files in these directories are in GTF format
gz : All files are compacted with GNU Zip for storage efficiency.

Toplevel:These files contains all sequence regions flagged as toplevel in an Ensembl
schema. This includes chromsomes, regions not assembled into chromosomes and
N padded haplotype/patch regions.


## More information about the GTF file

The GTF (General Transfer Format) is an extension of GFF version 2 and used to represent transcription models. GFF (General Feature Format) consists of one line per feature, each containing 9 columns of data. 

### Fields

Fields are tab-separated. Also, all but the final field in each feature line must contain a value; "empty" columns are denoted with a '.'

    seqname   - name of the chromosome or scaffold; chromosome names 
                without a 'chr' 
    source    - name of the program that generated this feature, or 
                the data source (database or project name)
    feature   - feature type name. Current allowed features are
                {gene, transcript, exon, CDS, Selenocysteine, start_codon,
                stop_codon and UTR}
    start     - start position of the feature, with sequence numbering 
                starting at 1.
    end       - end position of the feature, with sequence numbering 
                starting at 1.
    score     - a floating point value indiciating the score of a feature
    strand    - defined as + (forward) or - (reverse).
    frame     - one of '0', '1' or '2'. Frame indicates the number of base pairs
                before you encounter a full codon. '0' indicates the feature 
                begins with a whole codon. '1' indicates there is an extra
                base (the 3rd base of the prior codon) at the start of this feature.
                '2' indicates there are two extra bases (2nd and 3rd base of the 
                prior exon) before the first codon. All values are given with
                relation to the 5' end.
    attribute - a semicolon-separated list of tag-value pairs (separated by a space), 
                providing additional information about each feature. A key can be
                repeated multiple times.

### Attributes

The following attributes are available. All attributes are semi-colon separated pairs of keys and values.

- gene_id: The stable identifier for the gene
- gene_version: The stable identifier version for the gene
- gene_name: The official symbol of this gene
- gene_source: The annotation source for this gene
- gene_biotype: The biotype of this gene
- transcript_id: The stable identifier for this transcript
- transcript_version: The stable identifier version for this transcript
- transcript_name: The symbold for this transcript derived from the gene name
- transcript_source: The annotation source for this transcript
- transcript_biotype: The biotype for this transcript
- exon_id: The stable identifier for this exon
- exon_version: The stable identifier version for this exon
- exon_number: Position of this exon in the transcript
- ccds_id: CCDS identifier linked to this transcript
- protein_id: Stable identifier for this transcript's protein
- protein_version: Stable identifier version for this transcript's protein
- tag: A collection of additional key value tags
- transcript_support_level: Ranking to assess how well a transcript is supported (from 1 to 5)

***

## Information about GTF & FASTA files downloaded from Gencode

**Organisms included: Human and Mouse**

Website: https://www.gencodegenes.org/mouse/release_M36.html 

Version: 
+ GRCm39/mm39 - version M36
+ GRCh38/hg38 - version 47 

Description of files:  

FASTA information
Genome sequence, primary assembly (GRCm39) 

    + Nucleotide sequence of the GRCm39 primary genome assembly (chromosomes and scaffolds)
    + The sequence region names are the same as in the GTF/GFF3 files

GTF information 
    
    + It contains the comprehensive gene annotation on the primary assembly (chromosomes and scaffolds) sequence regions 