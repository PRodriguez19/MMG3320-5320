## Genome Indexing Information for 2025 

For the alignment stage, you will require an *indexed* reference genome. The reference genome has been *indexed* for the following organisms: human, mouse, thale cress, and rice. Each genomics programs has a specific indexing strategy and therefore, requires to be indexed separately. Therefore, you will see indexes created for each organism for each the HISAT2 and STAR programs. The fasta files for these organisms were downloaded from either Gencode or Ensembl. 

Indexes can be found in this location: 

Please note, that the top directory contains all indexes for all organisms. Please take the index file only for the organism you require: 
list file paths 

Below is a depiction of the folder structure: 

**ADD HG19**

├── HISAT2_indexes
│   ├── Ensembl
│   │   ├── Osativa_IRGSP
│   │   │   ├── Oryza_sativa.IRGSP-1.0.60.gtf
│   │   │   ├── Oryza_sativa.IRGSP-1.0.dna.toplevel.fa
│   │   │   ├── Osativa_v60.1.ht2
│   │   │   ├── Osativa_v60.2.ht2
│   │   │   ├── Osativa_v60.3.ht2
│   │   │   ├── Osativa_v60.4.ht2
│   │   │   ├── Osativa_v60.5.ht2
│   │   │   ├── Osativa_v60.6.ht2
│   │   │   ├── Osativa_v60.7.ht2
│   │   │   └── Osativa_v60.8.ht2
│   │   └── TAIR10_v60
│   │       ├── Arabidopsis_thaliana.TAIR10.60.gtf
│   │       ├── Arabidopsis_thaliana.TAIR10.dna.toplevel.fa
│   │       ├── TAIR10-v60.1.ht2
│   │       ├── TAIR10-v60.2.ht2
│   │       ├── TAIR10-v60.3.ht2
│   │       ├── TAIR10-v60.4.ht2
│   │       ├── TAIR10-v60.5.ht2
│   │       ├── TAIR10-v60.6.ht2
│   │       ├── TAIR10-v60.7.ht2
│   │       └── TAIR10-v60.8.ht2
│   └── Gencode
│       ├── hg38_v47
│       │   ├── gencode.v47.primary_assembly.annotation.gtf.gz
│       │   └── GRCh38.primary_assembly.genome.fa.gz
│       └── mm39_v36
│           ├── gencode.vM36.primary_assembly.annotation.gtf.gz
│           └── GRCm39.primary_assembly.genome.fa.gz
└── STAR_indexes



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

## Information about GTF & FASTA files downloaded from Ensembl 

**Organisms included: Rice and Thale Cress**

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

The GTF (General Transfer Format) is an extension of GFF version 2 
and used to represent transcription models. GFF (General Feature Format) 
consists of one line per feature, each containing 9 columns of data. 

### Fields

Fields are tab-separated. Also, all but the final field in each 
feature line must contain a value; "empty" columns are denoted 
with a '.'

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

The following attributes are available. All attributes are semi-colon
separated pairs of keys and values.

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
