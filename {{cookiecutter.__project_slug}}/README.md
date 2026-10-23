# {{ cookiecutter.project_name }}

{{ cookiecutter.description }}

- **Assay:** {{ cookiecutter.assay }}
- **Organism / build:** {{ cookiecutter.organism }} ({{ cookiecutter.genome_build }})
- **Author:** {{ cookiecutter.author_name }} <{{ cookiecutter.author_email }}>

## Layout

```
data/
  raw/         raw FASTQ/BAM as received (never modified)
  reference/   genome FASTA, annotation, indices
  interim/     intermediate files (safe to delete)
  processed/   final processed data
logs/          tool and pipeline logs
results/
  qc/  alignments/  counts/  variants/  tables/  figures/
scripts/
  python/      Python scripts, numbered in run order (01_, 02_, ...)
  R/           R scripts, numbered in run order
```

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