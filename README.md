# VCF-workflow


## Initial analysis

Step 1: Quality check on reads

```
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
