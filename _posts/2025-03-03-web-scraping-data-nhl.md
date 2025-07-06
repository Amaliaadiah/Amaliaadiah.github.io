---
title: "Mengambil Data Tim Hoki NHL dengan Selenium dan Python"
date: 2025-03-03 08:00:00 +0800
categories: [Data Mining, Web Scraping]
tags: [Python, Selenium, Pandas]
image: /assets/img/img1.jpg
---

## 🧾 Pendahuluan

Web scraping merupakan metode otomatisasi yang digunakan untuk mengekstrak data dari halaman situs web. Dalam panduan ini, kita akan mempraktikkan teknik tersebut untuk mengambil informasi dari halaman [Hockey Teams: Forms](https://www.scrapethissite.com/pages/forms/) yang menyajikan data tim hoki NHL berdasarkan tahun.

## 🎯 Target Pembelajaran

* Mengambil informasi tim NHL dari tahun-tahun tertentu
* Menyimpan hasil dalam format CSV
* Mengelola dropdown dan navigasi antar halaman secara otomatis

## ⚙️ Teknologi yang Digunakan

| Alat / Library | Deskripsi                                                            |
| -------------- | -------------------------------------------------------------------- |
| Selenium       | Mengendalikan browser secara otomatis untuk mengambil konten dinamis |
| Pandas         | Mengolah dan menyimpan data dalam format tabel (CSV)                 |
| time           | Memberi jeda untuk memastikan elemen halaman dimuat sempurna         |

## 🔧 Instalasi

Langkah awal, pastikan kamu memiliki pustaka berikut:

```bash
pip install selenium pandas
```

Kemudian unduh **ChromeDriver** yang sesuai dengan versi browser:
[🔗 Unduh ChromeDriver](https://sites.google.com/chromium.org/driver/)

## 💻 Implementasi

Berikut adalah skrip lengkap untuk scraping:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import Select, WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import pandas as pd
import time

# Inisialisasi driver dan buka situs target
driver = webdriver.Chrome()
driver.get("https://www.scrapethissite.com/pages/forms/")

# Ubah dropdown agar menampilkan 100 data per halaman
dropdown = WebDriverWait(driver, 10).until(
    EC.presence_of_element_located((By.ID, "per_page"))
)
Select(dropdown).select_by_visible_text("100")
time.sleep(2)

# Ambil seluruh data dari semua halaman
all_data = []

while True:
    time.sleep(1.5)
    rows = driver.find_elements(By.CSS_SELECTOR, "table.table tbody tr.team")
    for row in rows:
        cols = row.find_elements(By.TAG_NAME, "td")
        data = [col.text.strip() for col in cols]
        all_data.append(data)

    try:
        next_button = driver.find_element(By.XPATH, '//a[text()="Next"]')
        parent_li = next_button.find_element(By.XPATH, "./..")
        if "disabled" in parent_li.get_attribute("class"):
            print("Data dari halaman terakhir berhasil diambil.")
            break
        else:
            next_button.click()
    except:
        print("Tombol Next tidak ditemukan. Proses scraping dihentikan.")
        break

# Simpan ke file CSV
df = pd.DataFrame(all_data, columns=[
    "Team Name", "Year", "Wins", "Losses", "OT Losses",
    "Win %", "Goals For (GF)", "Goals Against (GA)", "Goal Difference"
])
print(f"Data berhasil dikumpulkan: {df.shape[0]} baris")
df.to_csv("nhl_all_teams.csv", index=False)

driver.quit()
```

## 📁 Output

* **File CSV:** `nhl_all_teams.csv`
* **Jumlah Baris:** Antara 30 hingga 100+ tergantung filter tahun

Contoh data:

```
Team Name       Year    Wins  Losses  OT Losses  Win %  GF   GA   Diff
Boston Bruins   1990    44    24      0          0.55   299  264  35
```

## 🧩 Masalah yang Sering Muncul & Solusinya

| Permasalahan                         | Penanganan                                       |
| ------------------------------------ | ------------------------------------------------ |
| Halaman tidak pindah ke “Next”       | Periksa apakah tombol Next sudah “disabled”      |
| Data tidak muncul secara lengkap     | Gunakan `time.sleep()` agar konten sempat dimuat |
| Elemen tidak ditemukan oleh Selenium | Gunakan `WebDriverWait` untuk menunggu elemen    |

## 🔭 Pengembangan Selanjutnya

* Filter data berdasarkan tahun tertentu
* Simpan data ke dalam database
* Jalankan scraping dengan **headless browser** agar lebih efisien

## 📚 Sumber Bacaan

* [Selenium Docs](https://selenium.dev/documentation/)
* [Scrape This Site - Forms](https://www.scrapethissite.com/pages/forms/)
* [Pandas Documentation](https://pandas.pydata.org/docs/)