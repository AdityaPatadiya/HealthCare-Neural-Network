# Healthcare Neural Networks

This repository contains **SurvivalNet**, a deep-learning model for predicting
time-to-event clinical outcomes from cancer genomic data. It trains neural
networks with Cox partial likelihood and includes optional Bayesian
hyperparameter optimization and feature analysis.

> **Note:** This is legacy research code written for Python 2 and dependency
> versions from 2016–2017. Use an isolated legacy environment; it will not run
> unchanged on current Python versions.

## Repository contents

```text
Cancer Clinical Outcomes Predictor/
├── Brain_Integ.mat       # Sample dataset
├── SurvNet/              # Model, optimization, and analysis code
├── examples/             # Training and analysis scripts
└── results/              # Default output directory
```

## Quick start

The original dependencies are pinned in
`Cancer Clinical Outcomes Predictor/examples/requirements.txt` and include
Python 2-compatible versions of Theano, NumPy, SciPy, lifelines, and BayesOpt.

On a case-sensitive filesystem, create the lowercase package alias expected by
the example scripts, then run the sample from the project directory:

```bash
cd "Cancer Clinical Outcomes Predictor"
ln -s SurvNet survivalnet
python2 -m pip install -r examples/requirements.txt
PYTHONPATH=. python2 examples/Run.py \
  --input_path Brain_Integ.mat \
  --output_path results \
  --epochs 40
```

Add `--bayes_opt` to enable Bayesian hyperparameter tuning. This substantially
increases runtime. See `python2 examples/Run.py --help` for all options.

## Input and output

The training script expects a MATLAB `.mat` file containing:

- `Survival`: event or last-follow-up times
- `Censored`: `1` for censored samples and `0` for observed events
- A feature matrix, `Integ_X` by default; select another key with `--feature_key`

Rows must represent the same patients across all arrays. Training writes the
model to `final_model` and concordance-index results to `c_index_list.mat` in
the selected output directory.

More information about the scripts and custom data is available in the
[examples README](Cancer%20Clinical%20Outcomes%20Predictor/examples/README.md).

## References

- Yousefi et al., [Learning Genomic Representations to Predict Clinical Outcomes in Cancer](https://arxiv.org/abs/1609.08663) (2016)
- Yousefi et al., [Predicting clinical outcomes from large scale cancer genomic profiles with deep survival models](https://doi.org/10.1038/s41598-017-11817-6) (2017)

