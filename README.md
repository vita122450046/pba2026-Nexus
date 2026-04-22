# pba2026-Nexus
## 👥 Anggota Kelompok
- Vita Anggraini (122450046)
- Cintya Bella (122450066)
- Bastian (122450130)
Proyek analisis sentimen tweet berbasis **Machine Learning** dan **Deep Learning** dengan antarmuka **Streamlit**.  
Repository ini mencakup alur lengkap dari preprocessing data, training model, evaluasi, hingga deployment demo ke Hugging Face Spaces.

Penelitian ini bertujuan untuk:
1. Membangun model klasifikasi sentimen menggunakan machine learning dan deep learning
2. Membandingkan performa logistic regression dan bilstm
3. Mengimplementasikan model dalam bentuk aplikasi interaktif



## 🔗 Tautan Penting
- Dataset: [Sentiment140 (Kaggle)](https://www.kaggle.com/datasets/kazanova/sentiment140)
- Demo App (ML - Logistic Regression): [HF Space ML](https://huggingface.co/spaces/cintyabella28/pba2026-nexus-sentiment)
- Demo App (DL - BiLSTM): [HF Space DL](https://huggingface.co/spaces/cintyabella28/pba2026-nexus-sentiment-dl)

---

## 📁 Struktur Repository

```text
pba2026-Nexus/
├── app/
│   ├── app.py                 # Demo lokal model ML (TF-IDF + Logistic Regression)
│   └── app_dl.py              # Demo lokal model DL (BiLSTM PyTorch)
├── assets/
│   ├── sentiment_distribution.png
│   ├── text_length_distribution.png
│   └── dl/
│       ├── bilstm_sentiment_v1_history.csv
│       └── bilstm_sentiment_v1_loss_curve.png
├── data/
│   ├── cleaned_sample.csv
│   └── hasil_scrapping_komentar_youtube.xlsx
├── huggingface_app/           # Paket deploy ML ke Hugging Face
│   ├── app.py
│   ├── best_sentiment_model.pkl
│   ├── tfidf_vectorizer.pkl
│   ├── Dockerfile
│   └── requirements.txt
├── huggingface_app_dl/        # Paket deploy DL ke Hugging Face
│   ├── app.py
│   ├── bilstm_state_dict.pt
│   ├── vocab.json
│   ├── config.json
│   ├── Dockerfile
│   └── requirements.txt
├── models/
│   ├── best_sentiment_model.pkl
│   ├── tfidf_vectorizer.pkl
│   └── dl/
│       ├── bilstm_state_dict.pt
│       ├── bilstm_sentiment.pt
│       ├── vocab.json
│       ├── config.json
│       ├── train_config.json
│       ├── train_metrics.json
│       └── eval_metrics.json
├── notebooks/
│   └── EDA_dan_Preprocessing.ipynb
├── scripts/
│   ├── eda_preprocessing.py
│   ├── dl_bilstm_model.py
│   ├── check_bilstm_params.py
│   ├── train_bilstm.py
│   └── eval_bilstm.py
├── requirements.txt
└── README.md
```

---

## 🚀 Menjalankan Aplikasi Secara Lokal

### 1) Clone repository
```bash
git clone <url-repository-anda>
cd pba2026-Nexus
```

### 2) Buat dan aktifkan virtual environment (opsional, direkomendasikan)
```bash
python -m venv .venv
```

- Windows:
```bash
.venv\Scripts\activate
```

- Linux/macOS:
```bash
source .venv/bin/activate
```

### 3) Install dependency
```bash
pip install -r requirements.txt
```

### 4) Jalankan demo ML
```bash
streamlit run app/app.py
```

### 5) Jalankan demo DL
```bash
streamlit run app/app_dl.py
```

---

## 🤖 Model Machine Learning (Baseline)

- **Algoritma**: Logistic Regression  
- **Fitur teks**: TF-IDF  
- **Kelas sentimen**:
  - `0` = Negatif
  - `1` = Positif

---

## 📈 Hasil Evaluasi Model ML (Baseline)

Hasil baseline model **Logistic Regression + TF-IDF** (sesuai informasi demo ML):

- **Akurasi**: `73.5%`
- **Model**: Logistic Regression
- **Fitur**: TF-IDF
- **Dataset eksperimen**: Sentiment140 (sampling 10.000 data)

Catatan:
- Ringkasan performa baseline ini digunakan sebagai pembanding awal sebelum eksperimen Deep Learning (BiLSTM).
- Jika tersedia file evaluasi terstruktur untuk ML, metrik tambahan (precision/recall/F1/confusion matrix) bisa ditambahkan pada bagian ini.

---

## 🧠 Model Deep Learning (BiLSTM - PyTorch)

Arsitektur DL yang digunakan:
- **Embedding Layer**
- **BiLSTM (bidirectional=True)**
- **Dropout**
- **Linear Classifier**

Konfigurasi utama:
- `embedding_dim`: 128
- `hidden_dim`: 128
- `num_layers`: 2
- `dropout`: 0.3
- `max_len`: 50
- `vocab_size`: 4425

### ✅ Verifikasi Batas Parameter
- **Trainable parameters**: `1,226,370`
- **Batas maksimal tugas**: `10,000,000`
- **Status**: **PASS**

---

## 📊 Hasil Evaluasi Model DL (Test Set)

Berdasarkan `models/dl/eval_metrics.json`:

- **Accuracy**: `0.6917`
- **Precision**: `0.6890`
- **Recall**: `0.7000`
- **F1-score**: `0.6944`
- **Confusion Matrix**:
  - TN = 341
  - FP = 158
  - FN = 150
  - TP = 350

---

## 📉 Kurva Training & Validation Metrics

Grafik metrik training tersedia di:
- `assets/dl/bilstm_sentiment_v1_loss_curve.png` (kurva train/validation loss)
- `assets/dl/bilstm_sentiment_v1_accuracy_curve.png` (kurva train/validation accuracy)
- `assets/dl/bilstm_sentiment_v1_metrics_combined.png` (grafik gabungan loss + accuracy)

History per-epoch tersedia di:
- `assets/dl/bilstm_sentiment_v1_history.csv`

---

## 🏋️ Cara Training & Evaluasi Model DL

### Training BiLSTM
```bash
python scripts/train_bilstm.py --epochs 6 --batch-size 64 --run-name bilstm_sentiment_v1
```

### Analisis Training
Kurva training menunjukkan:
-Training loss terus menurun
-Validation loss sedikit lebih tinggi dari training loss
-Hal ini mengindikasikan adanya overfitting ringan

### Evaluasi BiLSTM
```bash
python scripts/eval_bilstm.py --output-json models/dl/eval_metrics.json
```

---

## 🚀 Deploy Hugging Face Spaces

### ML Demo Space
- Folder deploy: `huggingface_app/`
- Link: [HF Space ML](https://huggingface.co/spaces/cintyabella28/pba2026-nexus-sentiment)

### DL Demo Space
- Folder deploy: `huggingface_app_dl/`
- Link: [HF Space DL](https://huggingface.co/spaces/cintyabella28/pba2026-nexus-sentiment-dl)

---

## Preprocessing Data
-Case folding (lowercase)
-Cleaning (menghapus simbol dan karakter khusus)
-Tokenization
-Padding dan truncation hingga panjang maksimum 50
-Pembuatan vocabulary



## 📝 Catatan

- Artifact model DL standar untuk demo:
  - `models/dl/bilstm_state_dict.pt`
  - `models/dl/vocab.json`
  - `models/dl/config.json`
- Pastikan file artifact tersedia sebelum menjalankan `app/app_dl.py`.
- Untuk deployment, gunakan folder deploy terpisah (`huggingface_app/` dan `huggingface_app_dl/`) agar struktur rapi.

---

## 📜 Lisensi
Belum ditentukan.  
Disarankan menambahkan file `LICENSE` (misalnya MIT) jika repository dipublikasikan secara terbuka.
