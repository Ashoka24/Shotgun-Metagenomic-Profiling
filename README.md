# Shotgun metagenomic profiling

This repository runs a small, reproducible shotgun metagenomics workflow on a public paired-end WGS sample. It combines read-quality processing with Kraken 2 and Centrifuge, then compares the two taxonomic profiles.

## Dataset

- BioProject: PRJNA273761
- Run: SRR1779146
- BioSample: SAMN03295851
- Experiment: SRX858749
- Platform: Illumina HiSeq 2000; paired-end WGS

Dataset details and source links are in [metadata/DATASET.md](metadata/DATASET.md).

## Run

The full run downloads the FASTQ files and reference indexes, so it needs a working internet connection and substantial disk space. Install `fastp`, `kraken2`, `centrifuge`, `curl`, `python3`, and `bash`, then run:

```bash
bash scripts/run_shotgun.sh
```

The workflow checks the FASTQ checksums reported by ENA, writes the derived files under `results/`, and removes temporary reads and databases when finished.

## Outputs

- `results/REPORT.md` — run provenance and taxonomic summary
- `results/kraken2.report` and `results/centrifuge.report.tsv` — classifier reports
- `results/top_taxa.tsv` — leading Kraken 2 species assignments
- `results/classifier_comparison.tsv` — top-species comparison
- `results/fastp_summary.json` — read-processing summary

The committed result files are a snapshot from an earlier run. The workflow regenerates outputs from the public run; use the linked Actions artifact for the output from a specific execution.

## Interpretation

This is one sample, not a cohort analysis. It cannot establish taxa associated with necrotizing enterocolitis, causality, biomarkers, or prevalence. Kraken 2 percentages and Centrifuge abundance estimates are also not directly interchangeable.

## Repository layout

```text
metadata/   Dataset provenance
scripts/    Download, QC, classification, and comparison
results/    Compact example result snapshot
figures/    Workflow and output diagrams
.github/    Automated run and artifact upload
```
