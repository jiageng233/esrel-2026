# esrel-2026
Reliability Assessment of Reusable Equipment in Cryogenic Environments Using Bayesian Nonparametric Models
[README.md](https://github.com/user-attachments/files/25746502/README.md)
# Minimal reproducibility package (simulation + optional NASA)

This repository provides a minimal, self-contained verification program for the paper’s core computations:
- Failure-mode discovery via DP-like Gaussian mixture (variational Bayes)
- Censoring-aware reliability via Kaplan–Meier with bootstrap uncertainty bands
- GP-based RUL prediction with interval evaluation and conformal interval calibration
- Robustness sweeps across observation ratios and random seeds

## Contents
- `bnp_cryogenic_paper_demo.py` (main entry)
- `cryogenic_reliability_data_generator.py` (lightweight simulator)
- `requirements.txt`

## Install
```bash
python -m pip install -r requirements.txt
```

## Run (simulation)
Quick sanity run (fast, smaller internal iteration counts):
```bash
python bnp_cryogenic_paper_demo.py --quick --outdir results/paper_demo
```

Paper-style run (example; adjust repeats/ratios to match your settings):
```bash
python bnp_cryogenic_paper_demo.py \
  --seed 0 \
  --n-failure-samples 200 \
  --n-degradation-units 25 \
  --max-cycles 60 \
  --repeats 10 \
  --obs-ratios 0.5,0.7,0.9 \
  --outdir results/paper_demo_robustness
```

## Key outputs (written under `--outdir`)
- `dpmm_mode_recovery.csv`: ARI/NMI for simulated mode recovery
- `dpmm_contingency_true_vs_hat.csv`: true vs inferred mode contingency table
- `mode_reliability_summary.csv`: KM-based reliability summaries per inferred mode
- `km_reliability_by_mode.png`: KM curves with bootstrap bands
- `gp_rul_coverage_summary.csv`, `gp_rul_coverage_by_unit.csv`: RUL interval metrics (raw + conformal)
- `gp_interval_comparison.png`: raw vs conformal coverage/width comparison
- `rul_robustness_summary.csv`, `rul_robustness_runs.csv`, `rul_robustness.png`: robustness sweeps (only when `--repeats>1` or multiple `--obs-ratios`)
- `figures_bw/`: paper-friendly black/white figure copies (when plotting is available)

## Optional: NASA C-MAPSS (not bundled)
If you want to reproduce the NASA public benchmark/validation, download the C-MAPSS data separately and place files under:
- `data/nasa_cmapss/`

Expected filenames include (example):
- `train_FD001.txt`, `test_FD001.txt`, `RUL_FD001.txt`

Then run:
```bash
python bnp_cryogenic_paper_demo.py --run-nasa --nasa-subset FD001 --nasa-dir data/nasa_cmapss --outdir results/nasa_fd001
```
