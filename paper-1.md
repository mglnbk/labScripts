# Activity-by-contact model of enhancer–promoter regulation from thousands of CRISPR perturbations

## Goal

decide which enhancers regulate which promoters in the background of most of enhancers' fuctions are not known very clearly. Jesse, the author of the paper, uses CRISPERi-dCas9 to perturb candidate genes locus in order to explore potential EPP(enhancer-promoter pair)

## CRISPERi-FlowFISH
- 两个生物大分子，Cas9蛋白和gRNA（guide RNA）组成了CRISPR/Cas9基因编辑系统。在细胞内，Cas9蛋白与gRNA形成复合物，能特异性鉴别出靶序列。此过程中，Cas9蛋白负责将复合物定点到靶DNA和剪切靶DNA。Cas9蛋白有6个结构域，分别是Rec I、Rec II、Bridge Helix、PAM Interacting、HNH和RuvC。Rec I是6个中最大的一个结构域，负责结合gRNA。一旦结合了靶DNA，Bridge Helix就负责启动剪切。PAM interacting结构域赋予Cas9对PAM序列的特异性要求，负责启动与靶DNA的结合。HNH和RuvC都是核酸酶结构域，剪切单链DNA。
- dCas9蛋白指的是通过往RuvC和NHN两个核酸酶结构域分别导入氨基酸突变D10A和H840A，使得Cas9蛋白失去切割DNA活性，但仍保留结合DNA的能力，称为dead Cas9, 为了进一步提高转录抑制的效率，dCas9融合了一个基因抑制结构域，如KRAB（krüppel-associated box）结构域，这样的蛋白称之为dCas9-KRAB
- RNA-FISH：如果待检测的细胞或组织切片上的靶核酸与所用的核酸探针是同源互补的，二者经变性-退火-复性，即可形成靶核酸与核酸探针的杂交体。将核酸探针的某一种核苷酸标记上报告分子如生物素或直接标记荧光素，可利用该报告分子与荧光素标记的特异亲和素之间的免疫化学反应或直接经荧光检测体系在镜下对待测核酸(mRNA、lncRNA、circRNA、miRNA)进行定性、半定量或相对定位分析的一种实验方法。
- FACS：根据荧光流式分选



## Sample selection
K562 human erythroleukemia cells 

## Procedure
Use CRISPERi-dCas9-CRAB to establish a database of DEG or PEG pairs, which is used later on for enhancer-promoter prediction. They propose a formula: 
$$
	ABC_{score} = \frac{A_E \times C{E,G}}{\sum_{5Mb-of-G} A_e \times C_{e,G} }
$$
Note: A refers to the geometric mean of the read counts of DHS and H3K27ac chromatin ChIP-seq at element E and C as KR-normalized Hi-C frequency between E and promoter gene G at 5-kb resolution.

## Result
Pretty well.


