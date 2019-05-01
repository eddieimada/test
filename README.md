---
---

# Description
This repository hosts all the code used in the FC-R2 paper by Imada et al. 2018.

All data necessary to reproduce these analysis can be obtained in https://jhubiostatistics.shinyapps.io/recount/

## How to use

Data is available as a RangeSummarizedObject (RSE) which can be loaded in R enviroment using the SummarizedExperiment package.
Raw data is available as overall base coverage at gene level.

Data can be used as is, or scaled by the number of mapped reads and read length or AUC to get read-counts like data. For more information about processing data see:

*Collado-Torres, Leonardo, Abhinav Nellore, and Andrew E. Jaffe. "recount workflow: Accessing over 70,000 human RNA-seq samples with Bioconductor." F1000Research 6 (2017).*

```{r}
### Load required libraries
library(SummarizedExperiment)
library(recount)

### Load data
data <- "path/to/data"
load(data)

### Scale data
scaled_data <- scale_counts(rse, by="auc")
```

For huge datasets (e.g. TCGA or GTEX) you might want to subset only data relevant to your work (e.g. cancer type) to reduce the amount of computational resource used.

```{r}
### Load required libraries
library(SummarizedExperiment)
library(recount)

### Load TCGA data
load(TCGA.rda)

### Extract counts matrix
rseCounts <- assays(TCGA)$counts
rownames(rseCounts) <- rowRanges(rse_scaled)$ID

### Creating list of pheno data splited by cancer type
pheno <- as.data.frame(colData(TCGA))
phenoList <- split(pheno,pheno$gdc_cases.project.project_id)

### Check number of samples and projects available
table(pheno$gdc_cases.project.project_id)

### Create expression sets for a cancer type
x <- "TCGA_BRCA"
exp <- matrix[,rownames(phenoList[[x]])]
pheno <- phenoList[[x]]
eset <- ExpressionSet(assayData <- exp,
                      phenoData = AnnotatedDataFrame(pheno),
                      featureData= AnnotatedDataFrame(fdata))
```

## Citation
If you used any of the code or dataset available please cite:
