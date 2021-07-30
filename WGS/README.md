# Somatic Variant Calling

Somatic Variant Calling is implemented across six main procedures:

1.Genome Alignment \
2.Alignment Co-Cleaning \
3.Somatic Variant Calling \
4.Variant Annotation \
5.Mutation Aggregation \
6.Aggregated Mutation Masking 


Overall workflow:

Genome Alignment & Alignment Co-Cleaning \
![image](https://github.com/Junru-max/Sample_code/blob/master/WGS/Photos/dna-alignment-pipeline_1.png)

Somatic Variant Calling & Variant Annotation \
![image](https://github.com/Junru-max/Sample_code/blob/master/WGS/Photos/koku5rqdl77g.png)


Detection of cancer mutations is different from the detection of germline genetic variants, mainly because:

We are only interested in somatic mutations which are the differences between tumor and germline DNA.
Tumor samples are often impure due to a mixture of tumor and normal cells.
Tumors consists of sub-clones with different somatic mutations.
Germline genotype callers such as GATK's HaplotypeCaller are optimized for diploid samples or samples of known ploidy, and for detecting variants with allele frequencies close to 0.5 or 1. Therefore, somatic variants should be called with specialized callers. We follows GATK best practices for somatic mutation using Exome/Panel + Whole Genome data. MuTect2 is a somatic SNP and indel caller that combines the DREAM challenge-winning somatic genotyping engine of the original MuTect with the assembly-based machinery of HaplotypeCaller.

# Data
As part of its goal to overcome obstacles in realizing the full benefits of the application of genomic methods for cancer diagnosis and the guidance of precision treatment, the BCM Human Genome Sequencing Center and the TCRB has made genomic sequence data from high quality human tumor specimens with matched normal freely available to the public without the barriers that other, more restrictive data sharing initiatives impose. This data set can be accessed at http://txcrb.org/open.html (not valid)
https://platform.dnanexus.com/projects/BQZ8jJ80JvPgyQxgQ18009vg/data/ (latest link,wget + link)
We have applied mutect2 to the case_001, case_002 and case_003 tumor-normal pair data. For case_001, download TCRBOA1-N-WEX.read1.fastq.bz2, TCRBOA1-N-WEX.read2.fastq.bz2, TCRBOA1-T-WEX.read1.fastq.bz2 and TCRBOA1-T-WEX.read1.fastq.bz2. Download the corresponding data for case_002 and case_003.

Reference data in google cloud (Download by google-cloud-sdk: gsutil cp -p rawpath localpath,see also in https://cloud.google.com/sdk/docs/install#linux)


https://console.cloud.google.com/storage/browser/genomics-public-data/resources/broad/hg38/v0
Homo_sapiens_assembly38.fasta; Homo_sapiens_assembly38.fasta.fai; Homo_sapiens_assembly38.dict; 1000G_phase1.snps.high_confidence.hg38.vcf.gz; 1000G_phase1.snps.high_confidence.hg38.vcf.gz.tbi;  Homo_sapiens_assembly38.known_indels.vcf.gz; Homo_sapiens_assembly38.known_indels.vcf.gz.tbi;  Mills_and_1000G_gold_standard.indels.hg38.vcf.gz; Mills_and_1000G_gold_standard.indels.hg38.vcf.gz.tbi.

https://console.cloud.google.com/storage/browser/gatk-best-practices/somatic-hg38

small_exac_common_3.hg38.vcf.gz; small_exac_common_3.hg38.vcf.gz.tbi;\ 
af-only-gnomad.hg38.vcf.gz; af-only-gnomad.hg38.vcf.gz.tbi.

# Initial QC and adapter trimming
You should perform proper Initial QC and adapter trimming for your own data.

# Generation and initial processing of bam files
Bam files should be generated and preprocessed according to GATKs Best Practice for variant discovery (https://www.broadinstitute.org/gatk/guide/best-practices).

# ALIGNING THE READS TO A REFERENCE GENOME
We can use bwa_mem to align the reads to a reference genome. We choose hg37 as the human reference genome.

To use bwa_mem to align the case_001 reads, submit the following jobs to HTC cluster.
```
#!/bin/bash
#
#sBATCH --job-name=bwa_mem_case1
#SBATCH -N 1
#SBATCH --cpus-per-task=8 # Request that ncpus be allocated per process.
#SBATCH -t 1-00:00 # Runtime in D-HH:MM
#SBATCH --output=bwa_mem_case1.out

module load gcc/8.2.0
module load bwa/0.7.17
module load samtools/1.9

bwa index hg38_v0_Homo_sapiens_assembly38.fasta

bwa mem -t 8 -T 0 \
-R '@RG\tID:0\tPL:Illumina\tPU:700807_20131106_C2P0JACXX-6-ID04\tLB:TCRB.TCR002103-B-1_1AMP\tDT:2013-11-17T20:22:39-0600\tSM:TCR002103-B\tCN:BCM' \
hg38_v0_Homo_sapiens_assembly38.fasta  \
<(bzip2 -dc ./case_001/TCRBOA1-N-WEX.read1.fastq.bz2) \
<(bzip2 -dc ./case_001/TCRBOA1-N-WEX.read2.fastq.bz2) | samtools view -Shb -o TCRBOA1-N-WEX.bam -

bwa mem -t 8 -T 0 \
-R '@RG\tID:0\tPL:Illumina\tPU:700807_20131106_C2P0JACXX-6-ID06\tLB:TCRB.TCR002103-T-1_1AMP\tDT:2013-11-17T20:25:26-0600\tSM:TCR002103-T\tCN:BCM' \
hg38_v0_Homo_sapiens_assembly38.fasta  \
<(bzip2 -dc ./case_001/TCRBOA1-T-WEX.read1.fastq.bz2) \
<(bzip2 -dc ./case_001/TCRBOA1-T-WEX.read2.fastq.bz2) | samtools view -Shb -o TCRBOA1-T-WEX.bam -

```
