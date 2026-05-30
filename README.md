# 🎓 Student Score Prediction — End-to-End ML Project

> An end-to-end machine learning web application that predicts a student's **math score** based on demographic and academic background factors, deployed via a Flask REST API.

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-Web_App-000000?style=flat&logo=flask)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-Boosting-0073B7?style=flat)
![CatBoost](https://img.shields.io/badge/CatBoost-Boosting-FFCC00?style=flat)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Demo](#-demo)
- [Project Structure](#-project-structure)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)
- [How It Works](#-how-it-works)
- [Input Features](#-input-features)
- [Model Training](#-model-training)
- [API Routes](#-api-routes)
- [Author](#-author)

---

## 🔍 Overview

This project tackles a classic supervised regression problem: **predicting a student's math exam score** using features like gender, race/ethnicity, parental education level, lunch type, test preparation status, and scores in reading and writing.

The project follows a **production-grade ML pipeline** with modular components for data ingestion, transformation, model training, evaluation, and prediction — all served through a Flask web application.

---

## 🎥 Demo

| Home Page | Prediction Form | Score Result |
|-----------|----------------|--------------|
| Landing page with project intro | 3-step form collecting student data | Animated score gauge with grade |

To run locally, follow the [Getting Started](#-getting-started) section below.

---

## 📁 Project Structure

```
ml-student-score-prediction/
│
├── notebook/
│   └── data/                    # Raw dataset (CSV) used for EDA & training
│
├── src/
│   ├── components/
│   │   ├── data_ingestion.py    # Loads and splits the dataset
│   │   ├── data_transformation.py  # Feature engineering & preprocessing
│   │   └── model_trainer.py     # Trains and evaluates multiple models
│   │
│   ├── pipeline/
│   │   ├── train_pipeline.py    # Orchestrates the training workflow
│   │   └── prediction_pipeline.py  # Loads artifacts & runs inference
│   │
│   ├── exception.py             # Custom exception handler
│   ├── logger.py                # Logging configuration
│   └── utils.py                 # Shared utility functions
│
├── templates/
│   ├── index.html               # Landing page
│   └── home.html                # Prediction form & results page
│
├── app.py                       # Flask application entry point
├── requirements.txt             # Python dependencies
├── setup.py                     # Package setup
└── .gitignore
```

---

## ✨ Features

- **End-to-end ML pipeline** — from raw data to live prediction
- **Multiple model comparison** — trains Linear Regression, Ridge, Lasso, KNN, Decision Tree, Random Forest, XGBoost, CatBoost, AdaBoost, and picks the best
- **Custom preprocessing pipeline** — handles categorical encoding and numerical scaling via scikit-learn `Pipeline` and `ColumnTransformer`
- **Artifact persistence** — trained model and preprocessor saved as `.pkl` files using `dill`
- **Flask REST API** — clean `GET/POST` routes for serving predictions
- **Interactive web UI** — 3-step animated form with live score sliders and animated result dial
- **Production-ready** — `gunicorn`-compatible, modular `src` package with custom logging and exceptions

---

## 🛠 Tech Stack

| Category | Tools |
|----------|-------|
| Language | Python 3.8+ |
| Web Framework | Flask, Gunicorn |
| ML Libraries | scikit-learn, XGBoost, CatBoost |
| Data Processing | NumPy, Pandas |
| Visualization (EDA) | Matplotlib, Seaborn |
| Serialization | Dill |
| Frontend | HTML5, CSS3, Jinja2 |
| Notebook | Jupyter |

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8 or higher
- pip

### 1. Clone the Repository

```bash
git clone https://github.com/nishant-chitmulwar/ml-student-score-prediction.git
cd ml-student-score-prediction
```

### 2. Create a Virtual Environment

```bash
python -m venv venv

# On Windows
venv\Scripts\activate

# On macOS/Linux
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

> The `setup.py` will auto-install the `src` package as a local module if you run `pip install -e .`

### 4. Run the Application

```bash
python app.py
```

The app will start at **`http://0.0.0.0:5000`**. Open your browser and navigate to `http://localhost:5000`.

---

## ⚙️ How It Works

```
User Input (Web Form)
        │
        ▼
  CustomData Object
  (prediction_pipeline.py)
        │
        ▼
  Preprocessor (.pkl)
  ┌─────────────────────────┐
  │  • StandardScaler       │  ← Numerical features
  │  • OneHotEncoder        │  ← Categorical features
  └─────────────────────────┘
        │
        ▼
  Trained Model (.pkl)
  (Best model selected from evaluation)
        │
        ▼
  Predicted Math Score
  (Rendered back to home.html)
```

Trained artifacts (preprocessor + model) are saved to an `artifacts/` directory during the training pipeline and loaded at inference time.

---

## 📊 Input Features

The model uses the following 7 features to predict the math score:

| Feature | Type | Values |
|---------|------|--------|
| `gender` | Categorical | `male`, `female` |
| `race_ethnicity` | Categorical | `group A` – `group E` |
| `parental_level_of_education` | Categorical | `some high school`, `high school`, `some college`, `associate's degree`, `bachelor's degree`, `master's degree` |
| `lunch` | Categorical | `standard`, `free/reduced` |
| `test_preparation_course` | Categorical | `none`, `completed` |
| `reading_score` | Numerical | 0 – 100 |
| `writing_score` | Numerical | 0 – 100 |

**Target variable:** `math_score` (continuous, 0–100)

---

## 🧠 Model Training

The training pipeline evaluates the following models and selects the best based on **R² score**:

- Linear Regression
- Ridge Regression
- Lasso Regression
- K-Nearest Neighbors
- Decision Tree
- Random Forest
- **XGBoost** ← typically top performer
- **CatBoost** ← typically top performer
- AdaBoost

Hyperparameter tuning is performed via `GridSearchCV`. The best model and preprocessor are serialized to the `artifacts/` folder using `dill`.

---

## 🌐 API Routes

| Route | Method | Description |
|-------|--------|-------------|
| `/` | `GET` | Serves the landing page (`index.html`) |
| `/preictdata` | `GET` | Renders the prediction form (`home.html`) |
| `/preictdata` | `POST` | Accepts form data, runs inference, returns predicted score |

### POST Request Fields

```
gender, ethnicity, parental_level_of_education,
lunch, test_preparation_course, reading_score, writing_score
```

---

## 👨‍💻 Author

**Nishant Chitmulwar**
📧 [chitmulwarnishant0105@gmail.com](mailto:chitmulwarnishant0105@gmail.com)
🐙 [@nishant-chitmulwar](https://github.com/nishant-chitmulwar)

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).

---

*Built as an end-to-end machine learning project — from EDA and model selection to Flask deployment.*
