## Homework Assignment #3 (50 points)
**For this assignment you will have until 5PM on Monday, February 3rd to submit on Brightspace. Late assignments will NOT be accepted.**

### Directions for Students: 
Open a new Microsoft Word Document and submit answers to the questions below. The first four lines of your document should contain the following:  

+ Your name
+ MMG3320/5320
+ Today's date
+ Homework Assignment #4

## Instructions: Submit two screenshots. One of your final script (output from #14 above). The second screenshot will be of your terminal screen after opening `badreads.count.summary` with Nano.  

1. Create a directory called `badreads` in `unit1_unix`

2. Use Jupyter Notebook to create a new script called `generate_bad_reads_summary.sh`. 

3. At the beginning of your script add a **shebang line**. 
    
    ```bash
    #!/bin/bash
    ```

    This line is the absolute path to the Bash interpreter. The shebang line ensures that the bash shell interprets the script even if it is executed using a different shell.

    > #### Why do you need a shebang line? 
    > Having a shebang line is best practice. While your script will run fine without it in environments where bash is the default shell, it won't if the user of this script is using a different shell. To avoid any issues, we explicitly state that this script needs to executed using the bash shell.

4. After the shebang line, skip a line and copy-and-paste the following in Line 3: 
    
    ```bash
    # enter directory with raw FASTQs
    ```

5. In line 4 write a command to change directories into the `raw_fastq` directory. 

    > Why? Because this is where the fastq files are located! 

6. Add the following comment as Line 6. 
    
    ```bash
    # loop over each FASTQ file
    ```
7. Now you are ready to begin writing the for loop. To create the first line of your for loop use the following: 

    |    Loop component      |      Value          |
    | ---------------- | ---------------------- |
    | ***variable_name*** | `filename` |
    | **list** | **all** FASTQ files |

8. Type `do` in Line 8. 

9. Skip a line. On line 10 copy-and-paste the following comment. 

    ```bash
    # create a prefix for all output files
    ```

    
    > #### Now you are ready to move on to create a prefix for all (6) fastq files. These prefixes will be stored in a second variable called `samplename`. To write this line of code successfully, remember the following:
        variable_name=value_of_variable


10. On line 11 create the variable `samplename`
    + The value of the variable should be equal to the `basename` of the variable you created above!
    + Be sure to trim off the file extension `.subset.fq`

    > Why are we doing this? Storing the prefixes in `samplename` will allow us to uniquely label our output files later on. 

11. Copy-and-paste the following into lines 12 and 13. The `echo` statement will keep the user informed on which file is being processed in real-time. 

    ```bash
    # tell us what file we're working on
    echo $filename
    ```
12. Now, you will complete the command below to extract and save all "bad reads" into an output file. A read is considered "bad" if it contains 10 consecutive N's.

Below, you are given the right side of the command, which specifies the output file location. Your task is to complete the left side of the command using `grep`.

This will be lines 15 and 16 of your script.
    
    ```bash
    # Extract all bad read records and save them to a new file  
    WRITE-THE-COMMAND-HERE > ~/unit1_unix/badreads/${samplename}_badreads.fq  
    ``` 
    Requirements: 
    + Ensure that all four lines of each matching sequence read are included in the output. 

    #### Explanation of command above
     You are using `grep` to find all the bad reads (in this case, bad reads are defined as those with 10 consecutive N's), and then extracting the four lines associated with each sequence read and writing them to a file. The output file is named using the `samplename` variable you created earlier in the loop. You will also notice we are adding a path to redirect the output into the `badreads` directory.

    #### Why are we using curly brackets with the variable name?
    When we append a variable to some other free text, we need shell to know where our variable name ends. By encapsulating the variable name in curly brackets we are letting shell know that everything inside it is the variable name. This way when we reference it, shell knows to print the variable `$base` and not to look for a variable called `$base_badreads.fq`.

13. Okay you are almost finished! Just copy and paste the lines below in lines 18-20. 

    ```bash
    # grab the number of bad reads and write it to a summary file
    grep -cH NNNNNNNNNN $filename >> ~/unit1_unix/badreads/badreads.count.summary
    done
    ```
    
    #### Explanation of command above
    Above, you are counting the number of identified bad reads using the count flag of `grep`, `-c`, which will return the number of matches rather than the actual matching lines. Here, you are using the `-H` flag; this will report the filename along with the count value. This is useful because you are writing this information to a running summary file. So rather than just reporting a count value you will also know which file it is associated with. You then closed the loop with `done`. 

14. Save and exit, and voila! You now have a script you can use to assess the quality of all your new datasets. 

To run this script simply enter the following command:

```bash
sh generate_bad_reads_summary.sh
```

#### How do we know if your script worked? 
Take a look inside the `badreads` directory. You should see that for every one of the original FASTQ files, one bad read file was created. You should also have a summary file documenting the total number of bad reads from each file.

```
badreads.count.summary  Irrel_kd_2_badreads.fq  Mov10_oe_1_badreads.fq  Mov10_oe_3_badreads.fq
Irrel_kd_1_badreads.fq  Irrel_kd_3_badreads.fq  Mov10_oe_2_badreads.fq
```
