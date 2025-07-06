---
title: "Aplikasi Konversi Mata Uang Sederhana dengan Streamlit"
date: 2025-04-12 08:00:00 +0800
categories: [Data Mining, Streamlit]
tags: [Python, Streamlit, Aplikasi Sederhana]
image: /assets/img/img2.jpeg
---

## 💡 Pendahuluan

[Streamlit](https://streamlit.io) adalah pustaka open-source Python yang memungkinkan kita membangun aplikasi web interaktif hanya dengan beberapa baris kode Python. Cocok untuk data scientist, mahasiswa, atau siapa pun yang ingin menyajikan analisis atau tools berbasis Python secara visual tanpa harus belajar frontend (HTML, CSS, atau JavaScript).

## 🎯 Alasan Memilih Streamlit

- Sangat mudah dipahami (cukup dengan Python)
- Proses pembuatan aplikasi sangat cepat
- Tidak perlu mengatur backend, CSS, atau routing manual
- Cocok untuk membuat dashboard, visualisasi, atau kalkulator interaktif

## 🧪 Studi Kasus: Konversi Mata Uang IDR

Tutorial ini membahas cara membuat aplikasi konversi mata uang dari Rupiah (IDR) ke mata uang asing: USD, EUR, dan JPY.

### Tujuan Aplikasi

- Input jumlah Rupiah dari pengguna
- Pilih tujuan mata uang
- Hitung konversi berdasarkan nilai tukar tetap
- Tampilkan hasil konversi secara langsung

## 🔧 Tools dan Library

| Library / Tool | Fungsi |
|----------------|--------|
| Streamlit      | Antarmuka aplikasi web |
| Python         | Bahasa pemrograman utama |

## 💻 Kode Program

Simpan kode di bawah sebagai `currency_converter_app.py`:

```python
import streamlit as st

# Judul Aplikasi
st.title("Konversi Mata Uang: IDR ke USD / EUR / JPY")

# Input jumlah IDR
jumlah_idr = st.number_input("Masukkan jumlah Rupiah", min_value=0.0, step=1000.0)

# Pilih mata uang tujuan
mata_uang = st.selectbox("Pilih mata uang tujuan", [
    "USD (Dolar Amerika)", "EUR (Euro)", "JPY (Yen Jepang)"
])

# Kurs tetap (statis)
kurs = {
    "USD (Dolar Amerika)": 0.000065,
    "EUR (Euro)": 0.000060,
    "JPY (Yen Jepang)": 0.0098,
}

# Tombol konversi
if st.button("Konversi"):
    hasil = jumlah_idr * kurs[mata_uang]
    st.success(f"{jumlah_idr:,.0f} IDR = {hasil:,.2f} {mata_uang.split()[0]}")
```

## ▶️ Cara Menjalankan

1. Pastikan Python telah terinstal
2. Install Streamlit:
```bash
pip install streamlit
```
3. Simpan kode sebagai `currency_converter_app.py`
4. Jalankan aplikasi:
```bash
streamlit run currency_converter_app.py
```

Aplikasi akan terbuka otomatis di browser: `http://localhost:8501`

## 🖼️ Fitur Aplikasi

- Input nominal IDR
- Dropdown untuk pilihan mata uang tujuan
- Tombol konversi
- Hasil tampil langsung di halaman

**Contoh hasil:**  
`100.000 IDR = 6.50 USD`

## 📝 Catatan

Kurs yang digunakan bersifat statis untuk simulasi.

Ingin akurat? Gunakan API real-time seperti:

- [ExchangeRate API](https://www.exchangerate-api.com/)
- [Fixer.io](https://fixer.io)
- [Open Exchange Rates](https://openexchangerates.org/)

## 🔮 Pengembangan Selanjutnya

- Integrasi API kurs real-time
- Tambah grafik tren historis dengan Plotly
- Simpan hasil konversi ke file atau database
- Tambahkan lebih banyak mata uang (SGD, GBP, AUD, dll)
- Mode dua arah: konversi bolak-balik

## 📚 Referensi

- [Streamlit Docs](https://docs.streamlit.io)
- [ExchangeRate API](https://www.exchangerate-api.com/)
- [Python.org](https://www.python.org)
