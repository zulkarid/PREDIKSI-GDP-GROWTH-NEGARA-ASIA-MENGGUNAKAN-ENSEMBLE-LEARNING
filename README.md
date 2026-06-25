# PREDIKSI-GDP-GROWTH-NEGARA-ASIA-MENGGUNAKAN-ENSEMBLE-LEARNING
Dataset: IMF World Economic Outlook (WEO) 1980-2025

## Deskripsi Tugas Besar

Tugas besar ini berfokus pada prediksi pertumbuhan ekonomi (*GDP Growth*) negara-negara di kawasan Asia untuk tahun berikutnya ($t+1$) menggunakan pendekatan **Ensemble Learning**. 

Pertumbuhan ekonomi merupakan indikator makroekonomi krusial untuk mengevaluasi kesehatan finansial suatu wilayah. Mengingat variabel makroekonomi saling berkaitan secara nonlinear dan kompleks, proyek ini memanfaatkan kekuatan model berbasis *tree-ensemble* untuk menangkap pola tersebut secara lebih akurat dibandingkan metode statistik konvensional. 

Penelitian ini mengimplementasikan dan membandingkan performa beberapa arsitektur model pembelajar, antara lain:
- **Random Forest Regressor**
- **Gradient Boosting Regressor**
- **XGBoost Regressor**

Evaluasi difokuskan pada perbandingan performa model menggunakan fitur dasar (*baseline features*) dibandingkan dengan penambahan fitur historis hasil rekayasa (*feature engineering*) serta efek dari optimasi hyperparameter (*tuning*).

---

## Anggota Kelompok

Silakan isi daftar nama dan NIM anggota kelompok di bawah ini:
- [BALGIS KHAIRUNNISA SYAKIRA] - [103102400025]
- [KANAYA PUTRI PRABASA] - [103102400035]
- [Rayhan Akbar Zulkarnaen] - [103102400069]
- [ZAHIRA KAYLA WARDHANI] - [103102400022]
- [Shindy Yusnidha] - [103102400042]

---

## Dataset

Dataset yang digunakan bersumber dari **IMF World Economic Outlook (WEO)** periode tahun 1980 hingga perkiraan tahun 2025.

### Indikator Makroekonomi yang Digunakan

| No | Indikator Utama (IMF WEO) | Peran / Variabel | Satuan |
|----|---------------------------|------------------|--------|
| 1  | Gross domestic product (GDP), Constant prices, Percent change | **Target ($t+1$) & Fitur** | Persen (%) |
| 2  | All Items, Consumer price index (CPI), Period average, percent change | Fitur | Persen (%) |
| 3  | Gross domestic product (GDP), Current prices, Per capita, US dollar | Fitur | USD ($) |
| 4  | Population, Persons for countries / Index for country groups | Fitur | Jiwa (Orang) |
| 5  | Gross debt, General government, Percent of GDP | Fitur | Persen dari GDP |
| 6  | Current account balance (credit less debit), Percent of GDP | Fitur | Persen dari GDP |

### Karakteristik & Lingkup Geografis Dataset
- **Format Awal:** *Wide format* (kolom berbasis runtun waktu tahunan).
- **Format Akhir ML:** *Long format* panel data berbasis kombinasi unik `(COUNTRY, YEAR)`.
- **Cakupan Wilayah:** Negara-negara di kawasan Asia (Asia Timur, Asia Tenggara, Asia Selatan, Asia Tengah, dan Timur Tengah).

---

## Tahapan Analisis & Preprocessing

Proses pengerjaan dibagi menjadi tiga tahapan utama: pipeline data (preprocessing), rekayasa fitur, dan pemodelan.

### 1. Data Preprocessing & Transformasi (Pembersihan Data)
- **Filter Geografis & Fitur:** Menyaring data khusus regional Asia dan mengisolasi 6 indikator makroekonomi utama.
- **Strukturisasi Data (*Wide to Long*):** Mengubah bentuk tabel matriks tahunan menggunakan `pd.melt()` dan `pivot_table()` agar kompatibel dengan baris komputasi Scikit-Learn.
- **Penanganan Missing Values:** Melakukan **Interpolasi Linear Longitudinal** yang dikelompokkan per negara (`groupby("COUNTRY")`) untuk menjaga logika tren runtun waktu lokal.
- **Konstruksi Label Target:** Membuat kolom `TARGET` dengan metode pergeseran negatif (*negative shifting*) 1 periode ke depan ($t+1$).

### 2. Feature Engineering & Selection
- **Lag Features:** Membuat fitur `Lag 1` dan `Lag 2` pada indikator *GDP Growth* dan *Inflation Rate* untuk menangkap efek momentum ekonomi.
- **Rolling Features:** Menghitung rata-rata bergerak (`GDP_mean_3th`) dan standar deviasi bergerak (`GDP_std_3th`) dengan jendela waktu 3 tahun terakhir untuk mendeteksi stabilitas tren.
- **Analisis Korelasi:** Memanfaatkan matriks korelasi Pearson untuk mengevaluasi multikolinieritas dan signifikansi hubungan linear fitur baru terhadap `TARGET`.
- **Pembersihan Akhir & Pemotongan Data:** Menghapus baris kosong sisa dampak operasi *shifting/rolling* melalui perintah `.dropna()` sehingga menyisakan data bersih siap pakai.

### 3. Pemodelan & Evaluasi (Machine Learning)
- **Feature Scaling:** Menggunakan `StandardScaler` untuk menormalisasi sebaran nilai seluruh fitur numerik agar tidak timpang secara matriks jarak.
- **Data Splitting:** Pembagian data dengan proporsi **80% Data Training** dan **20% Data Testing**.
- **Model Training:** Melatih algoritma Random Forest, Gradient Boosting, dan XGBoost.
- **Hyperparameter Tuning:** Implementasi `RandomizedSearchCV` untuk mencari parameter optimal pada model demi mereduksi tingkat *error*.
- **Metrik Evaluasi:** Menggunakan Mean Squared Error (MSE), Root Mean Squared Error (RMSE), dan R-squared ($R^2$) Score.

---

## Library Python yang Digunakan

Aplikasi ini dibangun menggunakan beberapa pustaka Python utama sebagai berikut:
- Pandas
- NumPy
- Scikit-Learn
- XGBoost
- Matplotlib
- Seaborn

---

## Struktur Repositori

```text
├── ML_FINAL_ASS(1).ipynb
├── dataset_2026-06-10T06_24_43.190817281Z_DEFAULT_INTEGRATION_IMF RES_WEO_9.0.0.csv
└── README.md