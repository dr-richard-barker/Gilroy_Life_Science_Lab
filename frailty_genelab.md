# frailty\_genelab

#### Notebook for extracting Frailty biomarkers from GeneLab differential expression analysis datasets <a href="#notebook-for-extracting-frailty-biomarkers-from-genelab-differential-expression-analysis-datasets" id="notebook-for-extracting-frailty-biomarkers-from-genelab-differential-expression-analysis-datasets"></a>

Project: AWG Frailty Project

Author: Prachi Kothiyal

In \[34]:

```
#Load libraries
library(tidyverse)
library(data.table)
```

In \[5]:

```
#Set working directory
setwd("frailty")
```

In \[10]:

```
#Define URLs and destination files for downloading RNA-seq/microarray DEG datasets
URLs<-c("https://genelab-data.ndc.nasa.gov/genelab/static/media/dataset/GLDS-91_rna_seq_differential_expression.csv?version=1",
        "https://genelab-data.ndc.nasa.gov/genelab/static/media/dataset/GLDS-52_array_differential_expression.csv?version=1",
        "https://genelab-data.ndc.nasa.gov/genelab/static/media/dataset/GLDS-104_rna_seq_differential_expression.csv?version=1"
        )
destFiles<-c("GLDS-91_rna_seq_differential_expression.csv",
             "GLDS-52_array_differential_expression.csv",
             "GLDS-104_rna_seq_differential_expression.csv")

#Download files from URLs and save them to destination files
for (i in 1:length(URLs)){
        download.file(URLs[i], destFiles[i])
    }
```

In \[35]:

```
#Read in frailty biomarkers
biomarkers<-fread("biomarkers_multiple_ids.txt",header=T)
head(biomarkers)
```

| Orig\_name | Ensembl\_Hsap   | Ensembl\_Mmus      | Symbol\_Mmus | Symbol\_Hsap |
| ---------- | --------------- | ------------------ | ------------ | ------------ |
| \<chr>     | \<chr>          | \<chr>             | \<chr>       | \<chr>       |
| ACE        | ENSG00000159640 | ENSMUSG00000020681 | Ace          | ACE          |
| ACTN3      | ENSG00000248746 | ENSMUSG00000006457 | Actn3        | ACTN3        |
| CNTF       | ENSG00000242689 | ENSMUSG00000079415 | Cntf         | CNTF         |
| GDF8       | ENSG00000138379 | ENSMUSG00000026100 | Mstn         | MSTN         |
| IL6        | ENSG00000136244 | ENSMUSG00000025746 | Il6          | IL6          |
| mtDNA      | ENSG00000107815 | ENSMUSG00000025209 | Twnk         | TWNK         |

**Output DE metrics for overlapping genes between frailty biomarkers and GeneLab datasets**

In \[25]:

```
#GLDS-91: RNA-Seq, Human (use Ensembl_Hsap to map biomarkers)
glds91<-fread("GLDS-91_rna_seq_differential_expression.csv",header=T) %>%
        filter(ENSEMBL %in% biomarkers$Ensembl_Hsap) %>%
        select(ENSEMBL,SYMBOL,GENENAME,`Log2fc_(suG)v(1G)`,`P.value_(suG)v(1G)`,
               `Adj.p.value_(suG)v(1G)`,`All.mean`,`Group.Mean_(1G)`,`Group.Mean_(suG)`)
glds91
```

