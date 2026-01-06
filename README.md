Smart-seq2 RNA-seq Analysis Pipeline

This repository contains a reproducible analysis pipeline for Smart-seq2 RNA-seq data, including quality control, transcript quantification, genome alignment for QC, and differential expression analysis.

The workflow is suitable for paired-end Smart-seq2 RNA-seq and was designed to support manuscript publication.

Overview

Main steps

Project setup and sample annotation

FASTQ quality control (FastQC, MultiQC)

Transcript-level quantification (Salmon)

Genome alignment for QC and coverage (HISAT2)

Differential expression analysis (tximport + DESeq2)

Optional genome browser tracks (BigWig)

Organism: Mouse
Transcriptome: GENCODE vM38
Genome: mm39 (GRCm39)

Directory structure

After initialization, the project follows this layout:

rnaseq_smartseq2/
├── fastq/                  # input FASTQ files
├── ref/
│   ├── gencode/             # transcriptome reference (Salmon)
│   └── hisat2/              # genome reference (HISAT2)
├── work/
│   ├── fastqc/              # FastQC outputs
│   ├── salmon/              # Salmon quantification
│   ├── bam/                 # genome-aligned BAM files
│   └── rseqc/               # QC metrics
├── results/
│   ├── qc/
│   ├── tables/              # DE results
│   └── plots/
├── scripts/
│   └── analyze_rnaseq.R
├── samples.tsv
└── pipeline.sh

Input requirements
FASTQ files

Paired-end FASTQ files must follow this naming convention:

fastq/<sample>_R1.fastq.gz
fastq/<sample>_R2.fastq.gz

Software requirements

The pipeline uses micromamba to ensure reproducibility.

Main tools:

FastQC / MultiQC

Salmon

HISAT2

Samtools

DESeq2

tximport

deepTools (optional)

Running the pipeline
1. Edit project path

Open pipeline.sh and set:

export PROJECT_DIR=/absolute/path/to/rnaseq_smartseq2

2. Run
bash pipeline.sh

The full workflow (reference download, QC, quantification, DE analysis) will run automatically.

Quantification strategy

Reads are quasi-mapped to the transcriptome using Salmon

Transcript-level estimates are summarized to gene-level counts using tximport

 Post data analysis is performed in R

Analyses are conducted separately for each fraction (e.g. nucleus vs cytoplasm)

Differential expression analysis by Deseq2

Filtering criteria:

At least 10 counts in 2 samples

Outputs:

results/tables/
├── DE_nucleus.tsv
└── DE_cytoplasm.tsv


Each table contains:

log2 fold change

Wald test statistics

adjusted p-values (FDR)

Quality control
FASTQ-level QC

Per-base quality scores

Adapter content

Sequence duplication

→ summarized using MultiQC

Alignment-based QC

Genome alignment via HISAT2

BAM files generated for:

coverage visualization

read distribution analysis

strandedness checks

Optional: genome browser tracks

If enabled, BigWig coverage tracks are generated:

results/qc/*.bw


These can be visualized in:

UCSC Genome Browser

IGV

Normalization: RPKM

Reproducibility notes

All software versions are controlled via micromamba

No hard-coded system paths

Fully script-driven

Suitable for HPC or local environments

Citation

If you use this pipeline, please cite:

