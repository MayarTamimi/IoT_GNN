# IoT_GNN

**IoT Network Monitoring Tool Using Graph Neural Networks (GNN)** — graduation project, Hebron University, Faculty of Technology.

Team: Aya Sinokrot, Mayar Tamimi, Besan Darawish, Maysam Baradiya.

## Overview

The project analyzes communication traffic between IoT devices, represents it as a graph (devices/IP addresses as nodes, communications as edges), and uses a Graph Neural Network (GCN or GraphSAGE) to classify devices as normal or suspicious. A rule-based detector serves as a baseline, and results are presented in a Streamlit monitoring dashboard.

Pipeline: IoT Traffic Dataset → Data Preprocessing → Communication Graph → Feature Extraction → GNN Anomaly Detection → Monitoring Dashboard

## Current status

Phase 1 (data understanding and preparation) is in progress. Graph construction, the GNN model, the rule-based baseline, and the dashboard have not been implemented yet.

| Notebook | Purpose |
|---|---|
| `notebooks/01_data_exploration.ipynb` | Load an IoT-23 capture (CTU-IoT-Malware-Capture-34-1) and explore it |
| `notebooks/02_data_cleaning.ipynb` | Clean the data and save `data/cleaned/iot23_cleaned.csv` |
| `notebooks/03_data_featuer_engneering.ipynb` | Connection-level feature engineering |
| `notebooks/04_dataset_visualization _&_Plots.ipynb` | Dataset plots and interpretation |

## Repository structure

```
data/cleaned/        Cleaned dataset (CSV)
notebooks/           Jupyter notebooks for each phase
requirements.txt     Python dependencies
```

## Setup

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook
```

Run the notebooks from the `notebooks/` folder, in order. `02_data_cleaning.ipynb` downloads the raw IoT-23 log and writes the cleaned CSV used by the later notebooks.

## Dataset

[IoT-23](https://www.stratosphereips.org/datasets-iot23) — Stratosphere Laboratory, CTU. Labels are simplified to two classes: benign (0) and malicious (1).
