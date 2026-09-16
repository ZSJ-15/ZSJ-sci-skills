---
name: scanpy-singlecell
description: Standard single-cell RNA-seq analysis pipeline using Scanpy (Python).
---

# Scanpy Single-cell Pipeline

## Workflow
1. **Read**: `sc.read_10x_h5("filtered_feature_bc_matrix.h5")`
2. **QC**:
   - Filter cells: n_genes > 200, < 5000; pct_counts_mt < 10-15%.
   - `sc.pl.violin(adata, ['n_genes', 'total_counts', 'pct_counts_mt'])`
3. **Normalize**: `sc.pp.normalize_total(adata, target_sum=1e4)` → `sc.pp.log1p(adata)`
4. **HVG**: `sc.pp.highly_variable_genes(adata, n_top_genes=2000)`
5. **Scale & PCA**: `sc.pp.scale(adata)` → `sc.tl.pca(adata, n_comps=30)`
6. **Cluster**:
   - UMAP: `sc.tl.umap(adata)`
   - Leiden: `sc.tl.leiden(adata, resolution=0.5)`
7. **Marker genes**: `sc.tl.rank_genes_groups(adata, 'leiden')`
8. **Annotation**: Based on known markers (e.g., CD3D T-cell, MS4A1 B-cell).

## Visualization
- UMAP colored by cluster/cell type.
- Dotplot/violin for marker genes.
- Export: `sc.pl.umap(adata, color='cell_type', save='_celltype.pdf')`

## Constraints
- Doublet removal (Scrublet/DoubletFinder) recommended.
- Batch correction: `sc.pp.combat` or `scVI` for multiple samples.
- Save object: `adata.write('analysis.h5ad')`
