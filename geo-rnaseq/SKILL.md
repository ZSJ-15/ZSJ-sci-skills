---
name: geo-rnaseq
description: Standard bulk RNA-seq pipeline for GEO datasets (R/Bioconductor). Download, preprocess, DESeq2, visualization, and enrichment linkage.
---

# GEO Bulk RNA-seq Pipeline

## Workflow

1. Download: Use GEOquery (R) or GEOparse (Python).
   - R: getGEO(GSE_ID, AnnotGPL=TRUE)
   - Python: GEOparse.get_GEO(GSE_ID)
2. Preprocess:
   - Filter low-count genes (e.g., less than 10 reads in more than 50 percent of samples).
   - Normalize: TPM or FPKM for visualization, or raw counts for DESeq2.
3. Differential Expression:
   - DESeq2: DESeqDataSetFromMatrix then DESeq then results.
   - Threshold: absolute log2FC greater than 1, FDR less than 0.05.
4. Visualization:
   - PCA using top 500 variable genes.
   - Volcano plot and heatmap for top DEGs.
5. Enrichment: Go to enrichment-go-kegg skill.

## Code Snippets R

# Step 1: Download
library(GEOquery)
gse <- getGEO("GSEXXXXX", AnnotGPL = TRUE)[[1]]
exprs <- exprs(gse)
pdata <- pData(gse)

# Step 2: Preprocess
keep <- rowSums(exprs > 10) > 0.5 * ncol(exprs)
exprs_filt <- exprs[keep, ]

# Step 3: DESeq2
library(DESeq2)
coldata <- data.frame(condition = pdata$characteristics_ch1)
dds <- DESeqDataSetFromMatrix(countData = exprs_filt, colData = coldata, design = ~ condition)
dds <- DESeq(dds)
res <- results(dds, alpha = 0.05)
deg <- subset(res, abs(log2FoldChange) > 1 & padj < 0.05)

# Step 4: Visualization
vsd <- vst(dds, blind = FALSE)
plotPCA(vsd, intgroup = "condition")

## Constraints

- Use raw counts for DESeq2, not TPM or FPKM.
- Verify GEO ID and platform annotation GPL.
- Consider ComBat for batch effects.
- Link to enrichment-go-kegg for downstream pathway analysis.
