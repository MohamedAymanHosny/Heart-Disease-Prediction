# ❤️ Heart Disease Prediction — نظام التنبؤ بأمراض القلب

## 📌 فكرة المشروع

مشروع تصنيف ثنائي (Binary Classification) بيتنبأ باحتمالية إصابة مريض بمرض القلب (`HeartDisease`) بناءً على بيانات إكلينيكية.

الداتا المستخدمة هي `heart.csv` بحجم **918 صف و12 عمود**، منها **11 feature** وعمود الهدف `HeartDisease`.

المشروع بيقارن بين 4 موديلات مختلفة:

- Logistic Regression
- Random Forest
- XGBoost
- Deep Learning (MLP)

وبعدين بيجمعهم في **Hybrid Ensemble** باستخدام **Weighted Soft Voting** بأوزان محددة، بهدف تحسين الأداء والوصول لأفضل نتيجة.

وفي النهاية، المشروع بيحتوي على واجهة تفاعلية باستخدام **Gradio** تسمح للمستخدم بإدخال بيانات مريض جديد واختيار الموديل المستخدم للتنبؤ.

---

## 🧬 الـ Features

الداتا فيها 11 feature، والـ target هو `HeartDisease`.

| Feature | الوصف |
|---|---|
| `Age` | عمر المريض |
| `Sex` | النوع (M / F) |
| `ChestPainType` | نوع ألم الصدر (ATA, NAP, ASY, TA) |
| `RestingBP` | ضغط الدم وقت الراحة |
| `Cholesterol` | نسبة الكوليسترول |
| `FastingBS` | سكر الصيام (0/1، أعلى من 120 أو لأ) |
| `RestingECG` | نتيجة رسم القلب وقت الراحة (Normal, ST, LVH) |
| `MaxHR` | أقصى معدل ضربات قلب تم الوصول له |
| `ExerciseAngina` | ذبحة صدرية مرتبطة بالمجهود (Y/N) |
| `Oldpeak` | مقدار انخفاض ST |
| `ST_Slope` | ميل قطعة ST (Up, Flat, Down) |

الأعمدة الرقمية بتتعالج باستخدام `StandardScaler`، والأعمدة الفئوية باستخدام `OneHotEncoder`.

وفيه Pipeline منفصل للـ preprocessing حسب نوع الموديل:
- موديلات حساسة للـ scale: Logistic Regression و Deep Learning (MLP)
- موديلات شجرية: Random Forest و XGBoost

---

## 🤖 الـ Models المستخدمة

### 1. Logistic Regression

تم تجربة 5 solvers مختلفة:

- `liblinear`
- `lbfgs`
- `sag`
- `newton-cg`
- `saga`

وتم اختيار الأفضل بناءً على Cross-Validation باستخدام **F1 Score**، وكانت النتيجة الأفضل مع:

`liblinear`

### 2. Random Forest

تم استخدام `RandomizedSearchCV` لعمل Hyperparameter Tuning باستخدام **100 iteration** على شبكة واسعة من الـ hyperparameters.

### 3. XGBoost

تم استخدام `RandomizedSearchCV` لعمل Hyperparameter Tuning على مجموعة من الـ hyperparameters الخاصة بـ XGBoost.

### 4. Deep Learning (MLP)

تم عمل Architecture Search على **7 معماريات مختلفة**، مع تجربة عدد الطبقات والـ neurons والـ Dropout والـ Learning Rate، وتم اختيار أفضل Architecture بناءً على Validation Accuracy.

### 5. Hybrid Ensemble

تم دمج النماذج الأربعة باستخدام **Weighted Soft Voting** بالأوزان التالية:

| Model | Weight |
|---|---:|
| Logistic Regression | 0.15 |
| Random Forest | 0.25 |
| XGBoost | 0.35 |
| Deep Learning | 0.25 |

---

## 📊 Model Performance

النتائج على الـ Test Set، باستخدام **20% من الداتا** مع **Stratified Split**:

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| **Hybrid Ensemble** | **91.30%** | **91.35%** | **93.14%** | **92.23%** | **93.81%** |
| Logistic Regression | 89.67% | 88.79% | 93.14% | 90.91% | 92.96% |
| XGBoost | 89.67% | 90.29% | 91.18% | 90.73% | 93.47% |
| Random Forest | 89.13% | 88.68% | 92.16% | 90.38% | 92.69% |
| Deep Learning (MLP) | 86.41% | 88.89% | 86.27% | 87.56% | 93.46% |

### 🏆 Best Model

الـ **Hybrid Ensemble** حقق أفضل نتيجة في المشروع:

- **Accuracy:** 91.30%
- **Precision:** 91.35%
- **Recall:** 93.14%
- **F1 Score:** 92.23%
- **ROC-AUC:** 93.81%

