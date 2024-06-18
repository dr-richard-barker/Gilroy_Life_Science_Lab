# Example code for heatmap extraction from OSDR

#### Notebook for extracting biomarkers from GeneLab differential expression analysis datasets <a href="#notebook-for-extracting-frailty-biomarkers-from-genelab-differential-expression-analysis-datasets" id="notebook-for-extracting-frailty-biomarkers-from-genelab-differential-expression-analysis-datasets"></a>

1. **Loading libraries:** It loads the `tidyverse` package which offers a collection of tools for data manipulation and visualization. It also loads `data.table` for efficient data handling.
2. **Setting Working Directory:** It sets the working directory to "Meta\_analysis\_X" where presumably the data and analysis scripts reside.
3. **Downloading Data:** It defines URLs for example datasets and downloads them to your system. Note that these example datasets likely wouldn't be related to OSD-120 or Arabidopsis.
4. **Reading Biomarker Data:** It reads a file named "biomarkers\_multiple\_ids.txt" containing information on potential biomarkers.
5. **Preparing Analysis for Specific Datasets:** The code then demonstrates a loop that iterates through downloaded datasets (commented out). Inside the loop, it would likely:
   * Read a downloaded dataset.
   * Filter the data based on the Ensembl IDs from the biomarker data (assuming these datasets link genes with identifiers like Ensembl).
   * Select relevant columns like gene symbols, fold change, and p-values.

To analyze data related to for example: **OSD-120** and _Arabidopsis thaliana_, you would need to:

1. Replace the example data URLs with those containing data relevant to your research. This data should ideally link genes to identifiers like Ensembl IDs used in the biomarker data.
2. Modify the filtering step to target genes from Arabidopsis thaliana instead of the example "Human" or "Mouse" used in the code. This might involve using a different Ensembl identifier specific to Arabidopsis.
3. Depending on your specific goals, you might need additional code for further analysis like differential expression testing or visualization.

If you have the data related to OSD-120 and Arabidopsis thaliana with appropriate identifiers, you can adapt this code as a starting point for your analysis. It's recommended to consult relevant resources on differential expression analysis in R for Arabidopsis thaliana data.

**General AI example:**

Python

```
# Import libraries
import pandas as pd

# Set working directory (optional, uncomment if needed)
# !mkdir /content/Meta_analysis_X  # Create directory if it doesn't exist
# %cd /content/Meta_analysis_X  # Change directory

# Define example data URLs (replace with your actual data URLs)
urls = [
    "https://example.com/dataset1.csv",
    "https://example.com/dataset2.csv",
    "https://example.com/dataset3.csv"
]

# Define filenames for downloaded data (modify if needed)
filenames = ["dataset1.csv", "dataset2.csv", "dataset3.csv"]

# Download data (assuming data is in CSV format)
for url, filename in zip(urls, filenames):
  !wget {url} -O {filename}

# Read biomarker data (assuming data is in CSV format)
biomarkers_df = pd.read_csv("biomarkers_multiple_ids.txt", sep="\t")  # Adjust separator if needed

# Loop through downloaded data (modify logic for your specific data format)
for filename in filenames:
  data_df = pd.read_csv(filename)
  # Filter data based on Arabidopsis thaliana identifiers (replace logic as needed)
  arabidopsis_df = data_df[data_df["Species"] == "Arabidopsis thaliana"]
  # Select relevant columns (modify column names as needed)
  arabidopsis_df = arabidopsis_df[["Gene_ID", "Symbol", "Log2FoldChange", "pValue"]]
  
  # Perform further analysis on the Arabidopsis data (e.g., differential expression testing, visualization)
  # ... your analysis code here ...

  print(f"Analysis results for {filename}")
  # Print or visualize results from your analysis
  print(arabidopsis_df.head())  # Print the first few rows for example
```

But we also have this real example from friends who work on human data.



**Title: Was Project: xxx by  Author: yyy**

<pre><code><strong>#Load libraries
</strong><strong>library(tidyverse)
</strong>library(data.table)
</code></pre>

```
#Set working directory
setwd("Meta_analysis_X")
```

In \[10]:

#### ## change the links to represent the data you want.&#x20;

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

```
#Read in table of biomarkers
biomarkers<-fread("biomarkers_multiple_ids.txt",header=T)
head(biomarkers)
```



| Orig\_name | Ensembl\_Hsap   | Ensembl\_Mmus      | Symbol\_Mmus | Symbol\_Hsap |
| ---------- | --------------- | ------------------ | ------------ | ------------ |
| ACTN3      | ENSG00000248746 | ENSMUSG00000006457 | Actn3        | ACTN3        |
| mtDNA      | ENSG00000107815 | ENSMUSG00000025209 | Twnk         | TWNK         |

