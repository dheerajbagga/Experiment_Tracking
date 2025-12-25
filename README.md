# Experiment Tracking with MLflow

This repository contains an example project for tracking machine learning experiments using MLflow. It demonstrates how to run experiments, record metrics and artifacts (including models and plots), and inspect experiment history using the MLflow UI.

## Contents

- `Experiment-Tracking.ipynb` - Jupyter notebook used to run experiments and log parameters, metrics, and artifacts to MLflow.
- `data.csv`, `data_new.csv` - Dataset files used in the notebook.
- `confusion_matrix.png` - Example artifact stored at the repo root (also found under `mlruns/`).
- `mlflow.db` - SQLite backend store (local) used to persist MLflow run metadata.
- `mlruns/` - Directory containing MLflow run and model artifacts. Models are saved under `mlruns/1/models/`.
- `requirements.txt` - Python dependencies used by the project.

## Features

- Run experiments and log parameters, metrics, and artifacts (plots, models) with MLflow.
- Persist experiment metadata to a local SQLite store (`mlflow.db`).
- Store model artifacts in the `mlruns/` directory so they can be inspected and loaded later.

## Quickstart (Windows / PowerShell)

1. Create and activate a Python environment (recommended):

```powershell
python -m venv .venv; .\.venv\Scripts\Activate.ps1
```

2. Install dependencies:

```powershell
python -m pip install --upgrade pip; pip install -r requirements.txt
```

3. Open and run the notebook to reproduce experiments (recommended):

```powershell
jupyter notebook Experiment-Tracking.ipynb
```

4. Start the MLflow UI to inspect runs and artifacts:

```powershell
mlflow ui --backend-store-uri sqlite:///mlflow.db --default-artifact-root ./mlruns
# then open http://127.0.0.1:5000 in your browser
```

MLflow UI and ports

- The notebook included with this project sometimes launches the MLflow UI on port 50000 using a shell cell such as:

```python
!mlflow ui --port 50000
```

- If you run the UI from a terminal, the equivalent PowerShell command is:

```powershell
mlflow ui --backend-store-uri sqlite:///mlflow.db --default-artifact-root ./mlruns --port 50000
# then open http://127.0.0.1:50000 in your browser
```

- Notes and troubleshooting:
	- Choose a port that is not already in use. The default MLflow port is 5000; the notebook uses 50000 in some cells to avoid conflicts when the default port is already taken.
	- If `mlflow` isn't in PATH, run via Python:

```powershell
python -m mlflow ui --backend-store-uri sqlite:///mlflow.db --default-artifact-root ./mlruns --port 50000
```

	- To stop the UI started from the notebook, interrupt the cell or terminate the server process in your terminal (Ctrl+C in the terminal where the UI was started). On Windows, you can also find and kill the process by PID when necessary.
	- If you want to run MLflow in the background on Windows, consider starting it from a separate PowerShell session and leaving that session open, or use a lightweight service manager.

## Where to find models and artifacts

- Models created by experiments are saved under `mlruns/1/models/` (subfolders named `m-<hash>`). Each saved model directory contains `MLmodel` and serialized model files (e.g., `model.pkl`), plus environment files such as `conda.yaml` or `requirements.txt`.
- Example artifact images (such as `confusion_matrix.png`) are stored both at the repo root and inside the corresponding run's `artifacts/` folder.

## How to load a saved model

You can load a model saved by MLflow with the mlflow.pyfunc interface or with joblib/pickle depending on how it was saved. Example (pyfunc):

```python
import mlflow.pyfunc

# Replace the path with the model artifact URI or local path, e.g.:
model_uri = "mlruns/1/models/m-<hash>"
model = mlflow.pyfunc.load_model(model_uri)
preds = model.predict(df_to_predict)
```

Or load a pickled sklearn model directly using joblib:

```python
import joblib
model = joblib.load('mlruns/1/models/m-<hash>/artifacts/model.pkl')
```

## Reproducibility

- The project stores environment files (e.g., `conda.yaml`, `python_env.yaml`, `requirements.txt`) alongside saved models under `mlruns/` so specific experiment environments can be reproduced.
- To rerun experiments identically, create a virtual environment with the recorded requirements and run the notebook.

## Contributing

Contributions are welcome. Open an issue or submit a pull request for improvements, bug fixes, or additional examples.

## License

This repository does not include a license file. If you plan to share or reuse the code publicly, consider adding a suitable license (for example, MIT).

## Contact

If you have questions about the experiments or artifacts, open an issue or contact the repository owner.


