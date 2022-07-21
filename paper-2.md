# A Genome-wide Framework for Mapping Gene Regulation via Cellular Genetic Screens 2018

## Motivation

With abundant data of genomic data, determining whether and how each regulatory element is functional is essential. The author mainly adapted CRISPR/Cas9 technique to determine the potential enhancers-promoters relationship. But current framework has two limits.
- Low MOI(侵染性)
- Single gene experiment, not at scale
So they develop a eQTL-inspired framework to deal with this problem and obtain potential candidate pairs.

## eQTL-inspired Framework
- **Cell Lines and Culture**
  Skip

- **Pilot gRNA-library design 1119 candidate pairs**
  
  - Pick candidate enhancer regions
  	DNase peak(***ENCSR000EKS***) bedtools-intersect with Hi-C and other data
  - Candidate enhancer gRNAs design using FlashFry
  - TSS positive control gRNAs
  - etc.
  
- **At-Scale Library 5779 candidate enhancers**
  
  - A logistic regression classifier built on the pairs from pilot experiment is used to identify the **intergenic open chromatin regions(namely element region)** to screen for the candidate enhancers
  - The reason why using regression-based method but not experiement-based is for cost and efficiency
  
- **Choice of Exploratory candidate E-P pairs**

  - Analyze ***ENCSR000EKS*** DHS
  - After filtering out pilot DHS, do *Pearson correlation of overlapping epigenomic marks* in order to find similar structure assuming that they are potential pairs, through which we form a **similarity matrix**
  - Apply greedy submodular selection algorithm to identify 948 additional DHSs as exploratory candidate enhancers.

- **gRNA-library cloning**

- **Virus production transduction for CRISPRi**

- **Single cell transcriptome capture**

- **Sequencing of scRNA-seq lib**

  - Pilot
  - At-Scale

- #### **Digital gene expression quantification**

  - Create sparse matrices of UMI counts for each gene across all of cells
  - Use software Cell Ranger

- **Definition of genes well-expressed or detectably expressed**

  genes were defined as well expressed or detectably expressed in K562 if they had at least one read in 0.525% of cells in their respective (pilot or at-scale screen) single cell RNA-seq datasets.

- **Assigning genotypes to cells**

  
