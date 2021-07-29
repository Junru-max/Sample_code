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
As part of its goal to overcome obstacles in realizing the full benefits of the application of genomic methods for cancer diagnosis and the guidance of precision treatment, the BCM Human Genome Sequencing Center and the TCRB has made genomic sequence data from high quality human tumor specimens with matched normal freely available to the public without the barriers that other, more restrictive data sharing initiatives impose. This data set can be accessed at http://txcrb.org/open.html

We have applied mutect2 to the case_001, case_002 and case_003 tumor-normal pair data. For case_001, download TCRBOA1-N-WEX.read1.fastq.bz2, TCRBOA1-N-WEX.read2.fastq.bz2, TCRBOA1-T-WEX.read1.fastq.bz2 and TCRBOA1-T-WEX.read1.fastq.bz2. Download the corresponding data for case_002 and case_003.

```
function test() {
  console.log("notice the blank line before this function?");
}
```
