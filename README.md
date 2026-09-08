# ❤️ Heart Disease Prediction

> A Machine Learning project for predicting the presence of heart disease from clinical patient data.

## 📌 Overview

This project is a **Binary Classification** system that predicts whether a patient has heart disease (`HeartDisease`) based on clinical features.

The project compares four different Machine Learning and Deep Learning models and then combines them into a **Hybrid Ensemble** using **Weighted Soft Voting**.

The project also includes an interactive **Gradio** interface where users can enter patient information, select a model, and get a prediction.

---

## 📊 Dataset

The project uses the `heart.csv` dataset.

- **Records:** 918
- **Columns:** 12
- **Input Features:** 11
- **Target:** `HeartDisease`
- **Task:** Binary Classification

### Target

- `0` → No Heart Disease
- `1` → Heart Disease

---

## 🧬 Features

| Feature | Description |
|---|---|
| `Age` | Patient age |
| `Sex` | Sex (M / F) |
| `ChestPainType` | Chest pain type (ATA, NAP, ASY, TA) |
| `RestingBP` | Resting blood pressure |
| `Cholesterol` | Cholesterol level |
| `FastingBS` | Fasting blood sugar (0/1, above 120 or not) |
| `RestingECG` | Resting electrocardiogram result (Normal, ST, LVH) |
| `MaxHR` | Maximum heart rate achieved |
| `ExerciseAngina` | Exercise-induced angina (Y/N) |
| `Oldpeak` | ST depression |
| `ST_Slope` | Slope of the ST segment (Up, Flat, Down) |

---

## ⚙️ Preprocessing

The project uses different preprocessing pipelines depending on the model type.

### Scale-sensitive models

Used for:

- Logistic Regression
- Deep Learning (MLP)

Processing:

- `StandardScaler` for numerical features
- `OneHotEncoder` for categorical features

### Tree-based models

Used for:

- Random Forest
- XGBoost

Processing:

- `OneHotEncoder` for categorical features
- Numerical features passed without scaling

---

## 🤖 Models

### 1. Logistic Regression

Five different solvers were evaluated using Cross-Validation:

- `liblinear`
- `lbfgs`
- `sag`
- `newton-cg`
- `saga`

The best solver based on F1 Score was:

`liblinear`

### 2. Random Forest

`RandomizedSearchCV` was used for hyperparameter tuning with **100 iterations** over a wide hyperparameter search space.

### 3. XGBoost

`RandomizedSearchCV` was also used for XGBoost hyperparameter tuning.

### 4. Deep Learning — MLP

An automatic architecture search was performed across **7 different architectures**, varying:

- Number of layers
- Number of neurons
- Dropout
- Learning Rate

The best architecture was selected based on Validation Accuracy.

### 5. Hybrid Ensemble

The four models were combined using **Weighted Soft Voting**.

| Model | Weight |
|---|---:|
| Logistic Regression | 0.15 |
| Random Forest | 0.25 |
| XGBoost | 0.35 |
| Deep Learning | 0.25 |

---

## 📈 Model Performance

Results on the **Test Set (20% of the dataset, Stratified Split)**:

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| **Hybrid Ensemble** | **91.30%** | **91.35%** | **93.14%** | **92.23%** | **93.81%** |
| Logistic Regression | 89.67% | 88.79% | 93.14% | 90.91% | 92.96% |
| XGBoost | 89.67% | 90.29% | 91.18% | 90.73% | 93.47% |
| Random Forest | 89.13% | 88.68% | 92.16% | 90.38% | 92.69% |
| Deep Learning (MLP) | 86.41% | 88.89% | 86.27% | 87.56% | 93.46% |

### 🏆 Best Model

The **Hybrid Ensemble** achieved the best overall performance:

- 🎯 Accuracy: **91.30%**
- 🎯 Precision: **91.35%**
- 🎯 Recall: **93.14%**
- 🎯 F1 Score: **92.23%**
- 🎯 ROC-AUC: **93.81%**

---

## 🔍 Feature Importance

Feature importance was analyzed for the tree-based models, particularly Random Forest and XGBoost.

The most influential features included:

- `ST_Slope`
- `ChestPainType`
- `Cholesterol`
- `ExerciseAngina`

---

## 🖥️ Gradio Interface

The project includes an interactive Gradio application.

Users can:

1. Enter the patient's 11 features.
2. Select a Machine Learning model.
3. Click `Predict`.
4. View the prediction and probability.

### 🎛️ Interface

