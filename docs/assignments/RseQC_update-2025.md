
## Additional Resources for RseQC 

`RseQC_apptainer-loop.sh` script. Instead of using conda, this script now uses apptainer to call in a container I created with the extension `.sif`. You will need to modify the lines `BAM_DIR` and `BAM_FILE`. 

```bash
#!/bin/bash
#SBATCH --partition=general
#SBATCH --nodes=1
#SBATCH --ntasks=8  # Single task running sequentially
#SBATCH --mem=20G  # Adjust based on the number of BAM files
#SBATCH --time=24:00:00
#SBATCH --job-name=rseqc-loop
#SBATCH --output=run-%x_%j.out  # %x=job name, %j=job ID

# Load the Apptainer module
module load apptainer/1.3.4

# Path to the RSeQC container
CONTAINER_PATH="/gpfs1/cl/mmg3320/course_materials/containers/rseqc.sif"

# Define input directory (update if needed)
BAM_DIR="/users/p/d/pdrodrig/htseq_2025/bams"
BED_FILE="/users/p/d/pdrodrig/htseq_2025/refseq.hg19.bed12"
OUTPUT_DIR="rseqc_results"

# Create output directory if it doesn't exist
mkdir -p "$OUTPUT_DIR"

# Loop through all BAM files in the directory
for BAM_FILE in "$BAM_DIR"/*.bam; do
    # Extract filename without extension
    NAME=$(basename "$BAM_FILE" .bam)

    echo "Processing: $NAME"

    # Run infer_experiment.py inside the Apptainer container
    apptainer exec "$CONTAINER_PATH" infer_experiment.py -r "$BED_FILE" -i "$BAM_FILE" > "$OUTPUT_DIR/${NAME}.infer_experiment.txt"

    # Run read_distribution.py inside the Apptainer container
    apptainer exec "$CONTAINER_PATH" read_distribution.py -r "$BED_FILE" -i "$BAM_FILE" > "$OUTPUT_DIR/${NAME}.read_distribution.txt"
done
```

## Where can I find the bed12 files? 
The bed12 files can be found in this location: 

```bash
/gpfs1/cl/mmg3320/course_materials/RseQC_bed12
```

## I get this error when running multiqc "Oops! The 'rseqc' MultiQC module broke..."

Please use the [sequera.io website](https://seqera.io/multiqc/) to aid in your interpretations of the `infer_experiment.txt` and `read_distribution.txt` outputs. 



