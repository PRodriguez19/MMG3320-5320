## Class Exercise 1: Script submission for aligning with HISAT2 

Before we begin, we will submit the following script, which will map and align FASTQ files to the provided reference genome using the HISAT2 aligner. By the end of the lecture, it will be clear why this aligner was chosen. The script will run for approximately 40 minutes to run. We will most likely review the results together during the next class. 

+ [HISAT2 main page](http://daehwankimlab.github.io/hisat2/)
+ [HISAT2 user manual](http://daehwankimlab.github.io/hisat2/manual/)

### Instructions: 

1. Make a copy of the following folder located here: 

	```bash
	/gpfs1/cl/mmg3320/course_materials/HISAT2_example 
	```

2. Check to see that your file sizes are correct. 

	```bash
	total 2.1G
	-rwxr-xr-x 1 pdrodrig pi-jdragon  1.4K Feb 19 08:56 hisat2_align.sh
	-rwxr-xr-x 1 pdrodrig pi-jdragon 1010M Feb 19 08:56 SRR13423162.fastq.gz
	-rwxr-xr-x 1 pdrodrig pi-jdragon  1.1G Feb 19 08:56 SRR13423165.fastq.gz
	```

3. Submit the `hisat2_align.sh` using the SLURM job submission system. 

4. Check to see that your job is running with the `squeue -u your-net-id` command. You will see the following:

	```bash
    JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
    60223   general align_CD pdrodrig  R       0:08      1 node200
	```

<figure markdown="span">
  ![Loki's Friend Thor](../img/dog_good_job.jpg){ width="300"}
</figure>

***