# VCF-workflow

## Initial analysis

Step 1: Quality check with FastQC. <br>
FastQC provides insights into sequence quality, adapter contamination, GC content, and more in a user-friendly report.

```bash
#!/bin/bash
set -e
path=$(pwd)

echo "Running FastQC for reads..."
fastqc *.fastq*

echo 'Making directory to store FastQC results...'
mkdir -p ${path}/first_fastqc_results

echo "Saving FastQC results..."
mv *.zip ${path}/first_fastqc_results
mv *.html ${path}/first_fastqc_results
```

Step 2: Trimming with Trimmomatic <br>
Trimmomatic trimms and filters raw sequencing reads, removing low-quality bases and adapter sequences to enhance the quality of downstream analysis.

```bash

```
