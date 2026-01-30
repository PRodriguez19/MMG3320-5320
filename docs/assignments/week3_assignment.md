## Homework Assignment #3 (50 points)

**For this assignment you will have until 5PM on Friday, February 6th to submit on Brightspace. Late assignments will NOT be accepted.**

### Directions for Students: 
Open a new Microsoft Word Document and submit answers to the questions below. The first four lines of your document should contain the following:  

+ Your name
+ MMG3320
+ Today's date
+ Homework Assignment #3

## Part A: Find the number of unique exons in Chromosome 1 of Humans (25 points)

**We will be using `chr1-hg19_genes.gtf`?**
 
To determine the number of unique exons in chromosome 1 you will write and perform a series of steps that will: 
	
1. Extract only exon features from the GTF file
2. Subset dataset to keep only genomic coordinates
3. Remove duplicate exons
4. Redirect and check 

Your end goal is to have a single line of code, wherein you have strung together multiple commands using the pipe operator. These steps are detailed a little more below:

#### 1. Extract only exon features from GTF file

We only want the exons (not CDS or start_codon features), so let's use the command to search for the word "exon". You can perform a "sanity check" first by piping the result to the `head` command for the first 10 lines. 

#### 2. Subset the extracted information from step 1 to only keep genomic coordinates

We will define the uniqueness of an exon by its genomic coordinates (start and end position) and strand (positive or negative). Therefore, we would like to keep 4 columns (chr, start, stop, and strand). 

#### 3. Remove duplicate exons

Now, we need to remove exons that show up multiple times for different transcripts.

#### 4. Redirect and check 
Finally, redirect the output to a new file and call it `chr1-hg19_exons.txt`. Do a final check on the number of lines and file size for this newly created file. 

***Report the entire command you have at this stage, the number of lines output, and the file size*** in your homework as an answer for Part 1. 

## Part B: Jupiter Notebook Class Exercise #6 (25 points) - Intro to Shell Scripting (L5)

**Directions for Part 2:** Submit two screenshots. 

+ One of your final script (output from #11). 

+ The second screenshot will be of your terminal screen after running your script (output from #12).  