---
name: geo-rnaseq
description: Download and analyze bulk RNA-seq data from GEO (Geoquery/GEOparse) with DESeq2/edgeR.
---

# GEO Bulk RNA-seq Pipeline

## Workflow
1. **Download**: Use `GEOquery` (R) or `GEOparse` (Python).
   - R: `getGEO(GSE_ID, AnnotGPL=TRUE)`
   - Python: `GEOparse.get_GEO(GSE_ID)`
2. **Preprocess**:
   - Filter low-count genes (e.g., <10 reads in >50% samples).
   - Normalize: TPM/FPKM (for viz) or raw counts (for DESeq2).
3. **Differential Expression**:
   - DESeq2: `DESeqDataSetFromMatrix` → `DESeq` → `results`.
   - Threshold: |log2FC| > 1, FDR < 0.05.
4. **Visualization**:
   - PCA (top 500 variable genes).
   - Volcano plot, heatmap (top DEGs).
5. **Enrichment**: Go to `enrichment-go-kegg` skill.

## Code Snippets (R)
