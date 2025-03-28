Here’s a complete and clean **`setup_instructions.md`** with all steps, justifications, and version compatibility fixes included. It assumes the user will face all errors we encountered and provides exact resolutions and reasoning.

---

# ✅ MLOps Assignment Environment Setup Guide (Python 3.8.10 – JarvisLab Compatible)

This guide sets up a virtual environment with the following tools working **together without conflicts**:

- ✅ [PyCaret 2.3.10](https://pycaret.gitbook.io/docs/)
- ✅ [MLflow 1.30.0](https://mlflow.org/)
- ✅ [Airflow 2.5.3](https://airflow.apache.org/)
- ✅ [YData Profiling 4.6.4](https://github.com/ydataai/ydata-profiling) *(successor to Pandas Profiling)*

---

## 📁 1. Create a Virtual Environment (fix `ensurepip` error)

If you get this error:

```
The virtual environment was not created successfully because ensurepip is not available.
```

### 🛠 Fix:
```bash
apt update
apt install -y python3.8-venv
```

Then create the environment:
```bash
cd /home/CodePro-Lead-Scoring2
python3 -m venv venv
source venv/bin/activate
```

---

## 📦 2. Upgrade pip & setuptools

Required to avoid metadata-related install issues.
```bash
pip install --upgrade pip setuptools wheel
```

---

## 🧠 3. Install PyCaret 2.3.10 (with fix for deprecated `sklearn`)

PyCaret depends on a now-deprecated alias `sklearn`. This step allows it:

```bash
export SKLEARN_ALLOW_DEPRECATED_SKLEARN_PACKAGE_INSTALL=True
pip install pycaret==2.3.10
```

---

## 🚀 4. Install MLflow 1.30.0

Compatible with PyCaret 2.x:
```bash
pip install mlflow==1.30.0
```

**Note**: You'll see dependency warnings for:
- `pytz >=2023` vs. `mlflow requires <2023`
- `pyyaml 6.0` vs. `pycaret requires <6.0`

These do **not break functionality** for this project.

---

## 🔍 5. Replace `pandas-profiling` with `ydata-profiling`

PyCaret expects `pandas-profiling`, but it's deprecated and fails due to `typing_extensions` errors. We use the actively maintained fork:

```bash
pip uninstall -y pandas-profiling
pip install "typing-extensions>=4.7.1,<5.0"
pip install ydata-profiling==4.6.4
```

Then use in code:
```python
from ydata_profiling import ProfileReport
```

---

## 🛫 6. Airflow 2.5.3 Setup (with full dependency fix)

### 6.1 Downgrade setuptools to avoid `cron-descriptor` build error

```bash
pip install "setuptools==68.0.0"
```

### 6.2 Install compatible version of `cron-descriptor`

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

## 🧪 7. Test and Freeze Environment

```bash
pip freeze > requirements.txt
```

---

## ✅ Summary of Key Fixes

| Issue | Resolution |
|-------|------------|
| `ensurepip` missing | Installed `python3.8-venv` |
| Deprecated `sklearn` | Allowed install with env var |
| `pandas-profiling` import error | Replaced with `ydata-profiling` |
| Airflow fails on `cron-descriptor` | Downgraded `setuptools`, installed working version |
| Dependency version mismatches | Ignored safe warnings, functionality preserved |

---

## 📌 Notes

- Do **not upgrade setuptools** after Airflow install.
- Do **not mix PyCaret 3.x or newer MLflow versions** in this setup.
- Avoid `typing-extensions>=5.x` or `pytz>=2023.3`.

---
