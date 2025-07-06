---
title: "Dimensionality Reduction - Jenis dan Cara Implementasi"
date: 2025-06-26 08:00:00 +0800
categories: [Data Mining, Asynchronous Learning]
tags: [Dimensionality Reduction, PCA, UMAP, Data Science, Visualisasi Data]
image: /assets/img/img4.webp
---

## 💡 Apa Hubungannya Reduksi Dimensi dan Asynchronous Learning?

Sebelum masuk ke reduksi dimensi, yuk pahami dulu apa itu **asynchronous learning** dalam konteks pembelajaran mesin:

- **Online Learning**: model belajar dari data satu per satu seiring waktu  
- **Federated Learning**: data tersebar di banyak perangkat (misalnya HP), dan model dilatih tanpa harus mengirim data ke pusat

Nah, di sistem seperti itu, penting banget untuk membuat proses pelatihan **efisien**. Di sinilah *dimensionality reduction* berperan:

- ✅ Mempercepat training (lebih sedikit fitur yang diproses)  
- ✅ Mengurangi kompleksitas data  
- ✅ Menghemat bandwidth dalam federated learning  
- ✅ Mengurangi noise dan mempercepat konvergensi model

Jadi, sebelum data diproses atau dikirim, kita bisa melakukan **reduksi dimensi** terlebih dahulu dengan metode seperti **PCA** atau **UMAP**.

---

## 📉 Apa Itu Dimensionality Reduction?

**Dimensionality Reduction** adalah teknik untuk menyederhanakan data berdimensi tinggi tanpa kehilangan terlalu banyak informasi penting.  
Berguna saat data terlalu banyak fitur (kolom), yang membuat analisis dan visualisasi jadi sulit.

### ✨ Manfaat:
- Mengurangi noise dalam data
- Mempercepat proses training
- Mempermudah visualisasi (contoh: 2D/3D)

### 🔧 Metode Umum:
- **PCA (Principal Component Analysis)**
- **UMAP (Uniform Manifold Approximation and Projection)**
- **t-SNE** (lebih berat dan cocok untuk visualisasi)

---

## 🧠 Cara Kerja Singkat

Bayangkan kamu punya data 3D (X, Y, Z), dan ternyata informasi paling penting hanya dari X dan Y.  
Maka Z bisa diabaikan → hasilnya lebih ringkas, lebih mudah dianalisis.

- **PCA**: mencari kombinasi linier (sumbu baru) dengan variansi maksimum  
- **UMAP**: lebih fokus pada struktur lokal dan jarak antar data

---

## 🧪 Studi Kasus: Dataset Penguin 🐧

Kita akan mengaplikasikan **PCA** dan **UMAP** pada dataset `penguins` dari Seaborn, yang memuat data morfologi dari 3 spesies: *Adelie*, *Chinstrap*, *Gentoo*.

### 📚 Tools yang Digunakan:

| Library        | Fungsi                             |
|----------------|------------------------------------|
| seaborn        | Mengambil dataset penguins         |
| pandas         | Manipulasi data                    |
| scikit-learn   | PCA & StandardScaler               |
| umap-learn     | UMAP                               |
| matplotlib     | Visualisasi                        |

---

## 🧰 Langkah Implementasi

### 1. Load Data & Normalisasi
```python
import seaborn as sns
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
import umap

# Load dan filter data
df = sns.load_dataset('penguins').dropna(subset=[
    'bill_length_mm','bill_depth_mm','flipper_length_mm','body_mass_g','species'])

X = df[['bill_length_mm','bill_depth_mm','flipper_length_mm','body_mass_g']]
y = df['species']

# Normalisasi
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
````

### 2. PCA – Proyeksi Linear

```python
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)
print("Explained variance (PCA):", pca.explained_variance_ratio_)

plt.figure(figsize=(6,5))
sns.scatterplot(x=X_pca[:,0], y=X_pca[:,1], hue=y)
plt.title("PCA (2D) – Penguin")
plt.show()
```

---

### 3. UMAP – Proyeksi Non-Linear

```python
umap_model = umap.UMAP(n_components=2, random_state=42)
X_umap = umap_model.fit_transform(X_scaled)

plt.figure(figsize=(6,5))
sns.scatterplot(x=X_umap[:,0], y=X_umap[:,1], hue=y)
plt.title("UMAP (2D) – Penguin")
plt.show()
```
---

## ⚖️ Perbandingan PCA vs UMAP

| Aspek               | PCA (Linear)      | UMAP (Non-Linear)         |
| ------------------- | ----------------- | ------------------------- |
| Cara Kerja          | Variansi maksimal | Hubungan lokal / tetangga |
| Interpretasi        | Lebih mudah       | Lebih kompleks            |
| Visualisasi Cluster | Lumayan           | Jelas dan rapi            |
| Kecepatan           | Cepat             | Sedikit lebih lambat      |

---

## 🧾 Kesimpulan

Reduksi dimensi sangat bermanfaat, apalagi di sistem **asynchronous learning**:

* 💨 Mempercepat training
* 📦 Mengurangi data yang dikirim
* 📊 Mempermudah eksplorasi data

**PCA** cocok untuk reduksi fitur cepat dan matematis.
**UMAP** unggul untuk visualisasi dan pemetaan struktur data yang kompleks.

---

## 📚 Bacaan Lanjutan

* [GeeksforGeeks - Dimensionality Reduction](https://www.geeksforgeeks.org/dimensionality-reduction/)
* [UMAP Documentation](https://umap-learn.readthedocs.io/)
* [Scikit-learn PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)

---