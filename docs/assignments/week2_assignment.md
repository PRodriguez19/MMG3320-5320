## Homework Assignment #2 (50 points)
**For this assignment you will have until 10AM on Wednesday, January 28th to submit on Brightspace. Late assignments will NOT be accepted.**

### Directions for Students: 
Open a new Microsoft Word Document and submit answers to the questions below. The first four lines of your document should contain the following:  

+ Your name
+ MMG3320
+ Today's date
+ Homework Assignment #2

### Part A: Practice using `Less` 

1. This is a multi-part question:   
	
	a. Navigate into the `genomics_data` folder.

	b. Use the `less` command to open up the file `Encode-hesc-Nanog.bed`.  
    
	c. Use the shortcut to get to the end of the file.  
	
	d. Search for the string `chr11`.     
	
	e. **Report** two rows that start with `chr11`. Include the start and end position in your answer. 
    
    > Exit the `less` buffer.  

2. Print to screen the last 5 lines of the file `Encode-hesc-Nanog.bed`. Submit a screenshot of the output as your answer.

3. How many commands have you typed after going through this exercise? Submit a screenshot of the output as your answer.

### Part B: Generating your own script

You got the following line of codes from a trusted source but need to modify it so you can submit it to the VACC-Bluemoon server. You decide its time to make your own script. Follow the steps below: 

1. Create a new file in the `other` directory called `script.sh`. 

	+ The *.sh* file extension typically indicates that a file is a **shell script**. 

	+ In Unix-like operating systems (such as Linux and macOS), shell scripts are plain text files containing a sequence of commands that can be executed by a shell.

2. Paste in the code below to `script.sh`. 

	```
	STAR --runThreadN 4 \
	--runMode genomeGenerate \
	--genomeDir /username/chr1_hg19_STAR_index/ \
	--genomeFastaFiles /username/reference_data_ensembl/Homo_sapiens.GRCh19.dna.chromosome.1.fa \
	--sjdbGTFfile /username/reference_data_ensembl/Homo_sapiens.GRCh19.gtf 
	```
	
3. Replace every occurrence of "username" with your netid. 

4. Delete the line containing `--runMode`

5. Change the `--runThreadN` from 4 to 6  

6. You would also like to use the newest genome assembly, human reference 38 (hg38/GRCh38). Change this as well in your script. 

7. Submit a screenshot of your script in the **Nano buffer** as homework Part B. 

Save the file and EXIT. 

**Please Take Note:** 

+ The argument `--genomeDir` is pointing to an entire directory while `--genomeFastaFiles` is pointing to a specific file. This is really important as the program is looking for specific files or entire directories (with files in them!) to run successfully. 

+ Each line here ends with a `\`. The `\` can also be used as an escape character that signals that the character following it has a special meaning in this case its a continuation. 