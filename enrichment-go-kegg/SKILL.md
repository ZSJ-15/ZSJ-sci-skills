---
name: enrichment-go-kegg
description: Perform GO and KEGG pathway enrichment analysis (clusterProfiler in R or gseapy/Enrichr in Python), with publication-ready barplots, dotplots, and clear reporting standards.
---

# GO and KEGG Enrichment Analysis

## Inputs
- A gene list: differentially expressed genes (for example, absolute log2FC greater than 1 and FDR less than 0.05) or single-cell marker genes.
- Background: all expressed genes, or the full genome/transcriptome for the species.
- Species: human, mouse, rat, etc. (must match the input data).

## Workflow (R, clusterProfiler)

# 1. Convert gene symbols to Entrez IDs
library(clusterProfiler)
library(org.Hs.eg.db)

gene_df <- bitr(genes,
                fromType = "SYMBOL",
                toType   = "ENTREZID",
                OrgDb    = "org.Hs.eg.db")

# 2. GO enrichment (Biological Process, Molecular Function, Cellular Component)
ego <- enrichGO(gene         = gene_df$ENTREZID,
                OrgDb        = "org.Hs.eg.db",
                ont          = "ALL",
                pAdjustMethod = "BH",
                pvalueCutoff  = 0.05,
                qvalueCutoff  = 0.05)

# 3. KEGG pathway enrichment (human = hsa)
kegg <- enrichKEGG(gene         = gene_df$ENTREZID,
                    organism     = "hsa",
                    pvalueCutoff = 0.05)

# 4. Visualization
barplot(ego, showCategory = 20)
dotplot(ego, showCategory = 20)

## Workflow (Python, gseapy)

from gseapy import enrichr, barplot, dotplot

# genes is a list of gene symbols
enr = enrichr(gene_list=genes,
              gene_sets=['GO_Biological_Process_2023',
                         'KEGG_2021_Human'],
              organism='Human')

enr.results.to_csv("enrichment_results.csv", index=False)
barplot(enr.res2d, title='GO/KEGG Enrichment')
dotplot(enr.res2d, title='GO/KEGG Enrichment')

## Outputs
- enrichment_results.csv (term, p-value, adjusted p-value, gene count, gene list)
- barplot.pdf or barplot.png (top 20 terms)
- dotplot.pdf or dotplot.png (gene ratio versus adjusted p-value)

## Rules and Constraints
- Use the correct species database: org.Hs.eg.db for human, org.Mm.eg.db for mouse, org.Rn.eg.db for rat.
- Report adjusted p-values (FDR / q-value), not raw p-values.
- Define the background gene set explicitly; do not use an arbitrary list.
- Avoid over-interpreting very broad terms such as "metabolic process" or "regulation of biological process".
- Cross-check GO and KEGG results: prioritize pathways supported by both methods.
- Do not claim causality from enrichment results alone; enrichment indicates association, not mechanism.

## Linking to Other Skills
- Input gene lists come from geo-rnaseq (bulk differential expression) or scanpy-singlecell (cluster markers).
- Visualization style should follow nature-figure (font, colorblind-friendly palette, panels, resolution).