---

## 🔍 Feature Importance

تم تحليل أهمية الـ features في الموديلات الشجرية، خصوصًا Random Forest و XGBoost.

من أبرز الـ features المؤثرة:

- `ST_Slope`
- `ChestPainType`
- `Cholesterol`
- `ExerciseAngina`

وسيتم عرض الرسومات الخاصة بالـ Feature Importance في قسم الصور بالأسفل.

---

## 🖥️ Gradio Interface

المشروع يحتوي على Interactive Interface باستخدام Gradio.

المستخدم يقدر:

1. يدخل بيانات المريض.
2. يختار الـ Model.
3. يضغط على `Predict`.
4. يحصل على نتيجة التنبؤ والـ probability.

### واجهة التطبيق

![Gradio Interface](screenshots/gradio-interface.png)

### مثال على Prediction

![Prediction Result](screenshots/prediction.png)

---

## 📈 Model Evaluation

### Model Comparison

![Model Comparison](screenshots/model-comparison.png)

### Confusion Matrix

![Confusion Matrix](screenshots/confusion-matrix.png)

### ROC Curve

![ROC Curve](screenshots/roc-curve.png)

### Feature Importance

![Feature Importance](screenshots/feature-importance.png)

> لو أسماء الصور عندك مختلفة، غيّر أسماء الملفات داخل روابط الصور فقط مع الحفاظ على نفس فولدر `screenshots`.

---

## 🎯 شكل الـ Prediction

المستخدم بيدخل الـ 11 feature عن طريق:

- Sliders
- Dropdowns
- Radio Buttons

وبعدين يختار الـ Model:

- Logistic Regression
- Random Forest
- XGBoost
- Deep Learning (MLP)
- Hybrid Ensemble

النظام بيرجع:

- **Prediction:** `Heart Disease Detected` أو `No Heart Disease`
- **Probability of Heart Disease:** النسبة المئوية للاحتمال
- **Model used:** اسم الموديل المستخدم
- **Model test-set accuracy:** دقة الموديل على الـ Test Set، أو ملاحظة توضح استخدام الـ Ensemble

### Example Output

```text
Prediction: Heart Disease Detected
Probability of Heart Disease: 78.32%
Model used: Hybrid Ensemble
Weighted soft-voting ensemble of all 4 models.
```

---

## ⚙️ طريقة التشغيل

### تشغيل الـ Notebook

للتدريب والتحليل الكامل:

1. افتح `heart_disease_project.ipynb` على Google Colab أو Jupyter.
2. تأكد إن ملف `heart.csv` موجود في نفس المسار.
3. في حالة Google Colab يكون المسار:
   `/content/heart.csv`
4. شغّل الخلايا بالترتيب من فوق لتحت.

### تشغيل واجهة Gradio

```bash
python heart_disease_app.py
```

لازم `heart.csv` يكون موجود في نفس الفولدر.

أول مرة يتم فيها تشغيل التطبيق، الموديلات يتم تدريبها تلقائيًا وتتخزن في:

```text
model_cache/
```

وبالتالي التشغيلات التالية تقدر تحمل الموديلات المحفوظة بدل إعادة التدريب من البداية.

---

## 📦 المكتبات المطلوبة

```text
numpy
pandas
matplotlib
scikit-learn
xgboost
tensorflow
gradio
joblib
```

يمكن تثبيتها باستخدام:

```bash
pip install numpy pandas matplotlib scikit-learn xgboost tensorflow gradio joblib
```

---

## 🛠️ Technologies

- **Python**
- **Pandas / NumPy** — معالجة البيانات
- **scikit-learn** — Pipelines, Preprocessing, Logistic Regression, Random Forest, Cross-Validation, RandomizedSearchCV
- **XGBoost** — `XGBClassifier`
- **TensorFlow / Keras** — بناء وتدريب الـ MLP
- **Gradio** — الواجهة التفاعلية
- **Joblib** — حفظ وتحميل الموديلات المدربة
- **Matplotlib** — Learning Curves, Confusion Matrix, ROC Curve و Feature Importance

---

## 📁 Project Structure

```text
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
│                       │
├── Logistic Regression │
├── Random Forest       │
├── XGBoost             │
└── Deep Learning MLP   │
        ↓
 Model Evaluation
        ↓
 Hybrid Ensemble
        ↓
 Final Prediction
        ↓
    Gradio UI
```

---

## 📚 Notebook Structure

الـ Notebook الأساسي منظم في المراحل التالية:

1. Setup
2. Load Data & EDA
3. Train/Test Split & Preprocessing
4. Logistic Regression
5. Random Forest (Tuned)
6. XGBoost (Tuned)
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

GitHub: `https://github.com/USERNAME`

LinkedIn: `https://www.linkedin.com/in/USERNAME`
