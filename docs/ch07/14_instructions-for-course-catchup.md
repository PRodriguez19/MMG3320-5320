## Instructions for Monday, February 24th 

For today’s class, we will use this session as a **course catch-up day.** If you can answer **“yes”** to all the following questions, you do not need to attend as no new materials will be discussed:

1. Were you able to download the publicly available dataset for your final project? (*Note: not required yet but many have tried and are having trouble*)
2. Do you feel confident copying data from the shared course directory during class?
3. Are you confident in submitting scripts using the SLURM job submission system?
4. Can you transfer files between the VACC and your local desktop?
5. Can you read the sections below (before Wednesday's class) to understand what `conda` and `RSeQC` are and how these programs are used? 
6. Can you follow the instructions provided in the next lesson to independently install the program `RSeQC` using `conda`? 

In Wednesday’s class, we will continue **using** `RSeQC` and reviewing outputs from `HISAT2`. Since `RSeQC` is not available via `module load`, you must install the program beforehand. 

Below is background information on the `RSeQC` and `conda` programs. **Step-by-step instructions are provided in the next lesson.** Do not attempt to run the code shown below; it is for reading purposes only.

## Introducing Conda 

<figure markdown="span">
  ![Conda Logo](../img/Conda_logo.png){ width="500"}
</figure>

Conda is an open-source package management system and environment management system that runs on Windows, macOS, and Linux. Conda quickly installs, runs, and updates packages and their dependencies. Conda also easily creates, saves, loads, and switches between environments on your local computer. Conda was created for Python programs but it can package and distribute software for any language.

More information can be found [here](https://docs.anaconda.com/free/miniconda/index.html). 

### Managing environments with Conda 

Conda allows you to create separate environments containing files, packages, and their dependencies, ensuring they do not interfere with one another. By default, you already have a base environment called base. This environment serves as your "home base," and any packages installed here will always be accessible, regardless of your location on the server.

However, installing all programs in the base environment can lead to software conflicts. Instead, it is best practice to create a separate environment for each program. For example, `RSeQC` recommends using Python 3.10, but your default Python version may be different. To avoid compatibility issues, we will create a dedicated environment for `RSeQC`.

### Creating a Conda Environment 

To create a new environment with Conda, use the following command:

```bash
conda create -n new-env
```

+ `create` - Tells Conda to create a new environment.
+ `-n new-env` - Specifies the name of the environment (new-env in this case). You can change "new-env" to any descriptive name (e.g., rseqc_env) for each environment you create. 

**Example shown below:**

```bash
conda create -n rseqc_env python=3.10
```

### Handling Dependencies 

Conda will check if additional packages ("dependencies") are required. If prompted, type 'y' to proceed:

```bash
Proceed ([y]/n)? y 
```

+ `y` confirms the installation of required dependencies
+ `n` cancels the installation

### Activating the Environment 


After creating the environment, activate it with: 

```bash
conda activate new-env
```

+ `activate` - Switches from the `base` environment to the newly created one
+ `new-env` - Name of environment to activate (must match what you used in `-n new-env`)

Once activated, your prompt will change from `(base)` to `(new-env)`, indicating that you are now working inside the new environment. 

### Installing a Package 

Now you will be able to install the desired package inside the environment you just created. 

```bash
conda install -c bioconda multiqc
```

+ `install` installs a package 
+ `-c bioconda` specifies the channel. `bioconda` hosts many bioinformatics tools. 
+ `multiqc` is the name of package being installed in this example. 

### Deactivating the Environment 

When you are done using the environment, deactivate it using:

```bash
conda deactivate 
```

+ `deactivate` exits the current environment and returns to the `base` environment 


## Introducing `RSeQC` 

`RSeQC` (RNA-Seq Quality Control) is a widely used toolkit designed to evaluate the quality of RNA sequencing (RNA-Seq) data. It provides a collection of scripts to assess the integrity of sequencing reads, alignment quality, gene body coverage, GC content distribution, and other essential metrics. By identifying potential biases or errors in RNA-Seq data,`RSeQC` helps ensure accurate downstream analysis and interpretation.  

This toolkit is particularly useful for verifying the quality of **mapped reads** from aligners such as `HISAT2`, `STAR`, or `TopHat`. 

Commonly used features of RSeQC include:  

- **Read Distribution Analysis:** Determines the proportion of reads mapping to different genomic features (exons, introns, intergenic regions).  
- **Gene Body Coverage:** Evaluates uniformity of read distribution across genes.  
- **Junction Annotation:** Identifies and annotates splice junctions.  
- **GC Content and Read Duplication Metrics:** Helps detect sequencing biases.  

Since `RSeQC` is not typically available as a module on high-performance computing (HPC) systems, users need to install it manually using **Conda** and **pip**. The following tutorial, will guide you through installing `RSeQC`.

More information on `RseQC` can be found [here](https://rseqc.sourceforge.net/#). 



