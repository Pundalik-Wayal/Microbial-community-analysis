# 🦠 HITChip Gut Microbiome Analysis Pipeline

[![R](https://img.shields.io/badge/R-4.5.1-276DC3?style=flat&logo=r&logoColor=white)](https://www.r-project.org/)
[![Bioconductor](https://img.shields.io/badge/Bioconductor-3.22-87B13F?style=flat)](https://www.bioconductor.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![DOI](https://img.shields.io/badge/DOI-10.1038%2Fncomms5344-blue)](https://doi.org/10.1038/ncomms5344)
[![Status](https://img.shields.io/badge/Pipeline-Complete-brightgreen)](/)

> Reproducible gut microbiome analysis pipeline using HITChip data (Lahti et al. 2014, *Nature Communications*) — 1,006 western adults, 130 genera. Covers alpha/beta diversity, core microbiome, taxonomic composition, longitudinal LMM, and batch effect assessment. Built with R, phyloseq & microbiome packages.

---

## 📋 Table of Contents

- [Dataset](#-dataset)
- [Repository Structure](#-repository-structure)
- [Installation](#-installation)
- [Usage](#-usage)
- [Analysis Pipeline](#-analysis-pipeline)
- [Key Results](#-key-results)
- [Figures](#-figures)
- [Packages](#-packages)
- [Citation](#-citation)
- [Contact](#-contact)

---

## 📊 Dataset

| Property | Detail |
|---|---|
| **Study** | Tipping elements in the human intestinal ecosystem |
| **Reference** | Lahti L et al. *Nat Commun* **5**:4344 (2014) |
| **Technology** | HITChip phylogenetic microarray |
| **Samples** | 1,172 total · 1,006 baseline |
| **Subjects** | 1,006 western adults |
| **Taxa** | 130 genus-level groups |
| **Longitudinal** | 78 subjects with ≥ 2 time points (up to 8.3 months) |

<details>
<summary><b>Metadata variables (click to expand)</b></summary>

| Variable | Description | Range / Levels |
|---|---|---|
| Age | Years (integer, rounded) | 18 – 77 |
| Sex | Biological sex | female (n = 680), male (n = 455) |
| Nationality | Geographic region | CentralEurope · Scandinavia · SouthEurope · UKIE · US · EasternEurope |
| BMI_group | Standard BMI classification | underweight · lean · overweight · obese · severeobese · morbidobese · superobese |
| DNA_extraction_method | Library prep method | o (other) · p · r (Repeated Bead Beating) |
| ProjectID | Aggregated study of origin | 40 projects |
| Diversity | Shannon index (probe-level) | 4.7 – 6.35 |
| SubjectID | Subject identifier | — |
| Time | Months from baseline | 0 – 8.3 months |

</details>

---

## 📁 Repository Structure

```
HITChip-Microbiome-Pipeline/
│
├── HITChip_Microbiome_Pipeline_FIXED.R    # Main pipeline — 13 sections
├── README.md
├── LICENSE
├── .gitignore
│
├── data/
│   ├── HITChip.tab                        # Abundance matrix (1172 × 130)
│   ├── Metadata.tab                       # Sample metadata  (1172 × 9)
│   ├── README_for_HITChip_tab.txt
│   └── README_for_Metadata_tab.txt
│
├── figures/                               # Output plots (300 DPI PNG)
│   ├── 01_alpha_diversity.png
│   ├── 02_beta_diversity.png
│   ├── 03_core_microbiome.png
│   ├── 04_taxonomic_composition.png
│   ├── 05_group_comparisons.png
│   ├── 06_heatmap.png
│   ├── 07_longitudinal.png
│   ├── 08_associations.png
│   └── 09_batch_effect.png
│
└── results/
    └── summary_statistics.csv
```

---

## ⚙️ Installation

**Requirements:** R ≥ 4.1.0 · RStudio (recommended) · Windows / macOS / Linux

```r
# Step 1 — Bioconductor packages
if (!requireNamespace("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

BiocManager::install(c(
  "microbiome", "phyloseq", "DESeq2", "limma", "DirichletMultinomial"
))

# Step 2 — CRAN packages
install.packages(c(
  "tidyverse", "vegan", "ggplot2", "pheatmap", "RColorBrewer",
  "ggpubr", "scales", "reshape2", "patchwork",
  "lme4", "lmerTest", "ggrepel", "viridis"
))
```

> 💡 **Windows users:** Download and install [Rtools](https://cran.rstudio.com/bin/windows/Rtools/) before running the above.

---

## 🚀 Usage

```bash
# 1. Clone the repository
git clone https://github.com/YOUR-USERNAME/HITChip-Microbiome-Pipeline.git
cd HITChip-Microbiome-Pipeline

# 2. Place HITChip.tab and Metadata.tab inside data/

# 3. Run the full pipeline
Rscript HITChip_Microbiome_Pipeline_FIXED.R
```

> The script automatically creates `figures/` and `results/` directories.  
> Run section-by-section in **RStudio** for interactive exploration.

---

## 🔬 Analysis Pipeline

| # | Section | Method | Output |
|---|---|---|---|
| 0 | Package setup | Install + load all libraries | Console |
| 1 | Data loading | `read.table()` → phyloseq object | Console |
| 2 | Preprocessing | Compositional transform · prevalence filter · baseline subset | Console |
| 3 | **Alpha diversity** | Shannon · Evenness · Dominance · Kruskal-Wallis | `01_alpha_diversity.png` |
| 4 | **Beta diversity** | PCoA · NMDS (Bray-Curtis) · PERMANOVA | `02_beta_diversity.png` |
| 5 | **Core microbiome** | Blanket analysis · prevalence–abundance scatter | `03_core_microbiome.png` |
| 6 | **Taxonomic composition** | Stacked barplots by nationality & BMI | `04_taxonomic_composition.png` |
| 7 | **Statistical comparisons** | Kruskal-Wallis per taxon · BH-FDR · PERMANOVA | `05_group_comparisons.png` |
| 8 | **Heatmap** | Z-scored log abundance · Ward.D2 clustering | `06_heatmap.png` |
| 9 | **Longitudinal analysis** | LMM: Shannon ~ Time + Age + Sex + (1\|SubjectID) | `07_longitudinal.png` |
| 10 | **Continuous associations** | Pearson r (age) · Spearman ρ (taxon vs diversity) | `08_associations.png` |
| 11 | **Batch effect** | PERMANOVA: community ~ DNA extraction method | `09_batch_effect.png` |
| 12 | Summary table | All results compiled | `summary_statistics.csv` |
| 13 | Session info | Reproducibility record | Console |

---

## 📈 Key Results

### 🔹 Alpha Diversity
| Comparison | Test | Result |
|---|---|---|
| Shannon ~ BMI group | Kruskal-Wallis | χ² = 41.38, **p = 7.9 × 10⁻⁸** |
| Shannon ~ Sex | Wilcoxon | **p = 0.039** (males slightly higher) |
| Pielou evenness ~ Nationality | Visual | US lowest · EasternEurope highest |

### 🔹 Beta Diversity
- PCoA Axis 1 = **27.3%** · Axis 2 = **19.7%** of total Bray-Curtis variance
- Clear nationality-driven clustering with 95% confidence ellipses
- NMDS stress = **0.201** (PCoA preferred for interpretation)
- PERMANOVA confirms significant nationality and BMI effects

### 🔹 Core Microbiome
- **56 core taxa** at > 50% prevalence and > 0.1% relative abundance
- Most prevalent (> 94%): *Faecalibacterium prausnitzii*, *Ruminococcus obeum*, *Oscillospira guillermondii*

### 🔹 Top Differentially Abundant Taxa (BMI groups, BH-FDR corrected)

| Rank | Taxon | KW χ² | FDR p |
|---|---|---|---|
| 1 | *Bifidobacterium* | 97.6 | 2.5 × 10⁻¹⁸ |
| 2 | *Ruminococcus obeum* et rel. | 77.0 | 2.7 × 10⁻¹⁴ |
| 3 | *Coprococcus eutactus* et rel. | 58.7 | 1.1 × 10⁻¹⁰ |
| 4 | *Subdoligranulum variable* at rel. | 42.1 | 2.1 × 10⁻⁷ |
| 5 | *Dorea formicigenerans* et rel. | 36.4 | 2.3 × 10⁻⁶ |
| 6–12 | … | … | < 0.05 |

*Bifidobacterium* is highest in **lean** subjects and progressively lower toward **obese** groups.

### 🔹 Longitudinal Stability (LMM)

```
Formula: Shannon ~ Time + Age + Sex + (1 | SubjectID)
Observations: 244   Groups: 78 subjects

Fixed effects:
  Time    β = +0.002   p = 0.692  →  microbiome stable over time
  Age     β = −0.002   p = 0.735  →  non-significant
  Sex     β = +0.160   p = 0.262  →  non-significant

Random effects:
  σ²_between-subject = 0.195  (large personal signature)
  σ²_within-subject  = 0.070  (small temporal fluctuation)
```

> **Conclusion:** The gut microbiome is **more variable between individuals than within the same individual over months** — confirming a stable personal microbiome signature.

### 🔹 Continuous Associations
- Shannon diversity **weakly declines with age** (Pearson r = −0.147)
- Strongest **positive** Spearman ρ with diversity: *Dorea formicigenerans* (ρ ≈ 0.57), *Coprococcus eutactus* (ρ ≈ 0.52), *Ruminococcus obeum* (ρ ≈ 0.51)
- Strongest **negative** ρ: *Prevotella melaninogenica* (ρ ≈ −0.35) — consistent with low-diversity Prevotella-dominated state

### 🔹 Batch Effect
- DNA extraction method has a **significant effect on Shannon diversity** (KW p < 2.2 × 10⁻¹⁶)
- Method **"o"** yields markedly lower diversity than **"p"** and **"r"**
- ⚠️ **Recommendation:** Always include `DNA_extraction_method` as a covariate in statistical models

---

## 🖼 Figures

### Figure 1 — Alpha Diversity
![Alpha Diversity](figures/01_alpha_diversity.png)
*Shannon diversity across BMI groups (KW p = 7.9×10⁻⁸), by sex (Wilcoxon p = 0.039), and Pielou evenness by nationality.*

---

### Figure 2 — Beta Diversity & Ordination
![Beta Diversity](figures/02_beta_diversity.png)
*PCoA by nationality (Axis1: 27.3%, Axis2: 19.7%) and BMI group; NMDS ordination (stress = 0.201).*

---

### Figure 3 — Core Microbiome
![Core Microbiome](figures/03_core_microbiome.png)
*Blanket analysis showing core size at varying thresholds (top). Prevalence vs abundance scatter: 56 core taxa in red (bottom).*

---

### Figure 4 — Taxonomic Composition
![Taxonomic Composition](figures/04_taxonomic_composition.png)
*Relative abundance of top 15 genera averaged by nationality (top) and BMI group (bottom).*

---

### Figure 5 — Differentially Abundant Taxa
![Group Comparisons](figures/05_group_comparisons.png)
*Bifidobacterium abundance across BMI groups — most significant taxon (BH-FDR = 2.5×10⁻¹⁸).*

---

### Figure 6 — Annotated Heatmap
![Heatmap](figures/06_heatmap.png)
*Top 25 genera (Z-scored log abundance), Ward.D2 hierarchical clustering, annotated by Sex, BMI group, and Nationality.*

---

### Figure 7 — Longitudinal Stability
![Longitudinal](figures/07_longitudinal.png)
*Per-subject Shannon trajectories (n = 78) over up to 8.3 months. LMM trend (orange): no significant temporal change (p = 0.692).*

---

### Figure 8 — Continuous Associations
![Associations](figures/08_associations.png)
*Shannon vs age (Pearson r = −0.147, top). Spearman ρ of top 15 genera against Shannon diversity (bottom).*

---

### Figure 9 — Batch Effect
![Batch Effect](figures/09_batch_effect.png)
*Shannon diversity by DNA extraction method — significant batch effect requiring covariate correction (KW p < 2.2×10⁻¹⁶).*

---

## 🛠 Packages

| Package | Version | Role |
|---|---|---|
| `microbiome` | 1.32.0 | Core functions, transforms, core detection |
| `phyloseq` | 1.54.2 | Data structure, subsetting, ordination |
| `vegan` | 2.7-3 | PERMANOVA (`adonis2`), NMDS |
| `ggplot2` | 4.0.3 | All visualisations |
| `lme4` / `lmerTest` | 2.0-1 / 3.2-1 | Linear mixed-effects models |
| `pheatmap` | 1.0.13 | Annotated hierarchical heatmap |
| `ggpubr` | 0.6.3 | Statistical annotations on plots |
| `patchwork` | 1.3.2 | Multi-panel figure composition |
| `ggrepel` | 0.9.8 | Non-overlapping taxon labels |
| `viridis` | 0.6.5 | Perceptually uniform colour scales |
| `tidyverse` | 2.0.0 | Data wrangling and piping |
| `RColorBrewer` | 1.1-3 | Annotation colour schemes |

---

## 📖 Citation

**Dataset:**
```bibtex
@article{lahti2014tipping,
  author  = {Lahti, Leo and Salojarvi, Jarkko and Salonen, Anne
             and Scheffer, Marten and de Vos, Willem M},
  title   = {Tipping elements in the human intestinal ecosystem},
  journal = {Nature Communications},
  volume  = {5},
  pages   = {4344},
  year    = {2014},
  doi     = {10.1038/ncomms5344}
}
```

**microbiome R package:**
```bibtex
@misc{lahti2017microbiome,
  author = {Lahti, Leo and Shetty, Sudarshan and others},
  title  = {Tools for microbiome analysis in {R}},
  year   = {2017},
  url    = {http://microbiome.github.com/microbiome}
}
```

---

## 📬 Contact

**Pundalik Wayal**
`your.email@institution.edu`
GitHub: [@your-username](https://github.com/your-username)

---

## 📄 License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.

---

<p align="center">
  <i>R 4.5.1 · Bioconductor 3.22 · Windows 11 · Completed May 2025</i>
</p>
