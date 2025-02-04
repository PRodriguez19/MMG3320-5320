---
Week: "5" 
Lesson: "Submit a job using Slurm"
Date: "Thursday, February 15, 2024"
---

# Submit a job using Slurm


Submitting a job to an HPC machine (such as Bluemoon) is done using the batch system. The batch system allows users to submit jobs requesting the resources (nodes, processors, memory, GPUs) that they need. The jobs are queued and then run as resources become available.

## Batch Computing 
To run a batch job, you put the commands into a text file instead of typing them at the prompt. You submit this file to the batch system, called **SLURM**, which will run it as soon as resources become available. The output you would normally see on your display goes into a log file. You can check the status of your job interactively and/or receive emails when it begins and ends execution. The basic steps include: 

1. Log-in to VACC
2. Write job script 
3. Submit Job 
4. Monitor job and wait for it to run! 
5. Retrieve your output 

A job script can be created using any text editor - such as Nano or Vim - or any GUI editor you may want to download! 
To use the Slurm job scheduler, it requires Slurm directives. To learn more you can do: 

```
man sbatch
```

## Slurm Directives 
At the top of the job script will always be several lines that start with #SBATCH. The Slurm directives provide the job setup information used by Slurm, including resources to request. This information is then followed by the commands to be executed in the script. 

### Partition
First, we will need to specify a partition. A partition refers to a group of nodes which are characterized by their hardware. Specifying a partition is optional and if not specified the default partition is bluemoon. As practice we will specify bluemoon anyways using the following line: 

```
#SBATCH --partition=bluemoon
```

Other Partitions: 

| Partition|  Intended Use  | Max Runtime |  
|:-----------:|:----------:|:----------:| 
|bluemoon | General computing – default partition |30 hours| 
|short | General computing with short runtime | 3 hours| 
|week | General computing with longer runtime | 7 days | 
|bigmem| Large memory requirements computing| 30 hours| 
|bigmemwk | Large memory requirements with longer runtime | 7 days | 

You can check partition usage using the following command: 

```
sinfo -p partition_name
sinfo -p bluemoon 
```

### Walltime 
Walltime is the maximum amount of time your job will run. It’s the runtime of your job.

Your job may run for less time than you request (and you will only be charged for that amount of time), but it will not run for more time than you request.

Walltime is requested with #SBATCH --time=<dd-hh:mm:ss>, where “dd” refers to day(s), “hh” to hour(s), “mm” to minute(s), and “ss” to second(s). You will replace each of these units with a two-digit numeral. Acceptable formats are: mm, mm:ss, hh:mm:ss, dd-hh, dd-hh:mm, dd-hh:mm:ss.

```
# requesting 30 hours of walltime (hh:mm:ss)
#SBATCH --time=30:00:00
```

### Nodes, Tasks, and Cores (CPUs)

The nodes, tasks, and core (CPU) resources you request depend on the type of job you are running. Useful terms to understand are:

    Node: A “node” is a server in the cluster. Each node has is configured with a certain number of cores (CPUs).
    Task: A “task” is a process sent to a core. By default, 1 core is assigned per 1 task.
    Core/CPU: The terms “core” and “cpu” are used interchangeably in high-performance computing.

The most common jobs on the Bluemoon cluster are serial jobs, which run as a single process. VACC recommend's that you begin with 1 node and 2 processes. As we move forward, we will change the number of nodes required for "bigger" jobs.

```
# requesting 1 compute node
#SBATCH --nodes=1
# requesting 2 processes
#SBATCH --ntasks=2
```
### Mail Type

In order to receive emails, you must set what types of emails you would like to receive, using the flag --mail-type. The options include: BEGIN (when your job begins), END (when your job ends), FAIL (if your job fails), ALL. For example:

```
#SBATCH --mail-type=ALL
```

### JOB NAME

Job name is used as part of the name of the job log files. It also appears in lists of queued and running jobs.

Specifying a job name is not required. If you don’t supply a job name, the job ID (supplied by Slurm) is used.

However, if you do wish to specify a job name, use the --job-name flag. For example, where your job name is “myjob”:

```
# replace "myjob" with YOUR chosen job name
#SBATCH --job-name=myjob
```

### Job Submission
Once your job script is written, you can submit it. To submit your job, use the sbatch command with your filename. For example, where the filename is “myfilename”:

```
# replace "myfilename" with YOUR filename
sbatch myfilename
```

When you submit your job, Slurm will respond with the job ID. For example, where the job ID Slurm assigns is “123456,” Slurm will respond:

```
Submitted batch job 123456
```
>Note your job ID! 

# Summary of Header Lines 

|Header Line| What It Does| Example| 
|:--------------------:|:--------------------:|:--------------------:|
|#SBATCH --partition=<partition_name> | Specifies partition | #SBATCH --partition=short or #SBATCH --partition=dggpu | 
|#SBATCH --nodes=<n> | Requests number of nodes (n) | #SBATCH --nodes=1 |
|#SBATCH --ntasks=<n> | Requests number of processes to run | #SBATCH --ntasks=4 | 
|#SBATCH --gres=gpu:<n> | Requests GPUs | #SBATCH --gres=gpu:1 | 
|#SBATCH --mem=<amount> | Requests memory for the entire job | #SBATCH --mem=24G | 
|#SBATCH --mem-per-cpu=<amount> | Requests memory for the entire job | #SBATCH --mem-per-cpu=1G | 
|#SBATCH --time=<dd-hh:mm:ss> | Requests amount of time needed for job. Acceptable formats are: mm, mm:ss, hh:mm:ss, dd-hh, dd-hh:mm, dd-hh:mm:ss| #SBATCH --time=01:00:00|  
|#SBATCH --job-name=<jobname> | Sets job name | #SBATCH --job-name=myjob | 
|#SBATCH --mail-user=<youremail@uvm.edu> | Sets email address where status emails are sent | #SBATCH --mail-user=usr1234@uvm.edu | 
|#SBATCH --mail-type=<type> | Requests that a status email be sent. Options include: NONE, BEGIN, END, FAIL, REQUEUE, ALL. | #SBATCH --mail-type=ALL | 
|#SBATCH --output=%x_%j.out | This command sets a custom output file name by using Slurm-assigned variables: %x = <jobname you have assigned> and %j = <job_id Slurm has assigned>| Output filed named: myjob_123456.out| 

# Commands 

|Command | What It Does | 
|:----------|:-----------|
|sbatch <file_name> | Submits a job, e.g., sbatch myjob | 
|scontrol show job <job_id> | Detailed information about a particular job, e.g., scontrol show job 123456 | 
|squeue | Checks status of all jobs in scheduling queue | 
|squeue -u <username> | Checks status of all jobs belonging to the named user, e.g., squeue -u usr1234 | 
|squeue --start -j <job_id> | Estimates earliest start time of a particular job, e.g., squeue --start -j 123456 | 
|squeue --start -u <username> | Estimates earliest start time of all jobs belonging to the named user, e.g., squeue --start -u usr1234 | 
|scancel -u <username> | Deletes/cancels all jobs belonging to the named user, e.g., scancel -u usr1234 | 
|scancel <job_id> | Deletes/cancels a particular job, e.g., scancel 123456| 

## Citation 

*This lesson was developed using materials from the [Vermont Advanced Computing Center](https://www.uvm.edu/vacc). These materials are freely available.*