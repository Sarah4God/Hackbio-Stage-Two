**README: Single-Cell RNA-Seq Analysis of Bone Marrow**

**Project Overview**

This project performs a **single-cell RNA sequencing (scRNA-seq) analysis** on human bone marrow cells to identify cell types, quantify their relative abundances, and characterize lineage-specific gene expression. The analysis uses **Scanpy** for preprocessing, clustering, and dimensionality reduction, and **Decoupler** to map marker genes to cell types.

The workflow covers:

- Dataset download and loading
- Quality control and preprocessing
- Dimensionality reduction and clustering
- Cell type annotation via lineage markers
- Cluster abundance and marker gene analysis

**Dataset**

- **Source:** [Bone marrow scRNA-seq dataset on GitHub](https://github.com/josoga2/sc/raw/refs/heads/main/bone_marrow.h5ad)
- **Format:** .h5ad (AnnData object)
- **Organism:** Human

**Dependencies**

The notebook uses the following Python packages:

- scanpy (for scRNA-seq preprocessing and analysis)
- decoupler (for cell type marker analysis)
- igraph and leidenalg (for clustering)
- pandas, numpy (for data handling)

Install packages if not present:

pip install scanpy decoupler python-igraph leidenalg pandas numpy

**Pipeline Overview**

**1\. Dataset Download & Loading**

- Downloads the .h5ad file if not already present.
- Loads it into an **AnnData object** for analysis.

**2\. Quality Control (QC) & Preprocessing**

- **Filter cells and genes:** minimum 200 genes per cell, 3 cells per gene.
- **Mitochondrial content filter:** removes cells with >10% mitochondrial gene expression.
- **Normalization & log-transformation:** scales counts to 10,000 per cell, then log-transforms.
- **Highly variable genes:** selects top 2,000 genes for downstream analysis.
- **Scaling:** scales gene expression to unit variance.

**3\. Dimensionality Reduction & Clustering**

- **PCA** reduces dimensions to 40 principal components.
- **Neighborhood graph** constructed using 10 nearest neighbors.
- **UMAP** visualizes cells in 2D space.
- **Leiden clustering** identifies clusters with resolution 0.5.
- **UMAP plot** is saved as umap_clusters.png.

**4\. Marker Gene Mapping**

- Downloads **PanglaoDB markers** and maps gene symbols to **Ensembl IDs** via Biomart.
- Prepares a **cluster × marker gene table** for downstream annotation.

**5\. Cluster Analysis**

- **Cluster proportions:** relative abundance of each cluster.
- **Top 8 marker genes per cluster:** identifies defining genes for each cluster.
- **Lineage marker expression:** averages key genes per cluster to assign cell types.

Key lineage markers include:

- **Stem/progenitor cells:** CD34, MKI67
- **Erythroid lineage:** GATA1, KLF1, HBB
- **Myeloid lineage:** MPO, ELANE, CSF1R, IL1B
- **T cells:** CD3D, CD4, CD8A
- **B cells:** MS4A1, CD19
- **Plasma cells:** SDC1
- **Platelets/megakaryocytes:** PF4, PPBP
- **NK cells/cytotoxic lymphocytes:** GZMB, PRF1, NKG7

**How to Run the Notebook**

- Clone the repository or download the .ipynb notebook.
- Install dependencies:

pip install scanpy decoupler python-igraph leidenalg pandas numpy

- Run the notebook cells **sequentially**:
  - QC & preprocessing
  - Dimensionality reduction
  - Clustering
  - Marker analysis
- Outputs:
  - bone_marrow_processed.h5ad → preprocessed dataset
  - umap_clusters.png → UMAP visualization of clusters
  - Terminal prints → cluster proportions, top marker genes, lineage marker expression

**Key Findings**

- **Clusters identified:** multiple clusters representing hematopoietic stem cells, erythroid, myeloid, lymphoid, and platelet lineages.
- **Cluster proportions:** reflect typical bone marrow composition with myeloid and erythroid dominance.
- **Lineage markers:** confirm cluster identities, e.g., CD34+ progenitors, MPO/ELANE+ neutrophils, CD3/CD8 T cells, MS4A1+ B cells.
- **Health status:** relative abundances are within normal ranges (can be used to infer infection/immune activation).
- **Tissue source:** cell types and proportions match bone marrow expectations; presence of progenitors supports BM origin.

**Notes**

- This analysis is **reproducible** for any human scRNA-seq bone marrow dataset in .h5ad format.
- All code is **modular**, so you can swap datasets or markers.
- Marker gene expression tables can be used for **manual annotation** or automated label transfer.