| ENSEMBL         | SYMBOL   | GENENAME                                             | Log2fc\_(suG)v(1G) | P.value\_(suG)v(1G) | Adj.p.value\_(suG)v(1G) | All.mean   | Group.Mean\_(1G) | Group.Mean\_(suG) |
| --------------- | -------- | ---------------------------------------------------- | ------------------ | ------------------- | ----------------------- | ---------- | ---------------- | ----------------- |
| \<chr>          | \<chr>   | \<chr>                                               | \<dbl>             | \<dbl>              | \<dbl>                  | \<dbl>     | \<chr>           | \<chr>            |
| ENSG00000096717 | SIRT1    | sirtuin 1                                            | 0.199574010        | 0.06599123          | 0.9999287               | 2155.43460 | 2006.846         | 2304.023          |
| ENSG00000100644 | HIF1A    | hypoxia inducible factor 1 subunit alpha             | -0.000137081       | 0.99876801          | 0.9999287               | 4438.56242 | 4438.767         | 4438.358          |
| ENSG00000101384 | JAG1     | jagged canonical Notch ligand 1                      | 0.083423669        | 0.68195421          | 0.9999287               | 734.29356  | 714.1282         | 754.4589          |
| ENSG00000107815 | TWNK     | twinkle mtDNA helicase                               | 0.013037142        | 0.90077607          | 0.9999287               | 2311.65608 | 2301.451         | 2321.861          |
| ENSG00000109819 | PPARGC1A | PPARG coactivator 1 alpha                            | 0.840837321        | 0.23152945          | 0.9999287               | 26.75166   | 19.41884         | 34.08449          |
| ENSG00000111424 | VDR      | vitamin D receptor                                   | 0.007172624        | 0.99072297          | 0.9999287               | 34.00289   | 33.56280         | 34.44298          |
| ENSG00000112096 | SOD2     | superoxide dismutase 2                               | 0.087705570        | 0.43750540          | 0.9999287               | 2362.45730 | 2290.793         | 2434.121          |
| ENSG00000131981 | LGALS3   | galectin 3                                           | -0.227021406       | 0.01824179          | 0.7575282               | 5976.96118 | 6445.851         | 5508.072          |
| ENSG00000141510 | TP53     | tumor protein p53                                    | -0.039990436       | 0.65544285          | 0.9999287               | 5493.37330 | 5569.739         | 5417.008          |
| ENSG00000142208 | AKT1     | AKT serine/threonine kinase 1                        | 0.032176780        | 0.69615182          | 0.9999287               | 6904.49006 | 6827.548         | 6981.432          |
| ENSG00000198793 | MTOR     | mechanistic target of rapamycin kinase               | -0.020623720       | 0.82135315          | 0.9999287               | 5320.50407 | 5358.207         | 5282.801          |
| ENSG00000204305 | AGER     | advanced glycosylation end-product specific receptor | 0.040746564        | 0.83298698          | 0.9999287               | 358.87361  | 353.8236         | 363.9236          |
| ENSG00000242689 | CNTF     | ciliary neurotrophic factor                          | -0.047661882       | 0.91700091          | 0.9999287               | 61.32093   | 62.35320         | 60.28867          |

In \[27]:

```
#GLDS-52: Array, Human (use Ensembl_Hsap to map biomarkers)
glds52<-fread("GLDS-52_array_differential_expression.csv",header=T) %>%
        filter(ENSEMBL %in% biomarkers$Ensembl_Hsap) %>%
        select(ENSEMBL,SYMBOL,GENENAME,`Log2fc_(Space Flight)v(Ground Control)`,`P.value_(Space Flight)v(Ground Control)`,
               `Adj.p.value_(Space Flight)v(Ground Control)`,`All.mean`,`Group.Mean_(Space Flight)`,`Group.Mean_(Ground Control)`)
glds52
```

