# Shotgun metagenomic profiling

This repository runs a reproducible shotgun metagenomics workflow on a public paired-end WGS sample. It combines read-quality processing with Kraken 2 and Centrifuge, then compares the two taxonomic profiles.

## Dataset

- BioProject: PRJNA273761
- Run: SRR1779146
- BioSample: SAMN03295851
- Experiment: SRX858749
- Platform: Illumina HiSeq 2000; paired-end WGS

Dataset details and source links are in [metadata/DATASET.md](metadata/DATASET.md).

## Run locally

The full run downloads FASTQ files and large reference indexes, so it needs an internet connection, several gigabytes of disk space, and time for both classifiers. Install `fastp`, `kraken2`, `centrifuge`, `curl`, `python3`, and `bash`, then run:

```bash
bash scripts/run_shotgun.sh
```

The script checks the FASTQ checksums reported by ENA, writes derived files under `results/`, and removes temporary reads and databases when finished.

## GitHub Actions

Pushes run fast checks on the scripts and confirm that the committed result snapshot is present. To execute the full public-sample workflow in GitHub Actions, open the Actions tab and manually run **Shotgun metagenomics**. A completed full run uploads the outputs as an artifact. This separation avoids downloading multi-gigabyte reference databases on every documentation or code change.

## Outputs

- `results/REPORT.md` — provenance and taxonomic summary
- `results/kraken2.report` and `results/centrifuge.report.tsv` — classifier reports
- `results/top_taxa.tsv` — leading Kraken 2 species assignments
- `results/classifier_comparison.tsv` — top-species comparison
- `results/fastp_summary.json` — read-processing summary

The committed result files are a reference snapshot, not output from every new CI run.

## Interpretation

This is one sample, not a cohort analysis. It cannot establish taxa associated with disease, causality, biomarkers, or prevalence. Kraken 2 percentages and Centrifuge abundance estimates are not directly interchangeable.
