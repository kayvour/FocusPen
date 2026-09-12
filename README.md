# FocusPen

FocusPen is a low-cost smart pen prototype for recognizing pen-interaction behaviours in real time. It uses a 6-axis MPU6050 IMU and a Temporal Convolutional Network (TCN) to classify:

- Writing
- Fidgeting
- Idle
- Gripping

Predictions can be passed to a control layer that triggers context-aware haptic feedback through a DRV2605L and LRA actuator.

## Project status

This repository contains the initial research prototype. The notebook covers signal preprocessing, temporal windowing, TCN training, a Random Forest baseline, held-out stream simulation, and haptic-control logic. Hardware integration and deployment are not yet packaged as a production application.

## Requirements

- Python 3.10 or newer
- A Jupyter-compatible environment
- A local labelled dataset named `pen.csv`

Install the Python dependencies in a virtual environment:

```bash
python -m venv .venv
```

Activate it, then install the requirements:

```bash
# Windows PowerShell
.venv\Scripts\Activate.ps1

# macOS/Linux
source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Dataset format

The notebook expects `pen.csv` in the repository root. It must contain these columns:

```text
accel_x, accel_y, accel_z, gyro_alpha, gyro_beta, gyro_gamma, label
```

`label` should contain the behaviour names listed above. The dataset is intentionally not included because it may contain locally collected or personally identifying sensor data.

## Run the prototype

Start JupyterLab and open `tcn.ipynb`:

```bash
jupyter lab
```

Run the cells from top to bottom after placing a compatible `pen.csv` in the repository root. The notebook automatically uses CUDA when it is available and otherwise falls back to the CPU.

## Reproducibility notes

The notebook fixes NumPy and PyTorch random seeds to `42`. Results can still vary across hardware, CUDA versions, library versions, and datasets. The dependency ranges in `requirements.txt` provide a supported baseline rather than a fully locked environment.

## Limitations

- Performance depends on the quality and coverage of the labelled sensor data.
- The current workflow is notebook-based and does not expose a packaged inference API.
- Hardware feedback behaviour has not been validated across pen designs or users.
- Do not use this prototype for medical, safety-critical, or other high-consequence decisions.

## License

This project is available under the [MIT License](LICENSE).