| ENSEMBL         | SYMBOL   | GENENAME                                     | Log2fc\_(Space Flight)v(Ground Control) | P.value\_(Space Flight)v(Ground Control) | Adj.p.value\_(Space Flight)v(Ground Control) | All.mean  | Group.Mean\_(Space Flight) | Group.Mean\_(Ground Control) |
| --------------- | -------- | -------------------------------------------- | --------------------------------------- | ---------------------------------------- | -------------------------------------------- | --------- | -------------------------- | ---------------------------- |
| \<chr>          | \<chr>   | \<chr>                                       | \<dbl>                                  | \<dbl>                                   | \<dbl>                                       | \<dbl>    | \<dbl>                     | \<dbl>                       |
| ENSG00000198793 | MTOR     | mechanistic target of rapamycin kinase       | 0.267793450                             | 0.16142200                               | 0.6075246                                    | 8.979825  | 9.113721                   | 8.845928                     |
| ENSG00000135744 | AGT      | angiotensinogen                              | -0.361001512                            | 0.06075150                               | 0.5038879                                    | 5.305773  | 5.125272                   | 5.486273                     |
| ENSG00000096717 | SIRT1    | sirtuin 1                                    | -0.031833602                            | 0.85640341                               | 0.9636808                                    | 8.460936  | 8.445019                   | 8.476853                     |
| ENSG00000107815 | TWNK     | twinkle mtDNA helicase                       | -0.074301343                            | 0.63644159                               | 0.8889291                                    | 6.612529  | 6.575379                   | 6.649680                     |
| ENSG00000248746 | ACTN3    | actinin alpha 3 (gene/pseudogene)            | -0.231763314                            | 0.20701069                               | 0.6493380                                    | 5.712231  | 5.596350                   | 5.828113                     |
| ENSG00000167779 | IGFBP6   | insulin like growth factor binding protein 6 | -0.013213502                            | 0.95030730                               | 0.9888423                                    | 9.950135  | 9.943528                   | 9.956742                     |
| ENSG00000111424 | VDR      | vitamin D receptor                           | 0.087755817                             | 0.62549496                               | 0.8862717                                    | 8.649621  | 8.693499                   | 8.605743                     |
| ENSG00000131981 | LGALS3   | galectin 3                                   | -0.438803568                            | 0.01677747                               | 0.4039667                                    | 7.592845  | 7.373443                   | 7.812247                     |
| ENSG00000100644 | HIF1A    | hypoxia inducible factor 1 subunit alpha     | 0.039776983                             | 0.82954697                               | 0.9542073                                    | 11.128879 | 11.148767                  | 11.108990                    |
| ENSG00000142208 | AKT1     | AKT serine/threonine kinase 1                | 0.358695147                             | 0.04082732                               | 0.4647857                                    | 9.829856  | 10.009204                  | 9.650509                     |
| ENSG00000172156 | CCL11    | C-C motif chemokine ligand 11                | -0.323944533                            | 0.10307619                               | 0.5568854                                    | 5.889406  | 5.727434                   | 6.051379                     |
| ENSG00000159640 | ACE      | angiotensin I converting enzyme              | -0.152431689                            | 0.33791414                               | 0.7394997                                    | 5.487047  | 5.410832                   | 5.563263                     |
| ENSG00000141510 | TP53     | tumor protein p53                            | 0.140666390                             | 0.43790845                               | 0.7986803                                    | 8.438301  | 8.508634                   | 8.367967                     |
| ENSG00000138379 | MSTN     | myostatin                                    | 0.030124562                             | 0.89552744                               | 0.9753748                                    | 3.400007  | 3.415069                   | 3.384945                     |
| ENSG00000101384 | JAG1     | jagged 1                                     | -0.096992447                            | 0.55169803                               | 0.8548793                                    | 7.703468  | 7.654972                   | 7.751964                     |
| ENSG00000109819 | PPARGC1A | PPARG coactivator 1 alpha                    | -0.001183764                            | 0.99427274                               | 0.9981448                                    | 4.685303  | 4.684712                   | 4.685895                     |
| ENSG00000038427 | VCAN     | versican                                     | 0.052618342                             | 0.77710327                               | 0.9356322                                    | 10.528576 | 10.554885                  | 10.502267                    |
| ENSG00000112096 | SOD2     | superoxide dismutase 2                       | -0.164123901                            | 0.30200182                               | 0.7173642                                    | 12.130779 | 12.048718                  | 12.212841                    |
| ENSG00000136244 | IL6      | interleukin 6                                | -0.396772036                            | 0.08666436                               | 0.5392735                                    | 12.681270 | 12.482884                  | 12.879656                    |
| ENSG00000164867 | NOS3     | nitric oxide synthase 3                      | -0.211760323                            | 0.26568815                               | 0.6873585                                    | 6.145768  | 6.039888                   | 6.251649                     |

