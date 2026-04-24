# Single-Cell-RNA-Seq-Analysis-of-Breast-Cancer
Identifying stress-response and survival gene programs across molecular subtypes using scRNA-seq
Biological Question

What cell populations exist within breast cancer tumors across molecular subtypes, and do stress-response and survival gene programs differ between these populations — with implications for therapeutic resistance?


Dataset
GSE176078 — Wu et al., 2021, Nature Genetics
FeatureDetailsTotal cells100,064Total genes29,733SubtypesTNBC (42,512) · ER+ (38,241) · HER2+ (19,311)Cell types9 major types including Cancer Epithelial, T-cells, Myeloid, CAFs, B-cellsSourceGEO: GSE176078

Pipeline
Raw MTX Files
     │
     ▼
Stage 1 — Data Loading
  └─ scipy sparse matrix → AnnData object + metadata

Stage 2 — Quality Control
  └─ nFeature > 200 · nCount < 37,000 · no mito% cutoff (biology-justified)
  └─ 97,710 cells retained (2.4% removed)

Stage 3 — Normalisation & Feature Selection
  └─ Normalise to 10,000 counts · log1p transform · top 2,000 HVGs

Stage 4 — Scaling & PCA
  └─ Scale (mean=0, std=1, max=10) · PCA 50 components · 15 PCs selected

Stage 5 — Neighbourhood Graph & UMAP
  └─ sc.pp.neighbors (n_pcs=15) · UMAP visualisation

Stage 6 — Clustering
  └─ Leiden algorithm (resolution=0.5) → 27 clusters

Stage 7 — Marker Gene Identification
  └─ Wilcoxon rank-sum test on Cancer Epithelial subset (n=22,824)

Stage 8 — Biological Interpretation
  └─ Stress-response dotplot · Violin plots by subtype · Subtype-cluster mapping

Key Findings
Cancer Epithelial Sub-populations
Leiden clustering identified 10 Cancer Epithelial sub-clusters, revealing substantial intratumor heterogeneity.
ClusterDominant SubtypeKey MarkersBiological Identity9TNBC (98%)ATG5, PKM, IDH2, CD24Autophagy / metabolic stress17TNBC (98%)HSPA1A, HSPA1BHeat shock / stress response16ER+ (99%)ESR1Estrogen receptor+ luminal cells7ER+ (88%)SCGB2A2, HSPA1ALuminal / heat shock22, 23TNBC (100%)S100A8, S100A9, KRT17Basal-like TNBC11MixedNormal + Cancer EpithelialTransitional cell states
Subtype-Specific Stress-Response Programs
TNBC and ER+ Cancer Epithelial cells use fundamentally different survival strategies:

TNBC → Autophagy (ATG5) + Metabolic reprogramming (PKM, IDH2)
ER+ → Heat shock protein response (HSPA1A, HSPA1B)
HER2+ → Intermediate heat shock expression

Therapeutic Implications
TNBC-enriched Clusters 9 and 17 activate survival machinery (ATG5, PKM) that may confer chemotherapy resistance. These genes represent potential therapeutic targets to sensitise TNBC to existing treatment regimens.

Notable Methodological Decisions
QC — No mito% cutoff applied
Cancer Epithelial cells showed a median mito% of 8.71% (vs 5.15% dataset-wide). A standard 10% cutoff would have removed ~50% of the primary cells of interest. Evidence-based, biology-first QC.
Transitional cell states identified
Cluster 11 contains equal numbers of Cancer Epithelial (1,852) and Normal Epithelial (1,852) cells — quantitative evidence for cells at intermediate stages of malignant transformation.

Tools & Environment
ToolVersion / NotesPython3.13.5ScanpyscRNA-seq analysis frameworkscipySparse matrix loadingleidenalgLeiden clusteringpandas / matplotlib / seabornData handling and visualisationEnvironmentJupyter Notebook in VSCodePCA solversvd_solver='randomized' (RAM-constrained local run)

Repository Structure
├── scrnaseq_analysis.ipynb    # Full analysis notebook
├── figures/
│   ├── umap_celltype_major.png
│   ├── umap_cancer_subtype.png
│   ├── dotplot_stress_genes.png
│   └── violin_stress_by_subtype.png
└── README.md

Reference
Wu, S.Z., Al-Eryani, G., Roden, D.L. et al. A single-cell and spatially resolved atlas of human breast cancers. Nature Genetics 53, 1334–1347 (2021). https://doi.org/10.1038/s41588-021-00911-1

Part of a self-directed bioinformatics portfolio — transitioning (MSc Medical Biology & Genetics) to computational biology.
