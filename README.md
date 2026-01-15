This repository contains anonymized artifacts accompanying the CCS submission "Evaluating Latent Representations in Network Intrusion Detection Datasets Through Multiverse Topological Analysis"

The artifacts are provided solely for the purpose of evaluating the methodology and claims in the paper.

In order to run the analyses of the paper without rerunning the multiverse pipeline, make sure that the `metrics_table.csv` file is placed in `data/processed/analysis/`. 

## Project Structure
Leaf locations with an asterisk indicate the presence of data artifacts

├── config
│   └── datasets
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

## CLI commands to reproduce the paper results
ds
