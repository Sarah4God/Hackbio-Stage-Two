**Single-Cell RNA-seq Analysis Pipeline: Tissue Inference, Cell Type Annotation, and Immune Profiling (MICAIAH ADEOLUWA ADEDEJI)**

This repository contains a complete single-cell RNA-sequencing analysis workflow used to identify cell types, infer tissue origin, and characterize immune status of an unlabelled human dataset. The analysis follows standard single-cell best practices using **Scanpy**, including preprocessing, dimensionality reduction, clustering, marker-based annotation, and lineage-specific interpretation.

All improvements requested in the project feedback - including clearer methodology, reproducibility instructions, expanded tissue justification, statistical caveats, and detailed documentation - have been incorporated.

**1\. Objectives**

- Identify major immune and hematopoietic cell populations from the dataset.
- Characterize lineage-specific roles of each population.
- Determine whether the tissue source is **bone marrow** or **peripheral blood**.
- Assess whether cell type proportions indicate **healthy** or **infected** status.
- Provide a transparent and reproducible computational workflow.

**2\. Data Source and Input Format**

The dataset is a single-cell gene expression matrix in standard AnnData format (.h5ad).  
It contains UMI-count gene expression for human cells.  
Cell metadata and raw counts were used directly for preprocessing.

If your dataset is different, update data_path in the notebook.

**3\. Environment Setup**

**Install Dependencies**

pip install scanpy==1.10.1 anndata==0.10.5 matplotlib seaborn leidenalg

**Expected Runtime**

- QC + normalization: ~1-3 minutes
- PCA + neighbors: ~1 minute
- Clustering + UMAP: ~1-2 minutes
- Marker detection (rank_genes_groups): ~1 minute

Times vary by hardware.

**4\. Analysis Workflow**

**4.1 Preprocessing**

Steps performed:

- **Quality control**
  - Filter cells by min counts
  - Filter genes by min expression
  - Compute mitochondrial percentage
- **Normalization** to 10,000 counts/cell
- **Log1p transformation**
- **Highly variable gene selection**
- **Scaling**
- **PCA dimensionality reduction**

All preprocessing is applied before clustering to ensure meaningful structure detection.

**4.2 Clustering**

- Nearest-neighbor graph construction using PCA components
- Leiden clustering (resolution parameter tuned to detect 10 distinct populations)
- UMAP visualization for interpretability

Clusters used for final annotation: **0 through 9 (10 total)**.

**5\. Cell Type Annotation**

Cell identity assignment was based on:

- Top markers from sc.tl.rank_genes_groups (log1p-normalized).
- Known canonical immune markers.
- Manual review of lineage signatures.

**Final Identified Cell Types**

| **Cluster** | **Cell Type** |
| --- | --- |
| 0   | Hematopoietic Stem/Progenitor Cells (HSPCs) |
| 1   | Classical Monocytes |
| 2   | Non-Classical / Intermediate Monocytes |
| 3   | Natural Killer (NK) Cells |
| 4   | Naïve B Cells |
| 5   | T Cells (Mixed CD4/CD8) |
| 6   | Megakaryocyte / Platelet Lineage |
| 7   | Plasma Cells |
| 8   | Erythroid Precursors |
| 9   | ILC-like / Nuocyte Population |

These 10 identities match the project requirement to list every cell type detected.

**6\. Biological Roles of Identified Cell Types**

Each cell type's core function:

- **HSPCs:** blood-forming progenitors
- **Classical Monocytes:** phagocytosis, inflammation
- **Non-Classical Monocytes:** vascular patrol, antiviral sensing
- **NK Cells:** cytotoxic killing of infected/transformed cells
- **Naïve B Cells:** antigen-inexperienced adaptive precursors
- **T Cells:** adaptive immunity, cytokine coordination, cytotoxicity
- **Megakaryocytes:** platelet production, hemostasis
- **Plasma Cells:** antibody secretion
- **Erythroid Precursors:** hemoglobin-producing developing RBCs
- **ILC/Nuocytes:** type-2 immunity, cytokine responses

**7\. Tissue Source Inference: PBMC vs Bone Marrow**

**Evidence Supporting Peripheral Blood**

- Dominance of T, B, NK, and monocyte populations typical of PBMC.
- Absence of granulocyte maturation stages characteristic of bone marrow.
- Small erythroid and megakaryocyte fractions consistent with PBMC.

**Contradiction Noted**

The HSPC cluster is unusually large for PBMC.  
Possible explanations:

- Donor mobilization (e.g., G-CSF)
- Sorting/enrichment of progenitors
- Doublet or ambient RNA artifacts
- Misannotation due to ENSG symbol mapping gaps

**Final Tissue Conclusion**

**The dataset most closely represents PBMC (peripheral blood mononuclear cells), likely with progenitor enrichment. Bone marrow identity is unlikely given the missing granulocytic stages.**

**8\. Health or Infection Status Assessment**

**Monocyte Fraction (~29%)**

Moderately elevated; possible mild inflammation.

**NK Cells (~8.4%)**

Within normal range; no strong cytotoxic activation detected.

**Lymphocyte Levels (~31%)**

Not depleted - inconsistent with acute viral infection.

**Plasma Cells**

Low abundance - argues against active infection.

**Final Health Conclusion**

**No strong evidence of acute infection.**  
Mild inflammatory signature possible due to monocyte elevation.

**9\. Statistical Considerations**

To improve interpretability:

- Proportions should be compared against **reference PBMC datasets**.
- Confidence intervals can be estimated via **bootstrap resampling** (if multiple samples exist).
- Differential expression should be computed on **log1p-normalized data**, not raw counts (the notebook now follows this).

**10\. Reproducibility Instructions**

**Run the notebook end-to-end:**

jupyter notebook analysis.ipynb

**Expected Outputs**

- UMAP plots
- Cluster proportions
- Marker-based annotation table
- Tissue source interpretation
- Final report summarizing findings

**11\. Known Limitations**

- No metadata to confirm mobilization or sorting.
- HSPC identity requires ENSG → symbol validation.
- Interpretation does not replace clinical evaluation.
