# NetworkSecurity — Phishing Detection Pipeline

A modular end-to-end machine learning pipeline for detecting phishing/network-security issues from tabular data. The project provides components for data ingestion (from MongoDB), validation, transformation, model training, and simple prediction/API tooling.

---

## Key Features
- Data ingestion from MongoDB and export to CSVs
- Schema-based data validation and dataset-drift detection (KS test)
- Missing-value handling and preprocessing with a KNN imputer
- Model training with hyperparameter search across multiple classifiers
- MLflow experiment tracking and artifact persistence
- Simple app/template for predictions and results display

## Technology Stack
- Python 3.10+
- pandas, numpy, scikit-learn
- pymongo (MongoDB data source)
- mlflow and DagsHub integration for experiment tracking
- fastapi / uvicorn (API/app endpoints)
- Docker for containerization

## Repository Layout
- [main.py](main.py) — pipeline orchestrator (ingest → validate → transform → train)
- [app.py](app.py) — lightweight web/app script (API or UI entrypoint)
- [requirements.txt](requirements.txt) — Python dependencies
- [Dockerfile](Dockerfile) — container image definition
- [networksecurity/](networksecurity/) — Python package containing core modules:
	- `components/` — pipeline stages (data_ingestion, data_validation, data_transformation, model_trainer)
	- `entity/` — configuration and artifact dataclasses
	- `utils/` — helpers for IO, persistence, and evaluation
	- `constant/` — pipeline constants and paths
	- `pipeline/` — higher-level pipeline orchestration
	- `logging/` — centralized logger
- `final_model/` — saved model and preprocessor artifacts
- `prediction_output/` — sample prediction outputs

## Quickstart — Local (development)

Prerequisites
- Python 3.10+ and pip
- MongoDB instance reachable from the machine (connection string configured via env)

Install dependencies

```bash
python -m pip install -r requirements.txt
```

Configuration
- Create an `.env` file in the project root and set `MONGO_DB_URL` (and any other secrets):

```ini
MONGO_DB_URL=mongodb://<username>:<password>@host:port/dbname
```

Run the full training pipeline

```bash
python main.py
```

Run the app (if you want to serve a minimal API/UI)

```bash
uvicorn app:app --reload
```

## Running with Docker

Build the image

```bash
docker build -t networksecurity:latest .
```

Run the container (pass env file)

```bash
docker run --env-file .env -p 8000:8000 networksecurity:latest
```

## Tests
- There is a `test_mongodb.py` file that can be used as a quick connectivity or smoke test. For a full test suite, add `pytest` tests under a `tests/` folder and run:

```bash
pytest
```

## Notes & Known Issues
- The training evaluator in `networksecurity/utils/main_utils/utils.py` currently uses `r2_score` to compare models. For classification tasks you should replace it with a classification-appropriate metric (e.g., F1, accuracy) to ensure correct model selection.
- Confirm that `MONGO_DB_URL` and any required buckets/permissions are configured before running ingestion.

## Contributing
- Fork, create a feature branch, and open a PR. Please include tests and update the README when adding features.

## License
This repository does not include a license file. Add a `LICENSE` at the project root to clarify usage terms.

