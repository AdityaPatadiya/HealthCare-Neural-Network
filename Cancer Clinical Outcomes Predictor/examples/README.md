# SurvivalNet examples

These scripts train and inspect SurvivalNet models:

- `Run.py` splits the data, trains models, and saves results.
- `Bayesian_Optimization.py` configures optional hyperparameter tuning.
- `CostFunction.py` evaluates configurations during tuning.
- `ModelAnalysis.py` calculates feature importance for a saved model.

## Requirements

The code uses Python 2 and legacy dependencies pinned in `requirements.txt`.
Run it in an isolated environment because these versions are incompatible with
modern Python installations. BayesOpt is imported even when optimization is
disabled, so all listed dependencies are required.

From the parent `Cancer Clinical Outcomes Predictor` directory:

```bash
# Required on case-sensitive filesystems because the scripts import
# "survivalnet" while the source directory is named "SurvNet".
ln -s SurvNet survivalnet

python2 -m pip install -r examples/requirements.txt
PYTHONPATH=. python2 examples/Run.py \
  --input_path Brain_Integ.mat \
  --output_path results \
  --epochs 40
```

Use `--bayes_opt` to tune the network architecture and learning parameters.
Run `PYTHONPATH=. python2 examples/Run.py --help` to view all command-line
options.

## Data format

`Run.py` reads a MATLAB `.mat` file with these arrays:

| Key | Description |
| --- | --- |
| `Survival` | Event or last-follow-up time, one value per patient |
| `Censored` | `1` for censored samples, `0` for observed events |
| `Integ_X` | Patient-by-feature matrix (default feature key) |

Pass `--feature_key KEY` to use a different feature matrix. The script creates
60%/20%/20% training, validation, and test splits and repeats training over 20
random shuffles.

The selected output directory receives:

- `final_model`: the serialized model
- `c_index_list.mat`: test concordance indices from all shuffles

`ModelAnalysis.py` expects the saved model plus the sample data's additional
analysis fields (`Patients`, `Integ_X_raw`, and `Integ_Symbs`). Update its
`ModelPaths`, `Models`, and `Data` values before running it on other files.
