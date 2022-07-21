# A Genome-wide Framework for Mapping Gene Regulation via Cellular Genetic Screens 2018

## Motivation

With abundant data of genomic data, determining whether and how each regulatory element is functional is essential. The author mainly adapted CRISPR/Cas9 technique to determine the potential enhancers-promoters relationship. But current framework has two limits.
- Low MOI(侵染性)
- Single gene experiment, not at scale
So they develop a eQTL-inspired framework to deal with this problem and obtain potential candidate pairs.

## eQTL-inspired Framework
- Cell Lines and Culture
Skip
- Pilot gRNA-library design 1119 candidate pairs
  - Pick candidate enhancer regions
  	DNase peak bedtools-intersect with Hi-C and other data
  - Candidate enhancer gRNAs design using FlashFry
  - TSS positive control gRNAs
  - etc.
- At-Scale Library 5779 candidate enhancers
  - A logistic regression classifier built on the pairs from pilot experiment is used to identify the **intergenic open chromatin regions**. 

