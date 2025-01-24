## Homework Assignment #3 (50 points)
**For this assignment you will have until 5PM on Monday, February 3rd to submit on Brightspace. Late assignments will NOT be accepted.**

### Directions for Students: 
Open a new Microsoft Word Document and submit answers to the questions below. The first four lines of your document should contain the following:  

+ Your name
+ MMG3320/5320
+ Today's date
+ Homework Assignment #3

**Assignment**  
Now that we know what type of information is inside the GTF file, let's use the commands we have learned so far to answer a simple question about our data: **how many unique exons are present on chromosome 1 using `chr1-hg19_genes.gtf`?**

To determine the number of unique exons on chromosome 1 you are going to perform a series of steps as shown below. For this homework, you need to figure out the command line for each step. 
	
1. Extract only the genomic coordinates of exon features
2. Subset dataset to only keep genomic coordinates
3. Remove duplicate exons
4. Count the total number of exons

Your end goal is to have a single line of code, wherein you have strung together multiple commands using the pipe operator. But, I recommend that you do it in a stepwise manner as detailed below.

#### 1. Extract only the genomic coordinates of exon features

We only want the exons (not CDS or start_codon features), so let's use `grep` to search for the word "exon". You should do a sanity check on the first few lines of the output of `grep` by piping the result to the `head` command for the first 10 lines. 

***Report the entire command you have at this stage*** in your homework as an answer to question #1. 

#### 2. Subset the extracted information from step 1 to only keep genomic coordinates

We will define the uniqueness of an exon by its genomic coordinates, both start and end, and strand (positive or negative). Therefore, from the step 1 output, we would like to keep 4 columns (chr, start, stop, and strand). 

Go ahead and extract those columns from the output of step 1. 

***Report the entire command you have at this stage*** in your homework as an answer to question #2. 

#### 3. Remove duplicate exons

Now, we need to remove those exons that show up multiple times for different transcripts. We can use the `sort` command with the `-u` option. 

***Report the entire command you have at this stage*** in your homework as an answer to question #3. 

Do you see a change in how the sorting has changed? Check with `less`. 

We will use step 4 to check if `sort -u` worked.

#### 4. Count the total number of exons

First, check how many lines we would have without using `sort -u` by piping the output to `wc -l`.

***Report the entire command you have at this stage and the number of lines output*** in your homework as an answer to question #4a. 

Now, to count how many unique exons are on chromosome 1, we will add back the `sort -u` and pipe the output to `wc -l`. Do you observe a difference in number of lines?

***Report the entire command you have at this stage and the number of lines output*** in your homework as an answer to question #4b.

#### 5. Redirect output into a new file 
Finally, redirect the output of step 4b to a new file and call it chr1-hg19_exons.txt. After outputing, do a final check on the number of lines and file size for this newly created file. 

***Report the entire command you have at this stage, the number of lines output, and the file size*** in your homework as an answer to question #5.
