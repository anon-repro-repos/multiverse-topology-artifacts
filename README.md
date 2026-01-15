This repository contains anonymized artifacts accompanying the CCS submission "Evaluating Latent Representations in Network Intrusion Detection Datasets Through Multiverse Topological Analysis"

The artifacts are provided solely for the purpose of evaluating the methodology and claims in the paper.

In order to run the analyses of the paper without rerunning the multiverse pipeline, make sure that the following files are unpacked in their respective directories: `metrics.tar` (`data/processed/metrics/`), `eval_metrics.tar` (`data/processed/eval_metrics/`) and `ae.tar` (`data/interim/autoencoder/`).  
The comparison of topological structure with model performance can not be run without running the first stage of the multiverse pipeline. All the other analyses rely solely on the included data artifacts.
## Project Structure
Leaf locations with an asterisk indicate the presence of data artifacts
```text
├── config
│   └── datasets*
│       ├── NF-CICIDS2018-v3.yml
│       ├── NF-ToN-IoT-v3.yml
│       └── NF-UNSW-NB15-v3.yml
├── data
│   ├── figures*
│   ├── interim
│   │   ├── autoencoder*
│   │   ├── landscapes*
│   │   ├── persistence
│   │   ├── preprocessing_status
│   │   └── projections
│   ├── processed
│   │   ├── analysis*
│   │   ├── eval_metrics*
│   │   ├── metrics*
│   │   ├── test
│   │   ├── train
│   │   └── validation
│   └── raw
├── logs
├── src
│   └── project
│       ├── io
│       │   └── storage.py
│       ├── pipeline
│       │   ├── autoencoder.py
│       │   ├── create_embeddings.py
│       │   ├── create_tda.py
│       │   ├── embeddings.py
│       │   ├── evaluation.py
│       │   ├── landscapes.py
│       │   ├── metrics.py
│       │   ├── persistence.py
│       │   ├── preprocessing.py
│       │   └── universes.py
│       ├── tests
│       │   ├── data_checks.py
│       │   ├── landscape_checks.py
│       │   └── landscape_norm_tests.py
│       ├── __init__.py
│       ├── analyses.py
│       ├── cli.py
│       ├── config.py
│       ├── example_plots.py
│       ├── logging_config.py
│       ├── plots.py
│       └── utils_analyses_plots.py
└── README.md
```
## CLI commands to reproduce the paper results
To run the complete pipeline, run the command line functions below. Note, universe index parameter takes an integer value between 0 and 383 and can be parallelized. 
```bash
    uv sync

    # One-time cleaning of the data
    uv run setup initiate NF-ToN-IoT-v3
    uv run setup initiate NF-UNSW-NB15-v3
    uv run setup initiate NF-CICIDS2018-v3

    # Preprocessing multiverse
    uv run acb prepare-preprocessing [--universe-index [integer]] --overwrite

    # Train AEs and retrieve embeddings
    uv run acb prepare-embeddings --split test [--universe-index [integer]] --force-recompute --retrain-regardless

    # Prepare embeddings and compute persistent homology
    uv run acb prepare-tda [--universe-index [integer]] --overwrite 
```

The code below reproduces the statistics, tables and figures in the paper and can largely be run solely with the provided artifacts, except for the comparison between topological complexity and reconstruction error.
```bash
    # Run uv sync only if this has not been run in the pipeline steps
    uv sync

    # Creates the metrics_table.csv file (already present as artifact)
    uv run analyze table --split test

    # Run these for tables and statistics
    uv run analyze presto-variance --split test --split-by dataset --norm-threshold 10 --exclude-zero-norms
        
    uv run analyze presto-global-sensitivity --split test --norm-threshold 10 --exclude-zero-norms
        
    uv run analyze presto-local-sensitivity --split test --param all --norm-threshold 10 --exclude-zero-norms
        
    uv run analyze presto-stability-regions --split test --param all --top-k 3 --norm-threshold 10 --exclude-zero-norms
        

    # Run these for plots
    uv run plot example-plots
    
    uv run plot presto-variance-violin --split test --dims 0,1,2 --norm-threshold 10 --exclude-zero-norms
        
    uv run plot presto-individual-violin --split test --dataset NF-CICIDS2018-v3 --norm-threshold 10 --exclude-zero-norms
        
    uv run plot presto-individual-violin --split test --dataset NF-ToN-IoT-v3 --norm-threshold 10 --exclude-zero-norms
        
    uv run plot presto-individual-violin --split test --dataset NF-UNSW-NB15-v3 --norm-threshold 10 --exclude-zero-norms
```