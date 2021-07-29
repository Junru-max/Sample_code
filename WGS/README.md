# Somatic Variant Calling

Somatic Variant Calling is implemented across six main procedures:

1.Genome Alignment \
2.Alignment Co-Cleaning \
3.Somatic Variant Calling \
4.Variant Annotation \
5.Mutation Aggregation \
6.Aggregated Mutation Masking \


Overall workflow:

![image](https://github.com/Junru-max/Sample_code/blob/master/WGS/Photos/dna-alignment-pipeline_1.png)


Detection of cancer mutations is different from the detection of germline genetic variants, mainly because:

We are only interested in somatic mutations which are the differences between tumor and germline DNA.
Tumor samples are often impure due to a mixture of tumor and normal cells.
Tumors consists of sub-clones with different somatic mutations.
Germline genotype callers such as GATK's HaplotypeCaller are optimized for diploid samples or samples of known ploidy, and for detecting variants with allele frequencies close to 0.5 or 1. Therefore, somatic variants should be called with specialized callers. We follows GATK best practices for somatic mutation using Exome/Panel + Whole Genome data. MuTect2 is a somatic SNP and indel caller that combines the DREAM challenge-winning somatic genotyping engine of the original MuTect with the assembly-based machinery of HaplotypeCaller.

```
function test() {
  console.log("notice the blank line before this function?");
}
```
