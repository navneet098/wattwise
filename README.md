# WattWise
**Household Electricity Forecasting & Unsupervised Anomaly Detection**

WattWise is a reproducible research pipeline for (1) next-hour household electricity-demand forecasting and (2) unsupervised flagging of unusual hourly consumption. It follows the scope and method plan in the project poster.

## Research question
Can time-aware forecasting and unsupervised anomaly detection identify unusual consumption patterns while keeping forecast errors and false alarms manageable?

## Dataset
UCI Individual Household Electric Power Consumption, a public one-household dataset containing 2,075,259 one-minute measurements from December 2006 to November 2010 and nine original attributes.

Official page: https://archive.ics.uci.edu/dataset/235/individual+household+electric+power+consumption

The raw dataset is **not committed**. The preprocessing script downloads the official archive into `data/raw/` on first run. See `data/README.md`.

## Methods
### Forecasting
- Previous-day-same-hour baseline (`t-24`)
- Linear Regression
- Random Forest Regressor

### Anomaly detection
- Rolling median / median absolute deviation rule
- Isolation Forest on hourly consumption
- Isolation Forest on forecast residuals

### Evaluation
Chronological split: earliest 70% train, next 15% validation, final 15% test. MAE and RMSE are reported in kW. Anomaly outputs are candidate alerts, not confirmed faults: the dataset contains no verified fault labels.

## Quick start
Python 3.10 or 3.11 is recommended.

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd WattWise
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
python -m src.run_pipeline
streamlit run dashboard/app.py
```

The first pipeline run downloads the UCI archive. Internet access is therefore needed once, unless the archive is placed manually at `data/raw/household_power_consumption.txt`.

## Outputs
The pipeline writes:
- `results/metrics.json`
- `results/forecast_comparison.csv`
- `results/anomaly_results.csv`
- `results/figures/forecast_test.png`
- `results/figures/anomaly_timeline.png`
- `models/` serialized fitted models
- `checkpoints/` model artifact manifest and generated model files (copied by the pipeline)

These outputs are generated locally and are intentionally not included as claimed experimental results in this starter submission. Run the pipeline to produce actual values.

## Reproducibility
1. Use the pinned dependencies in `requirements.txt`.
2. Download the public dataset through `src/data_preprocessing.py`.
3. Run `python -m src.run_pipeline`.
4. Record Python/platform/dependency versions and Git metadata with `python reproducibility/capture_provenance.py`.
5. Inspect `results/metrics.json` and the generated plots.
6. Launch the dashboard with `streamlit run dashboard/app.py`.

See `reproducibility/reproducibility_statement.md` and `reproducibility/experiment_config.yaml`.

## Repository hygiene
- Do not commit the raw dataset, virtual environment, caches, or large generated artifacts.
- Commit source code, configs, report, and small summary outputs only.
- If model files exceed the hosting limit, upload them to a durable model repository (for example Hugging Face Hub) and replace the placeholder in `checkpoints/README.md` with the actual immutable model revision URL.
- Never claim a commit hash or training result until it exists in the real Git repository.

## Team
The poster identifies Vishal Mardi (M26MAC012) and Navneet Singh (M26CSA004). It assigns data preparation/features/experiments to Vishal and dashboard/visualisation/integration to Navneet, with joint review of results and limitations.

## Limitations
One older French household is not representative of all homes or current Indian tariffs. Missing measurements, temporal drift, and lack of fault labels limit conclusions. Anomaly flags should be reviewed, not treated as diagnosis or proof of waste.

## Citation
Hebrail, G. and Berard, A. (2012). *Individual Household Electric Power Consumption*. UCI Machine Learning Repository. https://doi.org/10.24432/C58K54
