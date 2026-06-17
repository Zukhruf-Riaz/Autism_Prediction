# 🧠 Autism Screening Prediction App

## Overview

The Autism Screening Prediction App is a local, AI-assisted web tool that predicts the likelihood of Autism Spectrum Disorder (ASD) based on the AQ-10 questionnaire and personal information — without requiring any machine learning knowledge from the user.

It walks the user through a two-page form, computes an AQ-10 score automatically, runs an XGBoost prediction model, and generates a detailed report with a SHAP-based visual explanation of which factors influenced the result most.

**What the app does for you:**
- Guides users through the standardised AQ-10 autism screening questionnaire
- Collects personal information (age, family history, jaundice, relation to patient)
- Automatically computes the total AQ-10 score
- Predicts Autistic / Not Autistic using a trained XGBoost model
- Generates a plain-English prediction report with a full response summary
- Produces an interactive SHAP force plot explaining the model's decision

> All processing runs locally on your machine. No data is sent to any external server.

---

## Prerequisites

### Software

| Requirement | Purpose | How to Get It |
|---|---|---|
| Python 3.10+ | Runs the web app | [python.org](https://www.python.org/downloads/) |
| pip | Installs Python packages | Included with Python |

### Python Packages

```bash
pip install fastapi uvicorn pandas joblib shap matplotlib python-multipart
```

### Hardware
- Minimum: 4 GB RAM
- No GPU required
- Works on Windows, macOS, and Linux

---

## Pipeline

The app follows a 3-step flow from questionnaire to prediction report.

```
AQ-10 Questionnaire → Personal Information → Prediction Report + SHAP Explanation
```

---

### Step 1  AQ-10 Questionnaire

The user answers 10 standardised questions from the Autism Quotient screening tool. Each question has two options: **Agree** or **Disagree**.

The questions cover areas such as:
- Noticing small sounds others do not
- Reading between the lines in conversation
- Switching focus after interruptions
- Understanding others' intentions and emotions

The AQ-10 total score is computed automatically in the background before moving to Step 2.

---

### Step 2  Personal Information

The user provides additional demographic and health information:

| Field | Options |
|---|---|
| **Age** | Numeric input |
| **Gender** | Male / Female / Others |
| **Autism in immediate family?** | Yes / No |
| **Jaundice at birth?** | Yes / No |
| **Used this app before?** | Yes / No |
| **Country of Residence** | Free text |
| **Relation to patient** | Self / Parent / Relative / Health care professional / Others |

All AQ-10 scores are passed as hidden fields so nothing is lost between pages.

---

### Step 3  Prediction Report

After submission the app runs the XGBoost model and returns a full report containing:

| Output | What It Shows |
|---|---|
| **Prediction** | Autistic (red) or Not Autistic (green) |
| **AQ-10 Response Summary** | Plain-English interpretation of each answer |
| **Personal Information Summary** | All user-provided details in a readable list |
| **SHAP Explanation Link** | Opens an interactive force plot showing feature contributions |

The SHAP force plot shows which features pushed the prediction toward or away from an autism diagnosis, displayed in an interactive browser view.

> **Important:** This tool is a screening aid, not a clinical diagnosis. Always consult a licensed professional for a comprehensive evaluation.

---

## Project Structure

```
Autism project/
├── questionnaire.py       # Main FastAPI application
├── xgb_model.pkl          # Trained XGBoost model
├── requirements.txt       # Python dependencies
└── README.md              # This file
```

---

## Installation & Running the App

```bash
# 1. Clone the repository
git clone <your-repo-url>
cd autism-project

# 2. Create and activate a virtual environment
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # macOS / Linux

# 3. Install dependencies
pip install -r requirements.txt

# 4. Run the app
uvicorn questionnaire:app --reload
```

Open your browser at `http://127.0.0.1:8000`.

---

## Model Details

| Property | Value |
|---|---|
| **Algorithm** | XGBoost (Gradient Boosted Trees) |
| **Input Features** | 15 (10 AQ scores + age + result + autism + jaundice + relation) |
| **Output** | Binary classification — Autistic / Not Autistic |
| **Explainability** | SHAP TreeExplainer force plot |
| **Model File** | `xgb_model.pkl` |

---

## Troubleshooting

| Problem | Solution |
|---|---|
| `matplotlib is not installed` error | Run `pip install matplotlib` then restart the server |
| `Internal Server Error` on predict page | Check that all form fields are filled in, especially radio buttons |
| XGBoost version warning on startup | Safe to ignore — model still loads and runs correctly |
| Port already in use | Run `uvicorn questionnaire:app --reload --port 8001` |
| `python-multipart` missing error | Run `pip install python-multipart` |
| SHAP page shows "not available" | Complete a prediction first — SHAP file is generated per run |
