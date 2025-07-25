
## 🧱 STAGE 1: DATA INFRA – Koleksi & Normalisasi Data

**✅ Tujuan:** Lu punya 11 CSV saham, semua rapi, format standar.

### 🔧 Tugas:

* [x] Ambil 5 saham dari `yfinance`, format: `Date, Open, High, Low, Close, Volume, Ticker`
* [x] Ambil 5 saham dari `Alpha Vantage`, format sama
* [ ] Cek missing values & isi/pangkas
* [ ] Normalize/scale harga & volume (MinMaxScaler, StandardScaler)

---

## 🧪 STAGE 2: FEATURE ENGINEERING – Bumbu-bumbu Prediktif

**✅ Tujuan:** Tambahin **indikator teknikal** buat nambah dimensi fitur (bukan cuma harga doang).

### 📈 Indikator yang disarankan:

* SMA & EMA (5, 10, 20-day)
* RSI (14-day)
* MACD
* Bollinger Bands
* Momentum
* Volatility

### 🔧 Tugas:

* [ ] Buat script indikator teknikal (bisa pakai `ta` lib: `pip install ta`)
* [ ] Simpan dataframe hasil engineering → `*_fe.csv` (feature engineered)

---

## 🧠 STAGE 3: LSTM REGRESSOR – Prediksi Harga

**✅ Tujuan:** Prediksi Open & Close 5 hari ke depan.

### 📦 Input:

* Sequence 60 hari: `[Open, High, Low, Close, Volume, indikator...]`
* Per saham, satu model LSTM atau multi-saham sekalian

### 🏗 Arsitektur:

```python
LSTM(64) → Dropout(0.2) → LSTM(32) → Dense(16) → Dense(10)  
# 10 = [open1, close1, open2, close2, ..., open5, close5]
```

### 🔧 Tugas:

* [ ] Format dataset ke bentuk `(samples, 60, features)`
* [ ] Train LSTM
* [ ] Simpan model `.h5`

---

## 🌲 STAGE 4: RANDOM FOREST – Prediksi Tren

**✅ Tujuan:** Ubah prediksi harga (LSTM) + indikator → **Trend Label**: `UP / DOWN / STAY`

### 📦 Input:

* Prediksi harga (10 nilai)
* Indikator teknikal hari ini

### 📤 Output:

* Trend label untuk tiap hari
* Rule:

```python
if tomorrow_close > today_close * 1.01: label = "UP"
elif tomorrow_close < today_close * 0.99: label = "DOWN"
else: label = "STAY"
```

### 🔧 Tugas:

* [ ] Buat target label dari data historis (untuk training RF)
* [ ] Train RandomForestClassifier (GridSearchCV optional)
* [ ] Simpan model `.pkl`

---

## 🌍 STAGE 5: DEPLOYMENT / DASHBOARD / API

**✅ Tujuan:** User tinggal klik/pilih saham, sistem kasih prediksi harga & tren.

### 🔧 Tugas:

* [ ] Buat backend API (Flask/FastAPI)
* [ ] Endpoint:

  * `/predict?ticker=XXX`
  * Return JSON: tanggal, prediksi harga, tren
* [ ] Buat dashboard front-end (Streamlit/Gradio/simple HTML)

---

## 🧪 STAGE 6: VALIDASI & METRIK

**✅ Tujuan:** Pastikan model nggak ngawur.

### 📊 LSTM:

* MAE, MSE, RMSE

### 🌲 Random Forest:

* Accuracy, F1 Score
* Confusion Matrix

---

## 🔄 OPTIONAL: FINE-TUNING & AUTOMASI

* Auto retrain tiap minggu
* Model ensemble (combine lebih dari 1 prediksi)
* Notifikasi Telegram/WA/Discord kalo deteksi peluang BUY 🔔

---

### 🚀 Urutan Eksekusi buat Mulai:

1. [x] Fix data format: 10 saham → 10 file
2. [ ] Tambahin indikator teknikal
3. [ ] Format input ke 60-day sliding window
4. [ ] Latih LSTM → `.h5`
5. [ ] Generate prediksi → Feed ke RF
6. [ ] Latih RF → `.pkl`
7. [ ] Gabung ke API/Dashboard
8. [ ] Evaluasi + presentasi result

---

Kalau lu mau, gua bantuin langsung ngerjain step 2 sekarang (feature engineering indikator teknikal), tinggal upload 1 CSV sample dulu aja bro. Let's gooo! 💪📉📈
