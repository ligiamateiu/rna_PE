# RNAseq Workflow Repository

## **Overview**
This repository contains a comprehensive workflow for RNA sequencing (RNAseq) data analysis, from quality control to differential expression analysis (DEA). It uses Snakemake as the workflow manager and includes configurations, scripts, and rules for multiple sub-workflows.

The pipeline ensures reproducibility and scalability and is designed to handle paired-end RNAseq data. It comprises essential steps such as quality control, trimming, alignment, feature counting, and DEA. The repository also includes all required configurations and environment setup files.

---

## **Repository Structure**
### **1. Configurations**
- **`configs/`**
  - **`config_main.yaml`:** Main configuration file containing paths, parameters, and settings for the workflow.
  - **`env.yaml`:** Conda environment file specifying the dependencies for the workflow.

### **2. Scripts**
- **`main.py`:** The main Python script that integrates and manages the Snakemake workflows.
- **`dea.R`:** R script for performing differential expression analysis (DEA) using DESeq2 and generating various visualizations such as PCA plots, volcano plots, and heatmaps.

### **3. Workflow Rules**
- **`quality_control.rules`:** Snakemake rules for performing quality control on raw RNAseq reads using FastQC and MultiQC.
- **`trim.rules`:** Snakemake rules for trimming low-quality bases and adapter sequences using Trimmomatic.
- **`alignment.rules`:** Snakemake rules for aligning reads to the reference genome using STAR, performing QC on alignments with QualiMap, and generating count files with featureCounts.
- **`dea.rules`:** Snakemake rules for performing DEA using the processed count files and generating final reports.

### **4. Input Files**
- **`sampleinfo.tsv`:** Metadata file describing the samples, groups, and experimental conditions.
  - Example content:
    ```
    sample	group	gender	deause
    s1	patient	M	yes
    s2	patient	F	yes
    s3	control	M	yes
    s4	control	F	yes
    ```

### **5. Outputs**
- Intermediate and final outputs are stored in directories specified in `config_main.yaml`:
  - **Intermediate outputs:** Stored in the `OUTPUTPATH` directory (e.g., trimmed reads, alignment files).
  - **Final outputs:** Stored in the `FINALOUTPUT` directory (e.g., normalized counts, DEG reports, QC reports).

---

## **Workflow Steps**
The workflow is divided into four main stages:

### 1. **Quality Control**
   - **Tools:** FastQC, MultiQC
   - **Objective:** Evaluate the quality of raw RNAseq reads.
   - **Output:** A consolidated quality report (`report_quality_control.html`).

### 2. **Trimming**
   - **Tools:** Trimmomatic, FastQC
   - **Objective:** Remove low-quality bases and adapter sequences from reads.
   - **Output:** Trimmed reads (`*_R1_val_1.fq.gz`, `*_R2_val_2.fq.gz`).

### 3. **Alignment and Feature Counting**
   - **Tools:** STAR, featureCounts, QualiMap
   - **Objective:** Align the trimmed reads to the reference genome, perform QC on alignments, and generate read counts for genes.
   - **Outputs:**
     - BAM files (`*.bam`) for alignments.
     - Count files (`*_count.tsv`) for feature counting.
     - Alignment QC reports (`*_BAMqc`).

### 4. **Differential Expression Analysis (DEA)**
   - **Tools:** R (DESeq2, EnhancedVolcano, pheatmap, ggplot2)
   - **Objective:** Identify differentially expressed genes between experimental groups and generate visualizations.
   - **Outputs:**
     - Normalized gene counts (`library_normalized_gene_counts.tsv`).
     - DEG results (`SignifDiffExpr_*.tsv` and `LogRatios_*.tsv`).
     - Visualizations:
       - PCA plot (`PCA_plot.png`).
       - Volcano plot (`DEG_volcanoplot_enhanced_*.png`).
       - Heatmap of DEGs (`DEG_heatmap_*.png`).

---

## **Installation**

### 1. Clone the repository:
```bash
git clone <repository-url>
cd <repository-name>
```

### 2. Set up the environment:
Create a Conda environment using the provided `env.yaml` file:
```bash
conda env create -f configs/env.yaml
conda activate rnaseqsnake
```

### 3. Verify dependencies:
Ensure all required tools (e.g., FastQC, MultiQC, STAR, Trimmomatic) are installed in the Conda environment.

---

## **Usage**

### 1. **Edit Configuration**
- Update the `configs/config_main.yaml` file with paths to your raw data, reference genome, and annotation files.
- Update sample metadata in `sampleinfo.tsv`.

### 2. **Run the Workflow**
Execute the main script to run the entire pipeline:
```bash
python main.py
```

### 3. **Run Individual Sub-Workflows**
Alternatively, you can run specific stages using Snakemake:
- **Quality Control:**
  ```bash
  snakemake -p -s workflow/quality_control.rules
  ```
- **Trimming:**
  ```bash
  snakemake -p -s workflow/trim.rules
  ```
- **Alignment:**
  ```bash
  snakemake -p -s workflow/alignment.rules
  ```
- **DEA:**
  ```bash
  snakemake -p -s workflow/dea.rules
  ```

---

## **Visualization and Reports**
- **Quality Control Reports:** 
  - Found in `FINALOUTPUT/fastqc/report_quality_control.html`.

- **Alignment and Feature Counting Reports:** 
  - Found in `FINALOUTPUT/report_align_count.html`.

- **DEA Results:**
  - DEG and normalized count reports are in `FINALOUTPUT/dea/`.
  - Visualizations (PCA, volcano plots, heatmaps) are in `FINALOUTPUT/dea/plots/`.

---

## **Key Features**
- Scalable for high-throughput RNAseq data.
- Modular design allows running individual steps independently.
- Built-in quality control at multiple stages.
- Automatic generation of publication-ready plots and reports.
- Configurable parameters for customizable workflows.

---

## **References**
- [Snakemake Documentation](https://snakemake.readthedocs.io)
- [DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
- [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
- [STAR Aligner](https://github.com/alexdobin/STAR)

---

## **Contact**
For any issues or questions, contact the repository owner:
- **Name:** Ligia
- **Email:** [Your Email Address]
