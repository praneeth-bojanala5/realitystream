# 🔍 SHAP: Model Interpretability with SHapley Values

**SHAP (SHapley Additive exPlanations)** is a powerful library for explaining the predictions of machine learning models. It connects game theory with local explanations, using **Shapley values** to fairly distribute the "credit" of a prediction across input features.

---

## 🎯 Why Use SHAP?

Understanding why a model made a certain prediction is crucial for:

- 📈 **Building Trust** in machine learning models
- ✅ **Debugging** model behavior
- 🏥 **High-Stakes Applications** like healthcare, finance, and legal domains
- ⚖️ **Compliance** with regulations (e.g., GDPR)

---

## 🧠 How It Works

SHAP computes the contribution of each feature to the prediction using the concept of **Shapley values** from cooperative game theory. The idea is:

> _"How much does each feature contribute to the difference between the actual prediction and the average prediction?"_

---

## 🛠 Supported Models

SHAP supports many types of models:

- Tree-based models (XGBoost, LightGBM, CatBoost, RandomForest)
- Linear models (Logistic Regression, Linear Regression)
- Deep Learning models (Keras, PyTorch via DeepExplainer)
- Black-box models (via KernelExplainer)

---

## 📈 Visualizations

SHAP provides beautiful and insightful plots:

- `summary_plot`: global feature importance
- `force_plot`: detailed local explanations
- `dependence_plot`: interaction effects between features
- `waterfall_plot`: cumulative view of prediction explanations

---

## 🚀 Try It on Colab

You can try out SHAP right away with this hands-on notebook:

👉 [Colab: SHAP Tutorial](https://colab.research.google.com/github/dphi-official/Machine_Learning_Bootcamp/blob/master/SHAP.ipynb)

This notebook covers:
- Loading data
- Training a model
- Generating and visualizing SHAP values
- Interpreting results step-by-step

---

## 🧪 Quick Example

```python
import shap
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
import matplotlib.pyplot as plt

# Load data
X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42)

# Train model
model = RandomForestClassifier()
model.fit(X_train, y_train)

# SHAP explainer and values
explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(X_test)

# Summary plot for class 0
shap.summary_plot(shap_values[0], X_test, feature_names=load_iris().feature_names)
```

TO DO: Also implement [LIME](https://medium.com/@anshulgoel991/model-exploitability-using-shap-shapley-additive-explanations-and-lime-local-interpretable-cb4f5594fc1a)