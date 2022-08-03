# Differential cofactor dependencies define distinct types of human enhancers

## Main Goal

use cofactor dependencies to identify the distinct type of enhancers, providing a framework 
to illustrate enhancer function and regulatory specifity.

## BackGround

Transcriptional process needs **TFs and COFs** binding. Prominent COF include <u>EP300, BRD4 and CDK9</u>
, which mediates throurough **initiation, pause-release and elongation step**

## Process

1. Use AID-system to deplete each COF mentioned above
2. Use STARR-seq to assess the enhancer-activity following the loss of each COF
3. Each COF is tested based on the data obtaained above
4. The final result shows that some of transcriptional processes are running well without COFs

## Main Difficulty
1. **AID-system**

   **The auxin-inducible degron (AID)** is a powerful tool that is used for depletion of proteins to study their function *in vivo*. This method can conditionally induce the degradation of any protein by the proteasome, simply by the addition of the plant hormone auxin. This approach is particularly valuable to study the function of essential proteins.

2. **STARR-seq reveals what kinds of information and how**

   - Background

     **3' UTR** refers to the region following the translation terminal region

   - clone into ORF downstream regulatory region making it high expressed

   - reveal to what extent the regulatory regions are regulated, mainly the vigor of enhancer.

3. **How to define the enhancer regions**

   - TF motifs use JASPAR database
   - Use STARR enhancers overlap **motifs, repeat elements, TF and COF binding sites or histone modifications sequence peak** in order to construct binary matrices

4. **How to divide enhancers into 4 groups, PAM algorithm**

   - Use enhancer change after each depletion of BRD2, BRD4, P300/CBP, MED14 and CDK7

## Result

They found that some of the TFs don't rely on the specific COFs. And COFs can be used as a signature to divide the cells into different groups