In \[32]:

```
#GLDS-104: RNA-Seq, Mouse (use Ensembl_Mmus to map biomarkers)
glds104<-fread("GLDS-104_rna_seq_differential_expression.csv",header=T) %>%
        filter(ENSEMBL %in% biomarkers$Ensembl_Mmus) %>%
        select(ENSEMBL,SYMBOL,GENENAME,`Log2fc_(FLT)v(GC)`,`P.value_(FLT)v(GC)`,
               `Adj.p.value_(FLT)v(GC)`,`All.mean`,`Group.Mean_(GC)`,`Group.Mean_(GC)`)
glds104
```

| ENSEMBL            | SYMBOL   | GENENAME                                                                | Log2fc\_(FLT)v(GC) | P.value\_(FLT)v(GC) | Adj.p.value\_(FLT)v(GC) | All.mean     | Group.Mean\_(GC) |
| ------------------ | -------- | ----------------------------------------------------------------------- | ------------------ | ------------------- | ----------------------- | ------------ | ---------------- |
| \<chr>             | \<chr>   | \<chr>                                                                  | \<dbl>             | \<dbl>              | \<dbl>                  | \<dbl>       | \<chr>           |
| ENSMUSG00000001729 | Akt1     | thymoma viral proto-oncogene 1                                          | -0.15120851        | 6.510060e-03        | 2.910469e-02            | 4676.474660  | 4921.207         |
| ENSMUSG00000006457 | Actn3    | actinin alpha 3                                                         | 1.42699617         | 1.729493e-10        | 4.790096e-09            | 43205.182102 | 23425.06         |
| ENSMUSG00000006818 | Sod2     | superoxide dismutase 2, mitochondrial                                   | 0.05208492         | 3.756666e-01        | 5.797757e-01            | 19527.849682 | 19176.13         |
| ENSMUSG00000015452 | Ager     | advanced glycosylation end product-specific receptor                    | 0.97037511         | 2.462439e-01        | NA                      | 3.189281     | 2.473816         |
| ENSMUSG00000020063 | Sirt1    | sirtuin 1                                                               | -0.14890456        | 3.049729e-02        | 9.835285e-02            | 757.251892   | 795.9599         |
| ENSMUSG00000020676 | Ccl11    | chemokine (C-C motif) ligand 11                                         | -0.18985848        | 9.055864e-02        | 2.213271e-01            | 418.159256   | 445.3817         |
| ENSMUSG00000020681 | Ace      | angiotensin I converting enzyme (peptidyl-dipeptidase A) 1              | 0.28257622         | 2.195515e-03        | 1.194415e-02            | 2112.690682  | 1906.482         |
| ENSMUSG00000021109 | Hif1a    | hypoxia inducible factor 1, alpha subunit                               | -0.28193091        | 2.127509e-07        | 3.464387e-06            | 1492.128195  | 1637.628         |
| ENSMUSG00000021614 | Vcan     | versican                                                                | -0.41367318        | 1.029325e-01        | 2.424486e-01            | 656.518483   | 749.9442         |
| ENSMUSG00000022479 | Vdr      | vitamin D (1,25-dihydroxyvitamin D3) receptor                           | 0.16532745         | 7.219136e-01        | 8.479569e-01            | 30.654267    | 28.90707         |
| ENSMUSG00000023046 | Igfbp6   | insulin-like growth factor binding protein 6                            | -0.04980936        | 6.459580e-01        | 7.979605e-01            | 1253.810866  | 1275.230         |
| ENSMUSG00000025209 | Twnk     | twinkle mtDNA helicase                                                  | -0.05291038        | 2.605371e-01        | 4.602686e-01            | 1557.036713  | 1585.774         |
| ENSMUSG00000025746 | Il6      | interleukin 6                                                           | -0.88200986        | 4.716321e-01        | NA                      | 2.492713     | 2.915508         |
| ENSMUSG00000026100 | Mstn     | myostatin                                                               | 0.87997934         | 3.630373e-04        | 2.621693e-03            | 160.694885   | 113.5097         |
| ENSMUSG00000027276 | Jag1     | jagged 1                                                                | -0.04807585        | 6.544758e-01        | 8.042058e-01            | 732.653710   | 745.2180         |
| ENSMUSG00000028978 | Nos3     | nitric oxide synthase 3, endothelial cell                               | -0.07928170        | 4.184706e-01        | 6.197673e-01            | 653.637895   | 671.7760         |
| ENSMUSG00000028991 | Mtor     | mechanistic target of rapamycin kinase                                  | -0.22479776        | 2.941392e-04        | 2.192716e-03            | 5223.873358  | 5630.251         |
| ENSMUSG00000029167 | Ppargc1a | peroxisome proliferative activated receptor, gamma, coactivator 1 alpha | -0.35539634        | 2.445877e-02        | 8.324930e-02            | 3129.884729  | 3512.884         |
| ENSMUSG00000031980 | Agt      | angiotensinogen (serpin peptidase inhibitor, clade A, member 8)         | 0.95476540         | 2.119508e-23        | 2.744763e-21            | 339.461378   | 231.8211         |
| ENSMUSG00000050335 | Lgals3   | lectin, galactose binding, soluble 3                                    | 0.08500287         | 5.656844e-01        | 7.400090e-01            | 122.813159   | 119.3328         |
| ENSMUSG00000059552 | Trp53    | transformation related protein 53                                       | 0.21602452         | 1.032822e-02        | 4.234238e-02            | 364.436362   | 337.6122         |
| ENSMUSG00000079415 | Cntf     | ciliary neurotrophic factor                                             | 0.21189127         | 5.250045e-01        | 7.090979e-01            | 99.076565    | 91.84797         |

