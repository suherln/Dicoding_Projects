# Laporan Proyek: Klasifikasi 15 Jenis Sayuran dengan CNN

## Oleh: Muhammad Rafli Suherlan
**Email:** raflisuherlan593@gmail.com  
**ID Dicoding:** CACC005D6Y2206

---

## 1. Ringkasan Teknis Proyek

Saya membangun model CNN untuk mengklasifikasikan 15 jenis sayuran. Model tersebut saya latih menggunakan 21.000 gambar.

---

## 2. Dataset

| Properti | Value |
|----------|-------|
| Total gambar | 21.000 |
| Jumlah kelas | 15 |
| Gambar per kelas | 1.400 |
| Format | JPG (RGB) |

**15 kelas sayuran:**
Bean, Bitter Gourd, Bottle Gourd, Brinjal, Broccoli, Cabbage, Capsicum, Carrot, Cauliflower, Cucumber, Papaya, Potato, Pumpkin, Radish, Tomato.

**Pembagian data:**
- Training: 70% (14.700 gambar)
- Validation: 15% (3.150 gambar)
- Testing: 15% (3.150 gambar)

Sumber: [Vegetable Image Dataset - Kaggle](https://www.kaggle.com/datasets/misrakahmed/vegetable-image-dataset/data)

---

## 3. Preprocessing

Sebelum dimasukkan ke model, saya melakukan:

1. **Resize** -> dari 224×224 menjadi **128×128 piksel** (mempercepat komputasi)
2. **Rescale** -> nilai pixel dari [0,255] dinormalisasi ke [0,1]
3. **Tidak menggunakan augmentasi** karena dataset sudah cukup besar dan seimbang

---

## 4. Arsitektur Model

Model tersebut saya bangun dengan arsitektur berikut:

| Layer (dari input ke output) | Detail Operasi | Ukuran Output |
|------------------------------|----------------|---------------|
| **Input Layer** | Gambar RGB ukuran 128×128 | 128 × 128 × 3 |
| **Conv2D + BatchNorm + ReLU** | 16 filter, kernel 3×3, padding='same' | 128 × 128 × 16 |
| **MaxPooling2D** | Pooling size 2×2 | 64 × 64 × 16 |
| **Conv2D + BatchNorm + ReLU** | 32 filter, kernel 3×3, padding='same' | 64 × 64 × 32 |
| **MaxPooling2D** | Pooling size 2×2 | 32 × 32 × 32 |
| **Conv2D + BatchNorm + ReLU** | 64 filter, kernel 3×3, padding='same' | 32 × 32 × 64 |
| **GlobalAveragePooling2D** | Rata-rata global tiap channel | 64 (feature vector) |
| **Dense + ReLU** | 64 neuron, aktivasi ReLU | 64 |
| **Dropout** | 50% neuron dimatikan saat training | 64 |
| **Dense + Softmax** | 15 neuron, aktivasi Softmax | 15 (probabilitas tiap kelas) |

**Total parameter:** sekitar 1,4 juta (trainable)

---

## 5. Hyperparameter Training

| Parameter | Nilai |
|-----------|-------|
| Optimizer | Adam |
| Learning rate | 5×10⁻⁴ (0.0005) |
| Loss function | categorical_crossentropy |
| Batch size | 32 |
| Maksimal epoch | 100 |
| Callback EarlyStopping | patience=5, monitor='val_loss' |
| Callback ReduceLROnPlateau | factor=0.5, patience=3 |

---

## 6. Evaluasi

Saya evaluasi model tersebut menggunakan data test (3.150 gambar).

### 6.1 Metrik yang dihitung
- Accuracy
- Precision (macro)
- Recall (macro)
- F1-score (macro)
- Confusion matrix (15×15)

### 6.2 Hasil yang diperoleh
- **Akurasi test**
- **Presisi makro rata-rata**
- **Recall makro rata-rata**
- **F1-score makro rata-rata**

### 6.3 Confusion Matrix
Saya plot matriks 15×15. Hasilnya:
- Diagonal utama (prediksi benar) mendominasi
- Kesalahan terbanyak terjadi pada kelas dengan warna dan bentuk mirip (misal: Capsicum dengan Cucumber)