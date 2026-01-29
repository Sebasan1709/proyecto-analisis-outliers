# 🕵️ Fraud Detection — EDA & Anomaly Analysis

## 📌 Descripción del proyecto
Este proyecto aborda el problema de **detección de fraude en transacciones financieras** mediante:

- Análisis exploratorio de datos (EDA)
- Detección de anomalías y outliers
- Modelos supervisados y no supervisados
- Análisis de comportamiento a nivel de usuario

El objetivo es **identificar patrones anómalos y señales de fraude**, priorizando enfoques **explicables y reproducibles**.

---

## 📊 Dataset
**Archivo:** `fraud_detection_transactions.csv`  
**Observaciones:** 50,000  
**Variables:** 21  

### Variables principales
- `Transaction_Amount`
- `Account_Balance`
- `Transaction_Distance`
- `Risk_Score`
- `Timestamp`
- `Fraud_Label` (variable objetivo)

Incluye información transaccional, histórica, contextual y de comportamiento del usuario.

---

## 🧪 Estructura del proyecto

proyecto-analisis-outliers/
│
├── data/
│ ├── raw/ # Dataset original
│ ├── processed/ # Dataset enriquecido tras EDA
│
├── notebooks/
│ ├── 01_eda.ipynb
│ ├── 02_anomaly_detection.ipynb
│ ├── 03_modeling.ipynb
│ └── 04_user_analysis.ipynb
│
├── src/ # Código reutilizable
│
├── requirements.txt / pyproject.toml
└── README.md


---

## 🔍 Análisis Exploratorio de Datos (EDA)

### Outliers clásicos
- Detectados mediante el método **IQR**
- Solo `Transaction_Amount` mostró outliers univariados relevantes (~4.5%)

### Anomalías contextuales
Se creó la variable:

amount_vs_avg_7d = Transaction_Amount / Avg_Transaction_Amount_7d


Resultados:
- Percentil 99 ≈ **8.6×**
- Máximo ≈ **61×**
- Señal altamente informativa para fraude

---

## 🧠 Feature Engineering

Se generaron señales explicables:

- `flag_amt_vs_avg_p99`
- `flag_far_distance`
- `flag_high_risk`
- `flag_night_tx`

Y un score agregado:

fraud_score_simple = suma de flags


### Resultados clave
| Fraud Score | % Fraude |
|------------|----------|
| 0 | 24.7% |
| 1 | 42.5% |
| 2 | 88.7% |
| 3 | 100% |

---

## 🤖 Modelado

### Logistic Regression (supervisado)
- Features numéricas + flags + score
- Escalado con `StandardScaler`

**Resultados:**
- ROC AUC ≈ **0.95**
- Recall fraude ≈ **90%**
- Modelo interpretable

### Isolation Forest (no supervisado)
- ~10% de transacciones detectadas como anómalas
- **79.8%** de fraude dentro de anomalías
- Útil cuando no hay etiquetas

---

## 👤 Análisis por Usuario

Agregación por `User_ID`:
- Tasa de fraude
- Severidad (`fraud_score_simple`)
- Reincidencia

Hallazgos:
- Usuarios con **100% de fraude**
- Usuarios con múltiples transacciones de alto score
- Evidencia clara de *bad actors*

---

## ⏱️ Análisis Temporal

Se analizaron ráfagas de transacciones (≤10 minutos entre operaciones).

Resultado:
- Las ráfagas **no presentan mayor tasa de fraude**
- El fraude se distribuye en el tiempo → comportamiento más sigiloso

---

## 🧾 Conclusiones

1. Los outliers clásicos no capturan completamente el fraude.
2. Las anomalías contextuales son más informativas.
3. Los modelos supervisados superan a los no supervisados cuando existen etiquetas.
4. Un enfoque explicable puede lograr alto desempeño.
5. El fraude no siempre ocurre en ráfagas temporales.

---

## ⚙️ Tecnologías

- Python 3.11
- pandas, numpy, matplotlib
- scikit-learn
- Poetry + Conda
- Jupyter Lab

---

## 🚀 Próximos pasos

- Optimización de umbrales según costo de error
- Modelos avanzados (XGBoost, LightGBM)
- Monitoreo en tiempo real
- Explicabilidad con SHAP

---

## ✍️ Autor
**Juan Sebastian Angel Perez**  
Proyecto de análisis de datos y detección de fraude para la clase de Big Data