**Output DE metrics for overlapping genes between frailty biomarkers and GeneLab datasets**

```
#GLDS-91: RNA-Seq, Human (use Ensembl_Hsap to map biomarkers)
glds91<-fread("GLDS-91_rna_seq_differential_expression.csv",header=T) %>%
        filter(ENSEMBL %in% biomarkers$Ensembl_Hsap) %>%
        select(ENSEMBL,SYMBOL,GENENAME,`Log2fc_(suG)v(1G)`,`P.value_(suG)v(1G)`,
               `Adj.p.value_(suG)v(1G)`,`All.mean`,`Group.Mean_(1G)`,`Group.Mean_(suG)`)
glds91
```

```
#GLDS-52: Array, Human (use Ensembl_Hsap to map biomarkers)
glds52<-fread("GLDS-52_array_differential_expression.csv",header=T) %>%
        filter(ENSEMBL %in% biomarkers$Ensembl_Hsap) %>%
        select(ENSEMBL,SYMBOL,GENENAME,`Log2fc_(Space Flight)v(Ground Control)`,`P.value_(Space Flight)v(Ground Control)`,
               `Adj.p.value_(Space Flight)v(Ground Control)`,`All.mean`,`Group.Mean_(Space Flight)`,`Group.Mean_(Ground Control)`)
glds52
```

```
#GLDS-104: RNA-Seq, Mouse (use Ensembl_Mmus to map biomarkers)
glds104<-fread("GLDS-104_rna_seq_differential_expression.csv",header=T) %>%
        filter(ENSEMBL %in% biomarkers$Ensembl_Mmus) %>%
        select(ENSEMBL,SYMBOL,GENENAME,`Log2fc_(FLT)v(GC)`,`P.value_(FLT)v(GC)`,
               `Adj.p.value_(FLT)v(GC)`,`All.mean`,`Group.Mean_(GC)`,`Group.Mean_(GC)`)
glds104
```

```
sessionInfo()
R version 4.1.3 (2022-03-10)
Platform: x86_64-pc-linux-gnu (64-bit)
```

```
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





This version also shows how to plot the data as a heatmap.

```
# Import libraries
import pandas as pd
import matplotlib.pyplot as plt
from scipy.cluster.hierarchy import dendrogram, linkage

# Set working directory (optional, uncomment if needed)
# !mkdir /content/Meta_analysis_X  # Create directory if it doesn't exist
# %cd /content/Meta_analysis_X  # Change directory

# Define example data URLs (replace with your actual data URLs)
urls = [
    "https://example.com/dataset1.csv",
    "https://example.com/dataset2.csv",
    "https://example.com/dataset3.csv"
]

# Define filenames for downloaded data (modify if needed)
filenames = ["dataset1.csv", "dataset2.csv", "dataset3.csv"]

# Download data (assuming data is in CSV format)
for url, filename in zip(urls, filenames):
  !wget {url} -O {filename}

# Read biomarker data (assuming data is in CSV format)
biomarkers_df = pd.read_csv("biomarkers_multiple_ids.txt", sep="\t")  # Adjust separator if needed

# Loop through downloaded data (modify logic for your specific data format)
for filename in filenames:
  data_df = pd.read_csv(filename)
  # Filter data based on Arabidopsis thaliana identifiers (replace logic as needed)
  arabidopsis_df = data_df[data_df["Species"] == "Arabidopsis thaliana"]
  # Select relevant columns (modify column names as needed)
  expression_data = arabidopsis_df[["Gene_ID", "Symbol"]]  # Separate expression data
  replicate_columns = [col for col in arabidopsis_df.columns if col not in expression_data.columns]
  expression_df = arabidopsis_df[replicate_columns]

  # Normalize expression data (e.g., Z-score normalization)
  normalized_df = (expression_df - expression_df.mean()) / expression_df.std()

  # Perform hierarchical clustering on normalized data
  distance_matrix = linkage(normalized_df, method='ward')

  # Generate heatmap with dendrogram
  plt.figure(figsize=(10, 6))
  plt.imshow(normalized_df, cmap='RdBu_r')  # Adjust colormap as needed
  plt.colorbar(label='Normalized Expression')

  # Add dendrogram
  dendrogram(distance_matrix, orientation='top')
  plt.xticks(rotation=45, ha='right')  # Rotate x-axis labels for better readability
  plt.xlabel("Samples")
  plt.ylabel("Genes (Normalized Expression)")
  plt.title(f"Heatmap with Dendrogram for {filename}")
  plt.tight_layout()
  plt.show()

  # Further analysis on the Arabidopsis data (e.g., differential expression testing)
  # ... your analysis code here ...

  print(f"Analysis results for {filename}")
```



\
