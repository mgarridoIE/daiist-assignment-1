# Car Price Advisor

Assignment 1 project: a dealership asking-price prototype using car advertisements
from Ukraine in 2016. All prices are in USD. The employee reviews the estimate
and decides the final advertised price. This is not a tool validated for current
market prices or time-to-sale predictions.

## Run the project

Use Python 3.12 and uv. From the repository folder, install the locked dependencies:

```bash
uv sync --locked
```

The dataset and saved models are included. Reproduce the complete training and
evaluation pipeline with:

```bash
uv run python main.py train
```

Open the dashboard with:

```bash
uv run python main.py app
```

Visit http://127.0.0.1:7860 in your browser. Stop the server with Ctrl+C.
The app loads saved models; it does not train at startup. Training regenerates
the prepared data, models and evaluation files. It reproduces the existing
experiment; rerunning does not create a new independent test set.

Run the inference checks with:

```bash
uv run python -m pytest -q test_dashboard.py
```

## Project files

`REPORT.md` explains the dataset, business idea, results, limitations and AI use.
`data/car_ad.csv` is the original dataset; `data/README.md` records its source,
units and cleaning rules. `clean_data.py` and `prepare_data.py` create the cleaned
and prepared data.

`train.py` runs the pipeline. The baseline, scikit-learn experiments and two
PyTorch implementations have separate scripts. `evaluate_test.py` evaluates
saved models on the reserved test rows. `results/` contains the saved models,
metrics and predictions. `app.py` builds the Gradio dashboard.

The dashboard offers a price recommendation with an employee adjustment, a
comparison of the three implementations, and data distributions. The three
final implementations use the same log-price regression. Each has test MAPE
of approximately 25.65%; this is an average error, not a per-car guarantee.

## Reproducibility and delivery

Commit the source code, dataset, prepared artifacts, results, report,
`pyproject.toml`, `uv.lock` and `.github/workflows/verify.yml`. Do not include
the local virtual environment, caches, editor settings, secrets or unused
starter notebooks. The `.gitignore` covers those local files.

The original assignment instructions are preserved in `docs/ASSIGNMENT.md`.
GitHub Actions runs training and checks dashboard startup after a push.
Local checks passed; a successful GitHub Actions run must still be confirmed.
