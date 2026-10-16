# 📈 Advertising Sales Prediction — Multiple Linear Regression

A machine learning project that predicts product **sales** from advertising spend on **TV, Radio and Newspaper**, using Multiple Linear Regression (scikit-learn). The trained model is served through a small **Flask web app**, so you can enter budgets in a form and get an instant sales prediction.

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Dataset](#-dataset)
- [Project Structure](#-project-structure)
- [Tech Stack](#-tech-stack)
- [Methodology](#-methodology)
- [Results](#-results)
- [Installation](#-installation)
- [Usage](#-usage)
- [API Reference](#-api-reference)
- [Future Improvements](#-future-improvements)
- [License](#-license)

---

## 🔍 Overview

Businesses spend across several advertising channels and want to know how each one affects sales. This project:

1. Explores the classic **Advertising dataset** (200 markets).
2. Trains a **Multiple Linear Regression** model: `Sales = β₀ + β₁·TV + β₂·Radio + β₃·Newspaper`.
3. Evaluates it with MAE, MSE and R².
4. Saves the model with `pickle` (`linear.pkl`).
5. Deploys it behind a **Flask** app with a `/predict` endpoint.

---

## 📊 Dataset

**File:** `advertising.csv` — 200 rows, 4 columns, no missing values.

| Column      | Description                                        | Min  | Mean   | Max   |
|-------------|----------------------------------------------------|------|--------|-------|
| `TV`        | Advertising budget spent on TV                     | 0.7  | 147.04 | 296.4 |
| `Radio`     | Advertising budget spent on Radio                  | 0.0  | 23.26  | 49.6  |
| `Newspaper` | Advertising budget spent on Newspaper              | 0.3  | 30.55  | 114.0 |
| `Sales`     | **Target** — product sales (in thousands of units) | 1.6  | 15.13  | 27.0  |

> Budgets are in thousands of dollars.

**Correlation with Sales:** TV `0.90` · Radio `0.35` · Newspaper `0.16` — TV is by far the strongest predictor.

---

## 📁 Project Structure

```
├── advertising.csv                        # Dataset
├── ML_Multiple_Linear_Regression_.ipynb   # Data loading, training, evaluation, model export
├── linear.pkl                             # Trained LinearRegression model (pickle)
├── app.py                                 # Flask application (serves the model)
├── templates/
│   └── Linear.html                        # Front-end form rendered by Flask
├── requirements.txt                       # Python dependencies
└── README.md
```

> ⚠️ `app.py` renders `templates/Linear.html`. Make sure this file exists in a `templates/` folder before running the app.

---

## 🛠 Tech Stack

- **Language:** Python
- **Data & ML:** NumPy, Pandas, scikit-learn, SciPy
- **Visualization:** Matplotlib
- **Web:** Flask
- **Environment:** Jupyter Notebook (ipykernel)

---

## 🧪 Methodology

1. **Load data** with Pandas.
2. **Select features/target:** `X = [TV, Radio, Newspaper]`, `y = Sales`.
3. **Split** into train/test sets (80% / 20%) with `train_test_split`.
4. **Train** `sklearn.linear_model.LinearRegression`.
5. **Evaluate** on the test set (MAE, MSE, R²) and plot *Actual vs. Predicted*.
6. **Validate** the fit using `scipy.stats.linregress` on actual vs. predicted values.
7. **Export** the model to `linear.pkl` with `pickle`.

### Learned model

| Term      | Coefficient |
|-----------|-------------|
| Intercept (β₀) | 4.4937 |
| TV (β₁)        | 0.0546 |
| Radio (β₂)     | 0.1056 |
| Newspaper (β₃) | 0.0039 |

**Interpretation:** holding other channels fixed, each extra unit of spend on Radio increases sales the most per unit (~0.106), followed by TV (~0.055). Newspaper has almost no effect (~0.004).

---

## ✅ Results

Test-set performance (20% hold-out):

| Metric                    | Value  |
|---------------------------|--------|
| Mean Absolute Error (MAE) | 1.31   |
| Mean Squared Error (MSE)  | 2.76   |
| R² Score                  | 0.902  |

The model explains about **90%** of the variance in sales. The Actual vs. Predicted scatter plot in the notebook sits close to the ideal `y = x` line.

> Note: the split in the notebook has no fixed `random_state`, so exact numbers can vary slightly between runs.

---

## ⚙️ Installation

**1. Clone the repository**

```bash
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>
```

**2. (Recommended) Create a virtual environment**

```bash
python -m venv venv
# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate
```

**3. Install dependencies**

```bash
pip install -r requirements.txt
```

---

## 🚀 Usage

### Run the notebook

```bash
jupyter notebook ML_Multiple_Linear_Regression_.ipynb
```

Run all cells to reproduce training, evaluation, and regenerate `linear.pkl`.

### Run the web app

```bash
python app.py
```

Then open **http://127.0.0.1:5000/** in your browser, enter the TV, Radio and Newspaper budgets, and submit to get the predicted sales.

### Use the model directly in Python

```python
import pickle
import pandas as pd

with open("linear.pkl", "rb") as f:
    model = pickle.load(f)

sample = pd.DataFrame([[230.1, 37.8, 69.2]], columns=["TV", "Radio", "Newspaper"])
print(model.predict(sample))   # ≈ [21.3]
```

---

## 🔌 API Reference

### `POST /predict`

Form-encoded request.

| Field       | Type  | Description                  |
|-------------|-------|------------------------------|
| `variable1` | float | TV advertising budget        |
| `variable2` | float | Radio advertising budget     |
| `variable3` | float | Newspaper advertising budget |

**Example**

```bash
curl -X POST http://127.0.0.1:5000/predict \
  -d "variable1=230.1" -d "variable2=37.8" -d "variable3=69.2"
```

**Success response**

```json
{ "prediction": 21.32 }
```

**Error response**

```json
{ "error": "<error message>" }
```

---

## 🔮 Future Improvements

- Add feature scaling and compare with Ridge / Lasso regression
- Try non-linear models (Random Forest, Gradient Boosting) and interaction terms (e.g. TV × Radio)
- Use k-fold cross-validation and a fixed `random_state` for reproducible results
- Add exploratory data analysis plots (pair plots, correlation heatmap)
- Pin dependency versions in `requirements.txt` and add `scipy`
- Deploy to Render / Heroku / Hugging Face Spaces

---

## 📝 Notes

- The `.pkl` file was created with a specific scikit-learn version. If loading raises a version warning, install the matching scikit-learn version or re-run the notebook to regenerate `linear.pkl`.
- Only load pickle files from sources you trust.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE). *(Add a `LICENSE` file, or change this section to your preferred license.)*

---

## 🙋 Author

**Your Name** — [GitHub](https://github.com/<your-username>) · [LinkedIn](https://www.linkedin.com/in/<your-profile>)

⭐ If you found this project useful, consider giving it a star!
