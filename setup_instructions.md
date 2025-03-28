# ✅ MLOps Assignment Environment Setup Guide (Python 3.8.10 – JarvisLab Compatible)

This guide sets up a virtual environment with the following tools working **together without conflicts**:

- ✅ [PyCaret 2.3.10](https://pycaret.gitbook.io/docs/)
- ✅ [MLflow 1.30.0](https://mlflow.org/)
- ✅ [Airflow 2.5.3](https://airflow.apache.org/)
- ✅ [YData Profiling 4.6.4](https://github.com/ydataai/ydata-profiling)
- ✅ Compatible versions of NumPy, Numba, Matplotlib, and Pillow

---

## 📁 1. Create a Virtual Environment (Fix `ensurepip` error)

If you get this:
```
The virtual environment was not created successfully because ensurepip is not available.
```

### ✅ Fix:
```bash
apt update
apt install -y python3.8-venv
```

### ✅ Create the virtual environment:
```bash
cd /home/CodePro-Lead-Scoring2
python3 -m venv venv
source venv/bin/activate
```

---

## 📦 2. Upgrade pip & setuptools (Required)
```bash
pip install --upgrade pip setuptools wheel
```

---

## 🧠 3. Install PyCaret 2.3.10 (with deprecated `sklearn` fix)

```bash
export SKLEARN_ALLOW_DEPRECATED_SKLEARN_PACKAGE_INSTALL=True
pip install pycaret==2.3.10
```

---

## 🚀 4. Install MLflow 1.30.0

```bash
pip install mlflow==1.30.0
```

Ignore warnings related to:
- `pytz>=2023` (we'll fix)
- `pyyaml>=6.0` (we'll fix)

---

## 🔍 5. Install YData Profiling (replaces pandas-profiling)

### ✅ Uninstall pandas-profiling (if installed)
```bash
pip uninstall -y pandas-profiling
```

### ✅ Fix typing errors
```bash
pip install "typing-extensions>=4.7.1,<5.0"
```

### ✅ Install YData Profiling
```bash
pip install ydata-profiling==4.6.4
```

### ✅ Use this import:
```python
from ydata_profiling import ProfileReport
```

---

## 🛫 6. Install Airflow 2.5.3 (Fix cron-descriptor & setuptools)

### 6.1 Downgrade setuptools
```bash
pip install "setuptools==68.0.0"
```

### 6.2 Install cron-descriptor directly
```bash
pip install cron-descriptor==1.2.24
```

### 6.3 Install Airflow with constraints
```bash
AIRFLOW_VERSION=2.5.3
PYTHON_VERSION=3.8
CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"

pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}" --no-cache-dir
```

---

## 🧬 7. Final Version Fixes for Compatibility

### ✅ Downgrade NumPy for PyCaret & Numba
```bash
pip uninstall -y numpy numba
pip install numpy==1.20.3
pip install numba==0.54.1
```

### ✅ Downgrade PyYAML and pytz for PyCaret and MLflow
```bash
pip install pyyaml==5.4.1
pip install pytz==2022.7
```

### ✅ Downgrade Matplotlib & Pillow for compatibility
```bash
pip install matplotlib==3.5.0
pip install pillow==8.4.0
```

---

## ✅ Final Test Snippet

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import mlflow
from pycaret.classification import *
from ydata_profiling import ProfileReport
```

---

## 📌 Summary of Final Working Versions

| Package             | Version    |
|---------------------|------------|
| pycaret             | 2.3.10     |
| mlflow              | 1.30.0     |
| ydata-profiling     | 4.6.4      |
| apache-airflow      | 2.5.3      |
| numpy               | 1.20.3     |
| numba               | 0.54.1     |
| pyyaml              | 5.4.1      |
| pytz                | 2022.7     |
| matplotlib          | 3.5.0      |
| pillow              | 8.4.0      |
| typing-extensions   | 4.13.0     |
| cron-descriptor     | 1.2.24     |
| setuptools          | 68.0.0     |

---

## 🧪 Save Your Environment
```bash
pip freeze > requirements.txt
```

---
