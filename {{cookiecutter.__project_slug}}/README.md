# {{ cookiecutter.project_name }}

{{ cookiecutter.description }}

- **Assay:** {{ cookiecutter.assay }}
- **Organism / build:** {{ cookiecutter.organism }} ({{ cookiecutter.genome_build }})
- **Author:** {{ cookiecutter.author_name }} <{{ cookiecutter.author_email }}>

## Layout

Files are organized by pipeline stage: `data/` holds the starting inputs and
the analysis objects built by our own scripts; `results/` holds the output of
standard NGS tools, one folder per step, plus final tables and figures.

```
data/
  raw/          FASTQs and sample sheets exactly as received (never modified)
  reference/    genome FASTA, annotation GTF, aligner indices
  interim/      temporary pipeline intermediates, e.g. trimmed FASTQs,
                unsorted BAMs (safe to delete)
  processed/    objects built by our scripts from pipeline outputs,
                e.g. DESeq2/Seurat objects, normalized matrices
logs/           tool and pipeline logs
results/
  qc/           FastQC, MultiQC and other QC reports
  alignments/   final sorted/deduplicated BAMs and their indices
  counts/       count tables from featureCounts, Salmon, etc.
  variants/     VCFs from variant callers
  tables/       final tables for reporting (e.g. DE gene lists)
  figures/      plots
scripts/
  python/       Python scripts, numbered in run order (01_, 02_, ...)
  R/            R scripts, numbered in run order
```

Data flow: `data/raw` → `data/interim` → `results/alignments` →
`results/counts` (or `results/variants`) → scripts → `data/processed` →
`results/tables` and `results/figures`.

## Environment (pixi)

First-time setup:

```bash
pixi init --channel conda-forge --channel bioconda
pixi add python pandas    # Python tools
pixi add r-base           # R tools
```

Running scripts:

```bash
pixi run python scripts/python/01_example.py
pixi run Rscript scripts/R/01_example.R
pixi shell                # or work interactively inside the environment
```

Commit `pixi.toml` and `pixi.lock` so the environment can be recreated exactly.

## Notes



## Conventions

1. Never edit files in `data/raw/`.
2. Every result should be reproducible from `data/raw/` + `config/` + code.
3. Paths and parameters live in `config/config.yaml`, not hard-coded in scripts.
4. Record software versions (conda env exports, `sessionInfo()`) in `logs/`.