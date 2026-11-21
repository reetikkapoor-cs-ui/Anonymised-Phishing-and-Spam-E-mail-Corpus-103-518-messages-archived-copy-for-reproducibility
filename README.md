# When Does Input-Conditional Fusion Pay Off?

Code, results and LaTeX sources for the paper:

> R. Kapoor, "When Does Input-Conditional Fusion Pay Off? Pool Strength, Locality and Equivalence in Phishing-Detection Ensembles," 2026.

The study compares a disagreement-aware gating meta-learner (CGAM), five dynamic ensemble selection (DES) methods and logistic stacking on three public phishing corpora. Every combiner is fitted on the same selection set (a matched protocol), each comparison is repeated over 15 paired evaluations, and results are tested with corrected resampled t-tests plus equivalence tests (TOST and a Bayesian ROPE).

## Key findings

- **Strong pools (RF, XGBoost, LightGBM, MLP):** CGAM is equivalent to logistic stacking within ±0.15 pp, and no DES method is credibly better, even when one learner is broken in failure regions covering 12–65% of the data.
- **Weak, diverse pools (bagged depth-3 trees):** four DES methods beat stacking by 2.9–4.3 pp on the real-text corpus, but that pool is still about 4 pp less accurate than the strong pool.
- **Gate behaviour:** CGAM's gate does down-weight a learner inside its failure region, but this brings no accuracy gain. Removing the residual path or training five times longer changes nothing.
- **Pre-test:** clustering-and-selection (Kuncheva, 2000) never beats the best single strong learner, which predicts the outcome cheaply.
- **Dataset audit:** two corpora are synthetic or semi-synthetic, and coercing string attributes to numbers silently makes 20 of 33 attributes of one corpus look constant, including SPF/DKIM/DMARC.

## Repository layout

```
paper/      main.tex, supplement.tex, IEEEtran.cls, generated tables/ and figs/
deck/       deck.tex (Beamer slides; reads ../paper/tables and ../paper/figs)
scripts/    experiment, analysis and table/figure generators
results/    summary JSON files, per-fold predictions (.npz), analysis logs
```

| Script | Purpose |
|---|---|
| `corpora.py` | Loaders and in-fold preprocessing for D1–D3 |
| `cgam.py` | NumPy CGAM (full, noGate, noDisagreement, gate-only); run it directly for a gradient check |
| `run_experiment.py` | Strong pool: natural, global-failure and ~60% local-failure settings |
| `run_experiment2.py` | Region-size sweep, diverse weak pools, CGAM variants, k-sweep pre-test |
| `deslib_compat.py` | Patches DESlib 0.3.7 for scikit-learn ≥ 1.6 |
| `equivalence.py` | TOST margin and Bayesian correlated t-test |
| `audit_all.py` | Provenance and saturation audit, including the coercion pitfall |
| `analyze.py`, `analyze2.py`, `fill_text.py` | Statistics and summary values |
| `make_tables.py`, `make_tables2.py`, `make_figs.py`, `make_deck.py` | All paper and deck tables, figures and numbers |

## Data

The datasets are not included in this repository. Download them from their sources:

| Corpus | Description | Source |
|---|---|---|
| D1 | 8,000 engineered records (semi-synthetic) | M. Alam, "Phishing email dataset," Kaggle, 2024 |
| D2 | 50,000 header/metadata records (synthetic) | [Phishing Email Detection Dataset (50k Emails)](https://www.kaggle.com/datasets/mobeenfatimah/phishing-email-detection-dataset-50k-emails), Kaggle |
| D3 | 103,518 anonymised real e-mails (phishing + spam) | Archived copy: Zenodo, doi: `10.5281/zenodo.XXXXXXX` (SHA-256 `14735fea…`) |

Place the files as `d1/D1.csv`, `d2/D2.pkl` and `d3/D3.csv` under the root set in `scripts/corpora.py`. D2 ships as an Excel file despite its `.csv` name; convert it once with:

```python
import pandas as pd
pd.read_excel("D2.csv").to_pickle("D2.pkl")
```

## Reproduce

```bash
pip install scikit-learn xgboost lightgbm deslib scipy matplotlib
cd scripts
python run_experiment.py  D1 0 1 2 && python run_experiment.py  D3 0 1 2 && python run_experiment.py  D2 0 1 2
python run_experiment2.py D1 0 1 2 && python run_experiment2.py D3 0 1 2 && python run_experiment2.py D2 0 1
python audit_all.py
python analyze.py && python analyze2.py && python fill_text.py
python make_tables.py && python make_tables2.py && python make_figs.py && python make_deck.py
```

The full run takes about 4–5 hours on a 2-core machine. Seeds are fixed, so reruns reproduce the reported numbers.

## Build the documents

```bash
cd paper && pdflatex main.tex && pdflatex main.tex
pdflatex supplement.tex && pdflatex supplement.tex
cd ../deck && pdflatex deck.tex && pdflatex deck.tex
```

## Citation

```bibtex
@inproceedings{kapoor2026fusion,
  author = {Reetik Kapoor},
  title  = {When Does Input-Conditional Fusion Pay Off? Pool Strength, Locality and Equivalence in Phishing-Detection Ensembles},
  year   = {2026}
}
```

## License

Code: MIT. The datasets keep their original licences and are not redistributed here.
