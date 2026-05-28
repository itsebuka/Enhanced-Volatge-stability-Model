# ⚡ Enhanced Voltage Stability Model

> A machine learning system for predicting electrical grid voltage stability, built for academic study and real-world application to the Nigerian power grid.

---

## 📌 Overview

This project applies supervised machine learning to the problem of **binary voltage stability classification** in electrical power systems. Using the [UCI Electrical Grid Stability Simulated Dataset](https://archive.ics.uci.edu/dataset/471/electrical+grid+stability+simulated+data), two ML classifiers were trained, evaluated, and compared:

- **Logistic Regression** — a linear classifier
- **Decision Tree Classifier** — a non-linear, tree-based classifier

The final model includes an **operational threshold recommendation for NERC** (Nigerian Electricity Regulatory Commission) to trigger emergency grid intervention.

---

## 📊 Model Performance Summary

| Metric | Logistic Regression | Decision Tree |
|---|---|---|
| Accuracy | 81.85% | **85.85%** |
| Balanced Accuracy | 79.29% | **85.50%** |
| F1-Score | 0.7364 | **0.8117** |
| CV Accuracy Mean | 0.8154 | **0.8455** |
| ROC-AUC | 0.892 | — |

> ✅ **Best Model: Decision Tree Classifier (85.85% accuracy)**  
> The Decision Tree outperformed Logistic Regression because voltage stability in power systems is fundamentally **non-linear** — the tau (reaction time) thresholds do not have a simple linear relationship with grid stability states.

---

## 🔑 Key Findings

**Top features driving grid instability (by absolute impact):**

| Rank | Feature | Description | Impact |
|---|---|---|---|
| 1 | τ4 | Consumer 3 reaction time | 0.1530 |
| 2 | τ3 | Consumer 2 reaction time | 0.1462 |
| 3 | τ2 | Consumer 1 reaction time | 0.1457 |
| 4 | τ1 | Producer adaptation time | 0.1284 |
| 5 | g1 | Producer price elasticity | 0.1150 |

**Stabilizing variables:** p1, p2, p4 (positive Logistic Regression coefficients)  
**Destabilizing variables:** τ1–τ4, g1–g4 (negative coefficients → longer reaction times = greater instability risk)

---

## 🚨 NERC Operational Threshold Recommendation

> **Recommended Threshold: 75% (0.75)**

The model should be configured so that the grid is only declared **SAFE** when the model's stability confidence is **≥ 75%**. If confidence falls below this threshold, **emergency action should be triggered automatically.**

This prioritizes catching genuine instability events (false negatives are catastrophic) over the inconvenience of occasional false alarms (false positives are manageable).

---

## 🗂 Dataset

- **Source:** [UCI Machine Learning Repository — Electrical Grid Stability Simulated Data](https://archive.ics.uci.edu/dataset/471/electrical+grid+stability+simulated+data)
- **Size:** 10,000 samples
- **System:** 4-node star topology (1 producer + 3 consumers)
- **Features:** τ1–τ4 (reaction times), p1–p4 (nominal power balance), g1–g4 (price elasticity)
- **Target:** `stabf` → binary: `stable (1)` / `unstable (0)`
- **Class distribution:** 63.8% unstable / 36.2% stable (slight imbalance)
- **Train/Test split:** 8,000 / 2,000 samples

---

## 🛠 Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.13 | Core language |
| scikit-learn | ML models, metrics, cross-validation |
| pandas | Data loading and manipulation |
| NumPy | Numerical operations |
| Matplotlib | Plotting ROC curves, feature importance |
| Seaborn | Confusion matrix heatmaps |
| joblib | Model persistence (.pkl) |
| Google Colab / Jupyter | Development environment |
| Kaggle | Dataset sourcing |

---

## 🚀 How to Run

### 1. Clone the repository
```bash
git clone https://github.com/itsebuka/Enhanced-Volatge-stability-Model.git
cd Enhanced-Volatge-stability-Model
```

### 2. Install dependencies
```bash
pip install scikit-learn pandas numpy matplotlib seaborn joblib
```

### 3. Run the model
```bash
python voltage_stability_prediction_model_3_0.py
```
Or open `Voltage_Stability_Prediction_model_3_0.ipynb` in Google Colab / Jupyter Notebook and run cells sequentially.

---

## 📁 Repository Structure

```
Enhanced-Volatge-stability-Model/
│
├── Voltage_Stability_Prediction_model_3_0.ipynb   # Main Jupyter Notebook
├── voltage_stability_prediction_model_3_0.py       # Python script version
├── Voltage stability Model 2.0.0/                  # Earlier model version
├── README.md
├── LICENSE
└── .gitignore
```

---

## 📈 Output Artifacts

After running the model, the following are generated:

- `outputs/feature_importance_logistic.csv` — LR feature weights
- `outputs/feature_importance_decision_tree.csv` — DT feature importances
- `outputs/prediction_metadata.csv` — Per-prediction confidence scores
- `outputs/model_metrics.pkl` — Serialized evaluation metrics
- `models/voltage_stability_model.pkl` — Saved Logistic Regression model
- `models/voltage_scaler.pkl` — Saved StandardScaler
- `models/config.pkl` — Saved model configuration

---

## ⚠️ Limitations

- The model is trained on a **simulated 4-node grid** — it does not reflect the actual topological complexity of Nigeria's national grid, which spans thousands of nodes across multiple regions.
- For real NERC deployment, this would need to be retrained on **actual SCADA telemetry data** from Nigeria's transmission network.
- The Decision Tree may overfit on noisier, real-world grid data — ensemble methods like **Random Forest** or **XGBoost** are recommended as next steps.

---

## 🔭 Future Work

- [ ] Scale to Nigeria's full grid topology using real SCADA data
- [ ] Implement Random Forest / XGBoost for improved generalization
- [ ] Build a real-time early warning dashboard
- [ ] Explore deep learning approaches (LSTM for time-series grid data)
- [ ] Integrate with NERC's existing operational infrastructure

---

## 👤 Author

**Eleogu Chukwuebuka Joseph**  
Electrical/Electronics Engineering  
Pan Atlantic University — Matric No. 23120211012  
Course: GET 307 — Introduction to AI, Machine Learning and Convergent Technologies  
Supervisor: Engr. Abdulbasit Makinde

---

## 📄 License

This project is licensed under the [Unlicense](LICENSE) — free for public use.
