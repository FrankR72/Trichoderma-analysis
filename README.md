# VCF-workflow
<img src="https://images.unsplash.com/photo-1643780668909-580822430155?q=80&w=2232&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D" width="1000" height="563"/>



## Initial analysis

Step 1: Quality check with FastQC <br>
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
#!/bin/bash

path=$(pwd)

echo 'Beginning Trimmomatic...'
for infile in *_R1.fastq
do
  base=$(basename ${infile} _R1.fastq)
  trimmomatic PE ${infile} ${base}_R2.fastq\
     ${base}_R1.trim.fastq.gz ${base}_R1.untrim.fastq.gz\
     ${base}_R2.trim.fastq.gz ${base}_R2.untrim.fastq.gz\
     SLIDINGWINDOW:4:20 MINLEN:25 ILLUMINACLIP:NexteraPE-PE.fa:2:40:15
done

echo 'Making directory to store untrimmed reads...'
mkdir -p ${path}/untrimmed_reads
for infile in *untrim.fastq.gz
do
  mv ${infile} ${path}/untrimmed_reads
done

echo 'Making directory to store original reads...'
mkdir -p ${path}/original_reads
for infile in *_R1.fastq
do
  base=$(basename ${infile} _R1.fastq)
  mv ${infile} ${path}/original_reads
  mv ${base}_R2.fastq ${path}/original_reads
done

echo 'Unzipping trimmed reads'
for infile in *.trim.fastq.gz
do
  gzip -d ${infile}
done

echo "Trimmomatic done!"
```
Step 3: Second Quality check with FastQC
```bash
#!/bin/bash
set -e

path=$(pwd)

echo "Running FastQC for trimmed reads..."
fastqc *.fastq*

echo 'Making directory to store FastQC reads...'
mkdir -p ${path}/reads_fastqc_results

echo "Saving FastQC results..."
mv *.zip ${path}/reads_fastqc_results
mv *.html ${path}/reads_fastqc_results
cd ${path}/reads_fastqc_results

echo 'Making directory to store summary documentation...'
mkdir -p ${path}/reads_fastqc_results/fastqc_docs

echo "Saving summary..."
cat */summary.txt > ${path}/reads_fastqc_results/fastqc_docs/fastqc_summaries.txt
```
