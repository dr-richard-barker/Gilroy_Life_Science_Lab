---
description: 'Statistical report of test project GeneLab Ecotypes in space:'
---

# Statistical report (FL vs GC) BRIC19 OSD-37

### Pairwise analysis of conditions with SARTools' edgeR library

***

Author: Richard Barker

[Variation in the transcriptome of different ecotypes of Arabidopsis thaliana reveals signatures of oxidative stress in plant responses to spaceflight](https://pubmed.ncbi.nlm.nih.gov/30644539)Authors: Choi WG, Barker, Kim SH, Swanson SJ, Gilroy S.PubMed ID: [30644539](https://pubmed.ncbi.nlm.nih.gov/30644539)DOI: [10.1002/ajb2.1223](https://doi.org/10.1002/ajb2.1223)\


Date: 2018-08-18

***

### Table of contents

1. Introduction
2. Description of raw data
3. Filtering low counts
4. Variability within the experiment: data exploration
5. Normalization
6. Differential gene expression analysis
7. R session information and parameters
8. Bibliography

***

### 1 Introduction

The analyses reported in this document are part of the GeneLab Ecotypes in space project. The aim is to find features that are differentially expressed between Col, Cvi, Ler and WS2. The statistical analysis process includes data normalization, graphical exploration of raw and normalized data, test for differential expression for each feature between the conditions, raw p-value adjustment and export of lists of features having a significant differential expression between the conditions.

The analysis is performed using the R software \[R Core Team, 2014], Bioconductor \[Gentleman, 2004] packages including edgeR \[Robinson, 2010] and the SARTools package developed at PF2 - Institut Pasteur. Normalization and differential analysis are carried out according to the edgeR model and package. This report comes with additional tab-delimited text files that contain lists of differentially expressed features.

For more details about the edgeR methodology, please refer to its related publications \[Robinson, 2007, 2008, 2010 and McCarthy, 2012].

***

### 2 Description of raw data

The count data files and associated biological conditions are listed in the following table.

<table><thead><tr><th width="130">SampleLabel</th><th width="360">File</th><th>Treatment</th><th>Genotype</th></tr></thead><tbody><tr><td>Col_SpaceFlight_1</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>Col</td></tr><tr><td>Col_SpaceFlight_2</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>Col</td></tr><tr><td>Col_SpaceFlight_3</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>Col</td></tr><tr><td>Col_SpaceFlight_4</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>Col</td></tr><tr><td>Col_Ground_1</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>Col</td></tr><tr><td>Col_Ground_2</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>Col</td></tr><tr><td>Col_Ground_3</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>Col</td></tr><tr><td>Col_Ground_4</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>Col</td></tr><tr><td>Cvi_SpaceFlight_1</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>Cvi</td></tr><tr><td>Cvi_SpaceFlight_2</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>Cvi</td></tr><tr><td>Cvi_SpaceFlight_3</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>Cvi</td></tr><tr><td>Cvi_Ground_1</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>Cvi</td></tr><tr><td>Cvi_Ground_2</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>Cvi</td></tr><tr><td>Cvi_Ground_3</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>Cvi</td></tr><tr><td>Ler_SpaceFlight_1</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>Ler</td></tr><tr><td>Ler_SpaceFlight_2</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>Ler</td></tr><tr><td>Ler_SpaceFlight_3</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>Ler</td></tr><tr><td>Ler_Ground_1</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>Ler</td></tr><tr><td>Ler_Ground_2</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>Ler</td></tr><tr><td>Ler_Ground_3</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>Ler</td></tr><tr><td>WS2_SpaceFlight_1</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>WS2</td></tr><tr><td>WS2_SpaceFlight_2</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>WS2</td></tr><tr><td>WS2_SpaceFlight_3</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>WS2</td></tr><tr><td>WS2_SpaceFlight_4</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>SpaceFlight</td><td>WS2</td></tr><tr><td>WS2_Ground_1</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>WS2</td></tr><tr><td>WS2_Ground_2</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>WS2</td></tr><tr><td>WS2_Ground_3</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>WS2</td></tr><tr><td>WS2_Ground_4</td><td>DRB_RNAseq_counts_2018_Ecotypes_BRIC19_v5.txt</td><td>Ground</td><td>WS2</td></tr></tbody></table>

After loading the data we first have a look at the raw data table itself. The data table contains one row per annotated feature and one column per sequenced sample. Row names of this table are feature IDs (unique identifiers). The table contains raw count values representing the number of reads that map onto the features. For this project, there are 41671 features in the count data table.

|             | Col\_SpaceFlight\_1 | Col\_SpaceFlight\_2 | Col\_SpaceFlight\_3 | Col\_SpaceFlight\_4 | Col\_Ground\_1 | Col\_Ground\_2 | Col\_Ground\_3 | Col\_Ground\_4 | Cvi\_SpaceFlight\_1 | Cvi\_SpaceFlight\_2 | Cvi\_SpaceFlight\_3 | Cvi\_Ground\_1 | Cvi\_Ground\_2 | Cvi\_Ground\_3 | Ler\_SpaceFlight\_1 | Ler\_SpaceFlight\_2 | Ler\_SpaceFlight\_3 | Ler\_Ground\_1 | Ler\_Ground\_2 | Ler\_Ground\_3 | WS2\_SpaceFlight\_1 | WS2\_SpaceFlight\_2 | WS2\_SpaceFlight\_3 | WS2\_SpaceFlight\_4 | WS2\_Ground\_1 | WS2\_Ground\_2 | WS2\_Ground\_3 | WS2\_Ground\_4 |
| ----------- | ------------------- | ------------------- | ------------------- | ------------------- | -------------- | -------------- | -------------- | -------------- | ------------------- | ------------------- | ------------------- | -------------- | -------------- | -------------- | ------------------- | ------------------- | ------------------- | -------------- | -------------- | -------------- | ------------------- | ------------------- | ------------------- | ------------------- | -------------- | -------------- | -------------- | -------------- |
| AT1G01010.1 | 297                 | 411                 | 248                 | 258                 | 303            | 285            | 131            | 332            | 172                 | 172                 | 274                 | 551            | 271            | 154            | 392                 | 161                 | 254                 | 342            | 96             | 25             | 162                 | 49                  | 85                  | 121                 | 271            | 93             | 44             | 32             |
| AT1G01020.1 | 23                  | 50                  | 34                  | 47                  | 84             | 40             | 97             | 70             | 24                  | 29                  | 75                  | 113            | 67             | 32             | 74                  | 47                  | 90                  | 87             | 27             | 6              | 52                  | 5                   | 14                  | 10                  | 26             | 9              | 6              | 5              |
| AT1G01020.2 | 22                  | 44                  | 30                  | 44                  | 83             | 32             | 93             | 60             | 20                  | 27                  | 61                  | 108            | 65             | 33             | 71                  | 46                  | 80                  | 81             | 21             | 6              | 42                  | 4                   | 13                  | 8                   | 22             | 9              | 6              | 4              |
| AT1G01030.1 | 18                  | 42                  | 22                  | 23                  | 79             | 40             | 30             | 36             | 42                  | 29                  | 41                  | 11             | 12             | 7              | 30                  | 29                  | 72                  | 17             | 26             | 2              | 56                  | 6                   | 29                  | 32                  | 49             | 14             | 12             | 14             |
| AT1G01040.1 | 262                 | 303                 | 229                 | 225                 | 477            | 324            | 345            | 517            | 279                 | 323                 | 659                 | 850            | 411            | 269            | 545                 | 225                 | 402                 | 414            | 242            | 20             | 370                 | 70                  | 200                 | 138                 | 500            | 149            | 120            | 64             |
| AT1G01040.2 | 236                 | 281                 | 213                 | 204                 | 447            | 303            | 329            | 475            | 261                 | 309                 | 612                 | 801            | 374            | 254            | 512                 | 214                 | 360                 | 367            | 226            | 18             | 342                 | 67                  | 188                 | 129                 | 470            | 128            | 117            | 59             |

Looking at the summary of the count table provides a basic description of these raw counts (min and max values, median, etc).

|         | Col\_SpaceFlight\_1 | Col\_SpaceFlight\_2 | Col\_SpaceFlight\_3 | Col\_SpaceFlight\_4 | Col\_Ground\_1 | Col\_Ground\_2 | Col\_Ground\_3 | Col\_Ground\_4 | Cvi\_SpaceFlight\_1 | Cvi\_SpaceFlight\_2 | Cvi\_SpaceFlight\_3 | Cvi\_Ground\_1 | Cvi\_Ground\_2 | Cvi\_Ground\_3 | Ler\_SpaceFlight\_1 | Ler\_SpaceFlight\_2 | Ler\_SpaceFlight\_3 | Ler\_Ground\_1 | Ler\_Ground\_2 | Ler\_Ground\_3 | WS2\_SpaceFlight\_1 | WS2\_SpaceFlight\_2 | WS2\_SpaceFlight\_3 | WS2\_SpaceFlight\_4 | WS2\_Ground\_1 | WS2\_Ground\_2 | WS2\_Ground\_3 | WS2\_Ground\_4 |
| ------- | ------------------- | ------------------- | ------------------- | ------------------- | -------------- | -------------- | -------------- | -------------- | ------------------- | ------------------- | ------------------- | -------------- | -------------- | -------------- | ------------------- | ------------------- | ------------------- | -------------- | -------------- | -------------- | ------------------- | ------------------- | ------------------- | ------------------- | -------------- | -------------- | -------------- | -------------- |
| Min.    | 0                   | 0                   | 0                   | 0                   | 0              | 0              | 0              | 0              | 0                   | 0                   | 0                   | 0              | 0              | 0              | 0                   | 0                   | 0                   | 0              | 0              | 0              | 0                   | 0                   | 0                   | 0                   | 0              | 0              | 0              | 0              |
| 1st Qu. | 0                   | 0                   | 1                   | 2                   | 1              | 1              | 0              | 0              | 0                   | 0                   | 1                   | 1              | 0              | 0              | 1                   | 0                   | 1                   | 1              | 0              | 0              | 0                   | 0                   | 0                   | 0                   | 0              | 0              | 0              | 0              |
| Median  | 22                  | 28                  | 23                  | 26                  | 34             | 28             | 20             | 38             | 19                  | 21                  | 37                  | 48             | 26             | 15             | 46                  | 16                  | 31                  | 34             | 14             | 2              | 23                  | 5                   | 11                  | 10                  | 27             | 9              | 5              | 4              |
| Mean    | 285                 | 317                 | 283                 | 293                 | 379            | 319            | 262            | 449            | 231                 | 278                 | 399                 | 495            | 262            | 177            | 479                 | 213                 | 366                 | 372            | 189            | 48             | 311                 | 121                 | 212                 | 139                 | 362            | 152            | 66             | 50             |
| 3rd Qu. | 137                 | 170                 | 130                 | 142                 | 195            | 157            | 112            | 213            | 107                 | 115                 | 193                 | 266            | 148            | 88             | 231                 | 92                  | 175                 | 185            | 81             | 14             | 129                 | 31                  | 69                  | 57                  | 151            | 50             | 32             | 22             |
| Max.    | 1332155             | 949659              | 1684886             | 2117410             | 2003292        | 1946182        | 1419028        | 2259068        | 1054556             | 1717036             | 1568931             | 1565851        | 905457         | 412651         | 1291992             | 831156              | 1215163             | 1677741        | 970559         | 599191         | 1722356             | 1405776             | 2071750             | 507645              | 1742480        | 1936451        | 312860         | 317225         |

Figure 1 shows the total number of mapped reads for each sample. Reads that map on multiple locations on the transcriptome are counted more than once, as far as they are mapped on less than 50 different loci. We expect total read counts to be similar within conditions, they may be different across conditions. Total counts sometimes vary widely between replicates. This may happen for several reasons, including:

* Different rRNA contamination levels between samples (even between biological replicates);
* Slight differences between library concentrations, since they may be difficult to measure with high precision.

<figure><img src=".gitbook/assets/image (4).png" alt="" width="563"><figcaption><p>Figure 1: Number of mapped reads per sample. Colors refer to the biological condition of the sample.</p></figcaption></figure>



Figure 2 shows the proportion of features with no read count in each sample. We expect this proportion to be similar within conditions. Features with null read counts in the 28 samples will not be taken into account for the analysis with edgeR. Here, 1717 features (4.12%) are in this situation (dashed line).

<figure><img src=".gitbook/assets/image (1) (1).png" alt="" width="563"><figcaption><p>Figure 2: Proportion of features with null read counts in each sample.</p></figcaption></figure>



<figure><img src=".gitbook/assets/image (2) (1).png" alt="" width="563"><figcaption><p>Figure 3: Density distribution of read counts.</p></figcaption></figure>

Figure 3 shows the distribution of read counts for each sample. For sake of readability, \\(\text{log}\_2(\text{counts}+1)\\) are used instead of raw counts. Again we expect replicates to have similar distributions. In addition, this figure shows if read counts are preferably low, medium or high. This depends on the organisms as well as the biological conditions under consideration.



It may happen that one or a few features capture a high proportion of reads (up to 20% or more). This phenomenon should not influence the normalization process. The edgeR normalization has proved to be robust to this situation \[Dillies, 2012]. Anyway, we expect these high count features to be the same across replicates. They are not necessarily the same across conditions. Figure 4 illustrate the possible presence of such high-count features in the data set.

<figure><img src=".gitbook/assets/image (3) (1).png" alt=""><figcaption><p>Figure 4: Percentage of reads associated with the sequence having the highest count (provided in each box on the graph) for each sample.</p></figcaption></figure>



We may wish to assess the similarity between samples across conditions. A pairwise scatter plot is produced (figure 5) to show how replicates and samples from different biological conditions are similar or different (\\(\text{log}\_2(\text{counts}+1)\\) are used instead of raw count values). Moreover, as the Pearson correlation has been shown not to be relevant to measure the similarity between replicates, the SERE statistic has been proposed as a similarity index between RNA-Seq samples \[Schulze, 2012]. It measures whether the variability between samples is random Poisson variability or higher. Pairwise SERE values are printed in the lower triangle of the pairwise scatter plot. The value of the SERE statistic is:

* 0 when samples are identical (no variability at all: this may happen in the case of a sample duplication);
* 1 for technical replicates (technical variability follows a Poisson distribution);
* greater than 1 for biological replicates and samples from different biological conditions (biological variability is higher than technical one, data are over-dispersed with respect to Poisson). The higher the SERE value, the lower the similarity. It is expected to be lower between biological replicates than between samples of different biological conditions. Hence, the SERE statistic can be used to detect inversions between samples.

<figure><img src=".gitbook/assets/image (4) (1).png" alt=""><figcaption><p>Figure 5: Pairwise comparison of samples.</p></figcaption></figure>



***

### 3 Filtering low counts

edgeR suggests to filter features with null or low counts because they do not supply much information. For this project, 17902 features (42.96%) have been removed from the analysis because they did not satisfy the following condition: having at least 1 counts-per-million in at least 6 samples.

***

### 4 Variability within the experiment: data exploration

The main variability within the experiment is expected to come from biological differences between the samples. This can be checked in three ways. The first one is to perform a hierarchical clustering of the whole sample set. This is performed after a transformation of the count data as moderated log-counts-per-million. Figure 6 shows the dendrogram obtained from CPM data. An euclidean distance is computed between samples, and the dendrogram is built upon the Ward criterion. We expect this dendrogram to group replicates and separate biological conditions.

<figure><img src=".gitbook/assets/image (5).png" alt="" width="563"><figcaption><p>Figure 6: Sample clustering based on normalized data.</p></figcaption></figure>



The second method of visaulizing the experiment variability is to look at the heatmaps of the two conditions as show on figure 7. On this figure the x-axis represents the two conditions (along with the replicates) and the y-axis represent the top 20 genes with the top variance over all samples.

<figure><img src=".gitbook/assets/image (7).png" alt="" width="563"><figcaption><p>Figure 7: Heatmap based on normalized data.</p></figcaption></figure>



Another way of visualizing the experiment variability is to look at the first two dimensions of a multidimensional scaling plot, as shown on figure 8. On this figure, the first dimension is expected to separate samples from the different biological conditions, meaning that the biological variability is the main source of variance in the data.

<figure><img src=".gitbook/assets/image (8).png" alt="" width="563"><figcaption><p>Figure 8: Multidimensional scaling plot of the samples.</p></figcaption></figure>



***

### 5 Normalization

Normalization aims at correcting systematic technical biases in the data, in order to make read counts comparable across samples. The normalization proposed by edgeR is called Trimmed Mean of M-values (TMM) but it is also possible to use the RLE (DESeq) or upperquartile normalizations. It relies on the hypothesis that most features are not differentially expressed.

edgeR computes a factor for each sample. These normalization factors apply to the total number of counts and cannot be used to normalize read counts in a direct manner. Indeed, normalization factors are used to normalize total counts. These in turn are used to normalize read counts according to a total count normalization: if \\(N\_j\\) is the total number of reads of the sample \\(j\\) and \\(f\_j\\) its normalization factor, \\(N'\_j=f\_j \times N\_j\\) is the normalized total number of reads. Then, let \\(s\_j=N'\_j/\bar{N'}\\) with \\(\bar{N'}\\) the mean of the \\(N'\_j\\) s. Finally, the normalized counts of the sample \\(j\\) are defined as \\(x'\_{ij}=x\_{ij}/s\_j\\) where \\(i\\) is the gene index.

|                           | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   | 12   | 13   | 14   | 15   | 16   | 17   | 18   | 19   | 20   | 21   | 22   | 23   | 24   | 25   | 26   | 27   | 28   |
| ------------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| TMM normalization factors | 1.09 | 1.20 | 1.04 | 1.08 | 1.16 | 1.10 | 0.98 | 1.10 | 1.02 | 0.93 | 1.09 | 1.25 | 1.32 | 1.15 | 1.10 | 0.96 | 1.07 | 1.16 | 0.96 | 0.67 | 0.91 | 0.58 | 0.74 | 0.91 | 0.95 | 0.77 | 1.09 | 1.04 |

Boxplots are often used to assess the quality of the normalization process, as they show how distributions are globally affected during this process. We expect normalization to stabilize distributions across samples. Figure 9 shows boxplots of raw (left) and normalized (right) data respectively.

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption><p>Figure 9: Boxplots of raw (left) and normalized (right) read counts.</p></figcaption></figure>



***

### 6 Differential analysis

#### 6.1 Modelization

edgeR aims at fitting one linear model per feature. For this project, the design used is \~ Genotype and the goal is to estimate the models' coefficients which can be interpreted as \\(\log\_2(\texttt{FC})\\). These coefficients will then be tested to get p-values and adjusted p-values.

#### 6.2 Dispersions estimation

The edgeR model assumes that the count data follow a negative binomial distribution which is a robust alternative to the Poisson law when data are over-dispersed (the variance is higher than the mean). The first step of the statistical procedure is to estimate the dispersion of the data.

<figure><img src=".gitbook/assets/image (10).png" alt="" width="563"><figcaption><p>Figure 10: Dispersion estimates.</p></figcaption></figure>

Figure 10 shows the result of the dispersion estimation step. The x- and y-axes represent the mean count value and the estimated dispersion respectively. Black dots represent empirical dispersion estimates for each feature (from the observed count values). The blue curve shows the relationship between the means of the counts and the dispersions modeled with splines. The red segment represents the common dispersion.

#### 6.3 Statistical test for differential gene expression

Once the dispersion estimation and the model fitting have been done, edgeR can perform the statistical testing. Figure 11 shows the distributions of raw p-values computed by the statistical test for the comparison(s) done. This distribution is expected to be a mixture of a uniform distribution on \\(\[0,1]\\) and a peak around 0 corresponding to the differentially expressed features.

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption><p>Figure 11: Distribution(s) of raw p-values.</p></figcaption></figure>



#### 6.4 Final results

A p-value adjustment is performed to take into account multiple testing and control the false positive rate to a chosen level \\(\alpha\\). For this analysis, a BH p-value adjustment was performed \[Benjamini, 1995 and 2001] and the level of controlled false positive rate was set to 0.05.

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption><p>Figure 12: MA-plot(s) of each comparison. Red dots represent significantly differentially expressed features.</p></figcaption></figure>

Figure 12 represents the MA-plot of the data for the comparisons done, where differentially expressed features are highlighted in red. A MA-plot represents the log ratio of differential expression as a function of the mean intensity for each feature. Triangles correspond to features having a too low/high \\(\log\_2(\text{FC})\\) to be displayed on the plot.

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption><p>Figure 13: Volcano plot(s) of each comparison. Red dots represent significantly differentially expressed features.</p></figcaption></figure>

Figure 13 shows the volcano plots for the comparisons performed and differentially expressed features are still highlighted in red. A volcano plot represents the log of the adjusted P value as a function of the log ratio of differential expression.



Full results as well as lists of differentially expressed features are provided in the following text files which can be easily read in a spreadsheet. For each comparison:

* TestVsRef.complete.txt contains results for all the features;
* TestVsRef.up.txt contains results for up-regulated features. Features are ordered from the most significant adjusted p-value to the less significant one;
* TestVsRef.down.txt contains results for down-regulated features. Features are ordered from the most significant adjusted p-value to the less significant one.

These files contain the following columns:

* Id: unique feature identifier;
* sampleName: raw counts per sample;
* norm.sampleName: rounded normalized counts per sample;
* baseMean: base mean over all samples;
* Col, Cvi, Ler and WS2: means (rounded) of normalized counts of the biological conditions;
* FoldChange: fold change of expression, calculated as \\(2^{\log\_2(\text{FC})}\\);
* log2FoldChange: \\(\log\_2(\text{FC})\\) as estimated by the GLM model. It reflects the differential expression between Test and Ref and can be interpreted as \\(\log\_2(\frac{\text{Test\}}{\text{Ref\}})\\). If this value is:
  * around 0: the feature expression is similar in both conditions;
  * positive: the feature is up-regulated (\\(\text{Test} > \text{Ref}\\));
  * negative: the feature is down-regulated (\\(\text{Test} < \text{Ref}\\));
* pvalue: raw p-value from the statistical test;
* padj: adjusted p-value on which the cut-off \\(\alpha\\) is applied;
* tagwise.dispersion: dispersion parameter estimated from feature counts (i.e. black dots on figure 9);
* trended.dispersion: dispersion parameter estimated with splines (i.e. blue curve on figure 9).

***

### 7 R session information and parameters

The versions of the R software and Bioconductor packages used for this analysis are listed below. It is important to save them if one wants to re-perform the analysis in the same conditions.

* R version 3.3.1 RC (2016-06-14 r70789), x86\_64-pc-linux-gnu\

* Locale: LC\_CTYPE=en\_US.UTF-8, LC\_NUMERIC=C, LC\_TIME=en\_US.UTF-8, LC\_COLLATE=en\_US.UTF-8, LC\_MONETARY=en\_US.UTF-8, LC\_MESSAGES=en\_US.UTF-8, LC\_PAPER=en\_US.UTF-8, LC\_NAME=C, LC\_ADDRESS=C, LC\_TELEPHONE=C, LC\_MEASUREMENT=en\_US.UTF-8, LC\_IDENTIFICATION=C\

* Base packages: base, datasets, graphics, grDevices, methods, parallel, stats, stats4, utils\

* Other packages: Biobase 2.32.0, BiocGenerics 0.18.0, DESeq2 1.12.3, devtools 1.12.0, edgeR 3.14.0, genefilter 1.54.2, GenomeInfoDb 1.8.1, GenomicRanges 1.24.2, getopt 1.20.0, IRanges 2.6.1, knitr 1.13, limma 3.28.11, RColorBrewer 1.1-2, S4Vectors 0.10.1, SARTools 1.3.2, SummarizedExperiment 1.2.3, xtable 1.8-2\

* Loaded via a namespace (and not attached): acepack 1.3-3.3, annotate 1.50.0, AnnotationDbi 1.34.3, BiocParallel 1.6.2, chron 2.3-47, cluster 2.0.2, codetools 0.2-14, colorspace 1.2-6, data.table 1.9.6, DBI 0.4-1, digest 0.6.9, evaluate 0.9, foreign 0.8-65, formatR 1.4, Formula 1.2-1, geneplotter 1.50.0, ggplot2 2.1.0, grid 3.3.1, gridExtra 2.2.1, gtable 0.2.0, Hmisc 3.17-4, lattice 0.20-33, latticeExtra 0.6-28, locfit 1.5-9.1, magrittr 1.5, memoise 1.0.0, munsell 0.4.3, nnet 7.3-10, plyr 1.8.4, Rcpp 0.12.5, rpart 4.1-10, RSQLite 1.0.0, scales 0.4.0, splines 3.3.1, stringi 0.5-5, stringr 1.0.0, survival 2.38-3, tools 3.3.1, withr 1.0.2, XML 3.98-1.4, XVector 0.12.0, zlibbioc 1.18.0

#### Parameter values used for this analysis are:

* projectName: GeneLab Ecotypes in space
* author: Richard Barker
* targetFile: DRB\_RNAseq\_counts\_2018\_Ecotypes\_BRIC19\_targets\_v5.txt
* rawDir:
* featuresToRemove: alignment\_not\_unique
* varInt: Genotype
* condRef: Col
* batch: NULL
* alpha: 0.05
* pAdjustMethod: BH
* cpmCutoff: 1
* gene.selection: pairwise
* colors: dodgerblue
* normalizationMethod: TMM
* OutDir:

***

### 8 Bibliography

* R Core Team, **R: A Language and Environment for Statistical Computing**, _R Foundation for Statistical Computing_, 2014
* Gentleman, Carey, Bates et al, **Bioconductor: Open software development for computational biology and bioinformatics**, _Genome Biology_, 2004
* Robinson and Smyth, **Moderated statistical tests for assessing differences in tag abundance**, _Bioinformatics_, 2007
* Robinson and Smyth, **Small-sample estimation of negative binomial dispersion, with applications to SAGE data**, _Biostatistics_, 2008
* Robinson, McCarthy and Smyth, **edgeR: a Bioconductor package for differential expression analysis of digital gene expression data**, _Bioinformatics_, 2010
* Dillies, Rau, Aubert et al, **A comprehensive evaluation of normalization methods for Illumina RNA-seq data analysis**, _Briefings in Bioinformatics_, 2012
* Schulze, Kanwar, Golzenleuchter et al, **SERE: Single-parameter quality control and sample comparison for RNA-Seq**, _BMC Genomics_, 2012
* McCarthy, Chen and Smyth, **Differential expression analysis of multifactor RNA-Seq experiments with respect to biological variation**, _Nucleic Acids Research_, 2012
* Wu, Wang and Wu, **A new shrinkage estimator for dispersion improves differential expression detection in RNA-seq data**, _Biostatistics_, 2013
* Benjamini and Hochberg, **Controlling the False Discovery Rate : A Practical and Powerful Approach to Multiple Testing**, _Journal of the Royal Statistical Society_, 1995
* Benjamini and Yekutieli, **The control of the false discovery rate in multiple testing under dependency**, _The Annals of Statistics_, 2001