![gradio-interface](https://raw.githubusercontent.com/MohamedAymanHosny/Heart-Disease-Prediction/Screenshots/gradio-interface.png)

### 🔮 Prediction Example

![Prediction Result](screenshots/prediction.png)

---

## 📊 Visual Results

### Model Comparison

![Model Comparison](screenshots/model-comparison.png)

### Confusion Matrix

![Confusion Matrix](screenshots/confusion-matrix.png)

### ROC Curve

![ROC Curve](screenshots/roc-curve.png)

### Feature Importance

![Feature Importance](screenshots/feature-importance.png)

> 💡 If your screenshot filenames are different, update the image paths above to match your `screenshots/` folder.

---

## 🎯 Prediction Output

The user enters the 11 patient features using:

- 🎚️ Sliders
- 🔽 Dropdowns
- 🔘 Radio Buttons

The user can choose:

- Logistic Regression
- Random Forest
- XGBoost
- Deep Learning (MLP)
- Hybrid Ensemble

The application returns:

- **Prediction:** `Heart Disease Detected` or `No Heart Disease`
- **Probability of Heart Disease:** Probability percentage
- **Model Used:** Selected model
- **Test Set Accuracy:** Accuracy of the selected model

### Example

```text
Prediction: Heart Disease Detected
Probability of Heart Disease: 78.32%
Model used: Hybrid Ensemble
Weighted soft-voting ensemble of all 4 models.
```

---

## 🚀 How to Run

### 📓 Run the Notebook

1. Open `heart_disease_project.ipynb` in Google Colab or Jupyter.
2. Make sure `heart.csv` is available in the same path.
3. For Google Colab, the expected path is `/content/heart.csv`.
4. Run the notebook cells from top to bottom.

### 🌐 Run the Gradio Application

```bash
python heart_disease_app.py
```

Make sure `heart.csv` is in the same folder.

On the first run, the models are trained automatically and stored in:

```text
model_cache/
```

Future runs can load the cached models instead of retraining them.

---

## 📦 Installation

Install the required libraries:

```bash
pip install numpy pandas matplotlib scikit-learn xgboost tensorflow gradio joblib
```

---

## 🛠️ Technologies

- 🐍 **Python**
- 🐼 **Pandas / NumPy** — Data processing
- 🤖 **scikit-learn** — Pipelines, preprocessing, Logistic Regression, Random Forest, Cross-Validation, RandomizedSearchCV
- 🚀 **XGBoost** — `XGBClassifier`
- 🧠 **TensorFlow / Keras** — MLP model
- 🎨 **Gradio** — Interactive user interface
- 💾 **Joblib** — Model saving and loading
- 📈 **Matplotlib** — Learning Curves, Confusion Matrix, ROC Curve, Feature Importance

---

## 📁 Project Structure

```text
Heart-Disease-Prediction/
│
├── heart_disease_project.ipynb
├── heart_disease_app.py
├── heart.csv
├── README.md
│
├── screenshots/
│   ├── gradio-interface.png
│   ├── prediction.png
│   ├── model-comparison.png
│   ├── confusion-matrix.png
│   ├── roc-curve.png
│   └── feature-importance.png
│
└── model_cache/
    ├── logistic_regression.joblib
    ├── random_forest.joblib
    ├── xgboost.joblib
    ├── dl_preprocessor.joblib
    ├── mlp_model.keras
    └── test_accuracies.joblib
```

---

## 🔄 Project Workflow

```text
heart.csv
   ↓
Data Loading & EDA
   ↓
Train / Test Split
   ↓
Preprocessing
   ↓
┌───────────────────────┐
│ Logistic Regression   │
│ Random Forest         │
│ XGBoost               │
│ Deep Learning (MLP)   │
└───────────────────────┘
   ↓
Model Evaluation
   ↓
Hybrid Ensemble
   ↓
Final Prediction
   ↓
Gradio Interface
```

---

## 📚 Notebook Structure

1. Setup
2. Load Data & EDA
3. Train/Test Split & Preprocessing
4. Logistic Regression
5. Random Forest — Tuned
6. XGBoost — Tuned
7. Deep Learning — MLP
8. Hybrid Ensemble & Model Comparison
9. Gradio App

---

## ⚠️ Disclaimer

This project is intended for **educational and research purposes only**.

The predictions generated by this application should not be considered a medical diagnosis or a replacement for professional medical advice.

---

## 👨‍💻 Author

**Your Name**

GitHub: https://github.com/MohamedAymanHosny

LinkedIn: https://www.linkedin.com/in/mohamed-ayman-hosny-05361a289

---

⭐ If you find this project useful, feel free to give it a star!
