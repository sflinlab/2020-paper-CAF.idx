
title: "R codes to generate Figure 4b"
author: "sufang"
date: "6/16/2020"

---
* executed on R 4.0.0 (2020-04-24)
* X86_64-w64-mingw32/64 (64-bits)
* The orginial Seurat source code is available at (https://github.com/satijalab/seurat)
* expression matrix of GSE103322 was downloaded from (https://cells.ucsc.edu/?ds=head-neck)
_________________

##### clean-up exprs matrix and build up metadata -----

```{r}
# metadata 
GSE103322.data = read.table("GSE103322_HNSCC_all_data.txt", sep = '\t', header=TRUE, row.names = 1)

# extract first 5 rows for metadata
GSE103322.metadata = t(GSE103322.data[1:5,])
colnames(GSE103322.metadata) = c("maximaEnzyme", "lymphNode", "cancerCell", "nonCancerCell", "cellType")
GSE103322.metadata$cell_type[GSE103322.metadata$cell_type == 0] <- "Tumor"
GSE103322.metadata$cell_type[GSE103322.metadata$cell_type == -Fibroblast] <- "Fibroblast"
GSE103322.metadata <- GSE103322.metadata %>% mutate(lymphNode = ifelse(lymphNode == 0,"No", "Yes"))
GSE103322.metadata <- GSE103322.metadata %>% mutate(cancerCell = ifelse(cancerCell == 0,"No", "Yes"))
GSE103322.metadata <- GSE103322.metadata %>% mutate(non_cancer_Cell = ifelse(non_cancer_Cell == 0,"No", "Yes"))

# exprs matrix
GSE103322.data = as.sparse(GSE103322.data[-c(1:5),]) # remove first 5 rows
min(GSE103322.data)  # [1] 0
max(GSE103322.data) #[1] 16.109

GSE103322.data = as.sparse(2^GSE103322.data-1) # anti-log2 (reverse log2)
dim(GSE103322.data) # 23686 5902
min(GSE103322.data)  # [1] 0
max(GSE103322.data) #[1] 70678.29
```


##### Create Seurat Object ----------

```{r}
library(Seurat)

Puram <- CreateSeuratObject(counts = GSE103322.data, project = "GSE103322_Puram_5902_2017", meta.data = GSE103322.metadata)

```

##### Comply Satija's PMBC tutorial to normalize and scale data ---------
*** 1. the selection and filtration of cells based on QC metrics
```{r mito, fig.height=7, fig.width=13}
# The [[ operator can add columns to object metadata. This is a great place to stash QC stats
Puram[["percent.mt"]] <- PercentageFeatureSet(object = Puram, pattern = "^MT-")

```

*** 2. data normalization and scaling
```{r normalize}
# it seems 1e+03 matches the original value the best 

Puram <- NormalizeData(object = Puram, normalization.method = "LogNormalize", scale.factor = 1e+03)
```

*** 3. the detection of highly variable features
```{r regress, fig.height=7, fig.width=11, results='hide'}
all.genes <- rownames(x = Puram)
Puram <- ScaleData(object = Puram, features = all.genes)
```
*** remove unwanted sources of variation, as in Seurat v2

```{r regressvarmt, fig.height=7, fig.width=11, results='hide',eval = FALSE}
Puram <- ScaleData(object = Puram, vars.to.regress = 'percent.mt') 

```

*** save the normalized Seurat object for future use
```{r}
saveRDS(Puram, file = "Puram.rds")
```

*** DotPlot for Figure 4b
```{r fig.height=4.5, fig.width=5}

feature.DotPlot <- c("TP63","CSF1R","ACTA2", "FAP", "TGFBR3", "TGFBR2", "TGFBR1", "TGFBI", "TGFB3", "TGFB2", "TGFB1", "FN1")

Fig = DotPlot(Puram, features = feature.DotPlot, group.by = "cellType", cols = c("whitesmoke", "mediumblue"), col.min = 0, col.max = 5, dot.min = 0 , dot.scale = 6) + RotatedAxis()+ xlab("") + ylab("") + theme(axis.text = element_text(size = 10), legend.title = element_text(size = 8), legend.text = element_text(size = 8), legend.position = "top", legend.box = "vertical")   
 
ggsave("For paper new Fig4b.png", height = 4.5, width = 5)

```

![](https://i.imgur.com/3qPhTLt.jpg)