In \[31]:

```
sessionInfo()
```

```
R version 4.1.3 (2022-03-10)
Platform: x86_64-pc-linux-gnu (64-bit)
Running under: Debian GNU/Linux 10 (buster)

Matrix products: default
BLAS:   /usr/lib/x86_64-linux-gnu/openblas/libblas.so.3
LAPACK: /usr/lib/x86_64-linux-gnu/libopenblasp-r0.3.5.so

locale:
 [1] LC_CTYPE=C.UTF-8       LC_NUMERIC=C           LC_TIME=C.UTF-8       
 [4] LC_COLLATE=C.UTF-8     LC_MONETARY=C.UTF-8    LC_MESSAGES=C.UTF-8   
 [7] LC_PAPER=C.UTF-8       LC_NAME=C              LC_ADDRESS=C          
[10] LC_TELEPHONE=C         LC_MEASUREMENT=C.UTF-8 LC_IDENTIFICATION=C   

attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
 [1] data.table_1.14.2 forcats_0.5.1     stringr_1.4.0     dplyr_1.0.8      
 [5] purrr_0.3.4       readr_2.1.2       tidyr_1.2.0       tibble_3.1.6     
 [9] tidyverse_1.3.1   ggplot2_3.3.5    

loaded via a namespace (and not attached):
 [1] pbdZMQ_0.3-7     tidyselect_1.1.2 repr_1.1.4       haven_2.4.3     
 [5] colorspace_2.0-3 vctrs_0.4.0      generics_0.1.2   htmltools_0.5.2 
 [9] base64enc_0.1-3  utf8_1.2.2       rlang_1.0.2      pillar_1.7.0    
[13] glue_1.6.2       withr_2.5.0      DBI_1.1.2        dbplyr_2.1.1    
[17] modelr_0.1.8     readxl_1.3.1     uuid_1.0-3       lifecycle_1.0.1 
[21] munsell_0.5.0    gtable_0.3.0     cellranger_1.1.0 rvest_1.0.2     
[25] evaluate_0.15    tzdb_0.2.0       fastmap_1.1.0    fansi_1.0.3     
[29] broom_0.7.12     IRdisplay_1.1    Rcpp_1.0.8.3     scales_1.1.1    
[33] backports_1.4.1  IRkernel_1.3     jsonlite_1.8.0   fs_1.5.2        
[37] hms_1.1.1        digest_0.6.29    stringi_1.7.6    grid_4.1.3      
[41] cli_3.2.0        tools_4.1.3      magrittr_2.0.3   crayon_1.5.1    
[45] pkgconfig_2.0.3  ellipsis_0.3.2   xml2_1.3.3       reprex_2.0.1    
[49] lubridate_1.8.0  rstudioapi_0.13  assertthat_0.2.1 httr_1.4.2      
[53] R6_2.5.1         compiler_4.1.3  
```

In \[ ]:\


```
R version 4.1.3 (2022-03-10)
Platform: x86_64-pc-linux-gnu (64-bit)
Running under: Debian GNU/Linux 10 (buster)

Matrix products: default
BLAS:   /usr/lib/x86_64-linux-gnu/openblas/libblas.so.3
LAPACK: /usr/lib/x86_64-linux-gnu/libopenblasp-r0.3.5.so

locale:
 [1] LC_CTYPE=C.UTF-8       LC_NUMERIC=C           LC_TIME=C.UTF-8       
 [4] LC_COLLATE=C.UTF-8     LC_MONETARY=C.UTF-8    LC_MESSAGES=C.UTF-8   
 [7] LC_PAPER=C.UTF-8       LC_NAME=C              LC_ADDRESS=C          
[10] LC_TELEPHONE=C         LC_MEASUREMENT=C.UTF-8 LC_IDENTIFICATION=C   

attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
 [1] data.table_1.14.2 forcats_0.5.1     stringr_1.4.0     dplyr_1.0.8      
 [5] purrr_0.3.4       readr_2.1.2       tidyr_1.2.0       tibble_3.1.6     
 [9] tidyverse_1.3.1   ggplot2_3.3.5    

loaded via a namespace (and not attached):
 [1] pbdZMQ_0.3-7     tidyselect_1.1.2 repr_1.1.4       haven_2.4.3     
 [5] colorspace_2.0-3 vctrs_0.4.0      generics_0.1.2   htmltools_0.5.2 
 [9] base64enc_0.1-3  utf8_1.2.2       rlang_1.0.2      pillar_1.7.0    
[13] glue_1.6.2       withr_2.5.0      DBI_1.1.2        dbplyr_2.1.1    
[17] modelr_0.1.8     readxl_1.3.1     uuid_1.0-3       lifecycle_1.0.1 
[21] munsell_0.5.0    gtable_0.3.0     cellranger_1.1.0 rvest_1.0.2     
[25] evaluate_0.15    tzdb_0.2.0       fastmap_1.1.0    fansi_1.0.3     
[29] broom_0.7.12     IRdisplay_1.1    Rcpp_1.0.8.3     scales_1.1.1    
[33] backports_1.4.1  IRkernel_1.3     jsonlite_1.8.0   fs_1.5.2        
[37] hms_1.1.1        digest_0.6.29    stringi_1.7.6    grid_4.1.3      
[41] cli_3.2.0        tools_4.1.3      magrittr_2.0.3   crayon_1.5.1    
[45] pkgconfig_2.0.3  ellipsis_0.3.2   xml2_1.3.3       reprex_2.0.1    
[49] lubridate_1.8.0  rstudioapi_0.13  assertthat_0.2.1 httr_1.4.2      
[53] R6_2.5.1         compiler_4.1.3  
```
