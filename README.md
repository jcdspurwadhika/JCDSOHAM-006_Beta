# JCDSOHAM-006_Beta

# Telco Customer Churn Prediction

Proyek machine learning end-to-end untuk memprediksi risiko *churn* pelanggan perusahaan telekomunikasi 1 bulan ke depan, lengkap dengan analisis eksploratif, pemodelan, evaluasi cost-benefit, dan aplikasi web interaktif untuk mendukung keputusan tim retensi.

**Author:** Lauzia Fadhila Nareswari, Risky Adipratama, Dea Thrisnanti

---

## 🔗 Tautan Proyek

| Resource | Link |
|---|---|
|  Notebook (Analisis Lengkap) | [`Telco_Cust_Churn_FIXED.ipynb`](./Telco_Cust_Churn_FIXED.ipynb) |
|  Aplikasi Streamlit (Live Demo) | [telco-churn-app.streamlit.app](https://telco-churn-app-23mxzztus2mdqyvwxu9bgw.streamlit.app/) |
| 📊 Dashboard Tableau | https://public.tableau.com/views/TelcoCustomerChurn_17830864541630/Dashboard-TelcoCustomerChurn?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link |

---

## 📌 Ringkasan Proyek

Perusahaan telekomunikasi menghadapi tingkat *churn* pelanggan sebesar **26.5%**, sementara biaya akuisisi pelanggan baru jauh lebih mahal dibanding biaya retensi. Tim retensi saat ini menjalankan promo secara acak/reaktif, bukan proaktif berbasis data.

Proyek ini membangun model klasifikasi untuk:
1. Mengidentifikasi pelanggan dengan risiko *churn* tinggi sebelum mereka benar-benar berhenti.
2. Mengelompokkan pelanggan ke dalam **tingkat risiko (Low / Medium / High)** agar tim retensi bisa mengalokasikan budget promo secara bertingkat, bukan *blanket promotion*.
3. Mengukur estimasi penghematan biaya (cost-benefit) dari penggunaan model dibanding pendekatan acak.

## Isi Notebook

1. **Business Understanding** — latar belakang, problem statement, tujuan bisnis
2. **Data Understanding** — struktur dataset (7.043 baris, 21 kolom)
3. **Data Cleaning & Validation** — missing value tersembunyi pada `TotalCharges`, duplikat, outlier, kardinalitas kategori
4. **Exploratory Data Analysis (EDA)** — analisis univariat, bivariat (Mann-Whitney U, Chi-Square, Two-Proportion Z-Test), dan multivariat (korelasi Spearman)
5. **Preprocessing (Anti Data Leakage)** — split sebelum *fitting*, `RobustScaler`, `OrdinalEncoder`, `OneHotEncoder` dalam `ColumnTransformer`
6. **Modeling** — benchmarking 6 algoritma, tuning **Logistic Regression** (model terpilih untuk interpretability), optimalisasi threshold berbasis **F2-Score**, *risk tier binning*, feature importance
7. **Business Recommendation & Cost-Benefit Analysis** — perbandingan biaya *False Negative* vs *False Positive*, skenario dengan model vs pendekatan acak, rekomendasi bisnis untuk tim retensi

### Ringkasan Model

| Item | Nilai |
|---|---|
| Algoritma | Logistic Regression (tuned) |
| Metrik optimasi | F2-Score (mengutamakan recall untuk menekan *False Negative*) |
| Threshold operasional | F2-optimal (dihitung dari `precision_recall_curve` pada test set) |
| Risk tier | Low (< threshold), Medium (threshold – 0.60), High (≥ 0.60) |
| Fitur utama | `Contract`, `tenure`, `InternetService`, `MonthlyCharges`, `PaymentMethod` |

##  Aplikasi Streamlit

Aplikasi `app.py` menyediakan dua mode:
- **Prediksi Satu Pelanggan** — input data via form, menampilkan probabilitas churn, tingkat risiko, dan rekomendasi aksi retensi.
- **Prediksi Massal (CSV)** — upload file CSV berisi banyak pelanggan, menghasilkan ringkasan portofolio per tingkat risiko beserta file hasil prediksi yang bisa diunduh.

Coba langsung di: **https://telco-churn-app-23mxzztus2mdqyvwxu9bgw.streamlit.app/**

## Struktur Proyek

```
.
├── LICENSE                                # Lisensi proyek
├── Telco_Cust_Churn_FIXED.ipynb           # Notebook analisis end-to-end
├── WA_Fn-UseC_-Telco-Customer-Churn.csv   # Dataset mentah
├── contoh_data_pelanggan.csv              # Contoh/template data untuk prediksi massal (batch)
├── train_export.py                        # Script training & export model (churn_model.joblib)
├── app.py                                 # Aplikasi Streamlit
├── requirements.txt                       # Dependencies
├── runtime.txt                            # Versi Python untuk deployment (Streamlit Cloud)
├── churn_model.joblib                     # Model hasil training (siap pakai oleh app.py)
└── README.md
```

## Menjalankan Secara Lokal

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Train & export model
python train_export.py

# 3. Jalankan aplikasi
streamlit run app.py
```

## Tech Stack

- **Data & EDA:** pandas, numpy, matplotlib, seaborn, missingno, scipy, statsmodels
- **Modeling:** scikit-learn (Logistic Regression, GridSearchCV, ColumnTransformer), XGBoost
- **Deployment:** Streamlit, joblib

## Lisensi & Data

Dataset: [Telco Customer Churn (IBM Sample Dataset)](https://www.kaggle.com/datasets/blastchar/telco-customer-churn), digunakan untuk keperluan edukasi/final project.
