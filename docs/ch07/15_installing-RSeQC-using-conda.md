## Installing RSeQC using Conda 

Follow these steps to install RSeQC using Conda in your VACC account.

### Step 1. Load Python 

Before installing Conda and RSeQC, load the appropriate Python module:

```bash
module load python3.10-anaconda/2023.03-1 
```

### Step 2: Verify the Loaded Module

Check that the Python module is correctly loaded by listing active modules:

```bash
module list
```

Expected output:

```bash
Currently Loaded Modules:
  1) python3.10-anaconda/2023.03-1
```

### Step 3: Confirm Python and Conda Installation

Verify that Python and Conda are available:

```bash
python --version
conda --version
```

Expected output:

```bash
Python 3.10.9
conda 22.9.0
```

### Step 4: Create a Conda Environment

Create a dedicated Conda environment for `RSeQC` with Python 3.10:

```bash
conda create -n rseqc_env python=3.10 -y
```

During installation, you may see messages like:

```bash
Collecting package metadata (current_repodata.json): done
Solving environment: done
```

Conda will check for required dependencies. If prompted, type y to proceed:

```bash
Proceed ([y]/n)? y 
```

+ `y` confirms the installation of required dependencies
+ `n` cancels the installation

At the end, you will see a message like this:

```bash
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate rseqc_env
#
# To deactivate an active environment, use
#
#     $ conda deactivate

Retrieving notices: ...working... done
```

Now, proceed to Step 5.

### Step 5: Activate the `rseqc_env` Envrionment

Activate the newly created Conda environment:

```bash
conda activate rseqc_env
```

Once activated, your terminal prompt will change from (base) to (rseqc_env), indicating that you are inside the new environment.

```bash
[pdrodrig@vacc-login4 pdrodrig]$ conda activate rseqc_env 
(rseqc_env) [pdrodrig@vacc-login4 pdrodrig]
```

### Step 6: Install `RSeQC` Using `pip`

With the environment activated, install RSeQC using pip:

```bash
pip install RSeQC
```

Once installed successfully, you should see output similar to:

```bash
Successfully installed RSeQC-5.0.4 bx-python-0.13.0 
```

### Step 7: Verify the Installation

Test the installation by displaying the help page for read_distribution.py:

```bash
read_distribution.py --help
```

Expected output:

```bash
Usage: read_distribution.py [options]

Check reads distribution over exon, intron, UTR, intergenic ... etc
The following reads will be skipped:
	qc_failed
	PCR duplicate
	Unmapped
	Non-primary (or secondary)
```

**Congratulations! You have successfully installed RSeQC!**


### Important Notes

+ Each time you use RSeQC, you must first activate the Conda environment:

```bash
conda activate rseqc_env
```

+ RSeQC will not work in the `base` environment.

+ To deactivate the environment after use, run:

```bash
conda deactivate
```

