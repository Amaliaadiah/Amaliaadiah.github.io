---
title: "Web Scraping Data Kost Malang dari Mamikos Menggunakan Selenium"
date: 2025-06-26 08:00:00 +0800
categories: [Data Mining, Web Scraping]
tags: [Python, Selenium, Mamikos, Kost, Scraping, BeautifulSoup]
image: /assets/img/img5.png
---

## 🏘️ Mengambil Data Kost Malang Secara Otomatis

Salah satu tantangan menarik dalam scraping data real adalah saat berhadapan dengan situs yang dinamis dan memuat konten secara bertahap.  
Kasus yang dibahas di sini: **mengambil data kost bulanan di Kota Malang dari situs Mamikos**, yang dikenal dengan teknik *lazy-loading* dan penggunaan JavaScript untuk menampilkan data.

---

## 🔍 Mengapa Harus Pakai Selenium?

Situs Mamikos tidak menyajikan semua data kost langsung di awal. Alih-alih, data baru akan muncul setelah:

- Mengklik tombol "Lihat lebih banyak lagi"
- Melakukan scroll berkali-kali

Karena itulah kita tidak bisa hanya mengandalkan `requests` atau `BeautifulSoup`.  
Solusinya? Gunakan **Selenium** untuk mengontrol browser secara otomatis.

---

## 🎯 Tujuan Proyek

- Mengambil daftar kost bulanan di Kota Malang dari Mamikos
- Meng-handle pemuatan konten dinamis (scroll & klik)
- Menyimpan hasil akhir ke dalam file `.csv` untuk dianalisis lebih lanjut

---

## 🧰 Tools & Library

| Library         | Fungsi Utama                                    |
|------------------|--------------------------------------------------|
| `selenium`       | Mengontrol browser (klik, scroll, dll.)         |
| `beautifulsoup4` | Parsing HTML setelah konten dimuat              |
| `pandas`         | Mengolah dan menyimpan data ke format CSV       |
| `time`           | Memberi delay agar konten dimuat sepenuhnya     |

---

## 🧑‍💻 Implementasi Program

### 1. Import Library
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from bs4 import BeautifulSoup
import pandas as pd
import time
2. Setup Browser Driver
python
Copy
Edit
def setup_driver():
    options = webdriver.ChromeOptions()
    options.add_argument("--disable-blink-features=AutomationControlled")
    options.add_argument("user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64)")
    return webdriver.Chrome(options=options)
3. Klik Tombol "Lihat Lebih Banyak Lagi"
python
Copy
Edit
def expand_all_listings(driver, max_clicks=5):
    for _ in range(max_clicks):
        try:
            load_btn = WebDriverWait(driver, 5).until(
                EC.element_to_be_clickable((By.CLASS_NAME, "nominatim-list__see-more"))
            )
            ActionChains(driver).move_to_element(load_btn).click().perform()
            time.sleep(2)
        except:
            break
4. Scroll Halaman Otomatis
python
Copy
Edit
def scroll_page(driver, times=5, pause=2):
    for _ in range(times):
        driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
        time.sleep(pause)
5. Fungsi Ambil Data Kost
python
Copy
Edit
def extract_data_from_page(html):
    soup = BeautifulSoup(html, "html.parser")
    cards = soup.select('div[data-testid="kostRoomCard"]')
    data = []

    def get_text(tag, default="Tidak tersedia"):
        return tag.text.strip() if tag else default

    for card in cards:
        img_tag = card.select_one('img[data-testid="roomCardCover-photo"]')
        image = img_tag.get("src") if img_tag else "Tidak ada gambar"

        data.append([
            get_text(card.select_one(".rc-info__name")),
            get_text(card.select_one(".rc-overview__label")),
            get_text(card.select_one(".rc-info__location")),
            ", ".join([x.text for x in card.select('span[data-testid="roomCardFacilities-facility"]')]),
            get_text(card.select_one(".rc-overview__rating-text")),
            get_text(card.select_one(".rc-price__additional-discount-price")),
            get_text(card.select_one(".rc-price__text")),
            image
        ])
    return data
6. Eksekusi Utama
python
Copy
Edit
if __name__ == "__main__":
    url = "https://mamikos.com/cari/malang-kota-malang-jawa-timur-indonesia/all/bulanan/0-15000000"
    driver = setup_driver()
    driver.get(url)
    time.sleep(3)

    expand_all_listings(driver)
    scroll_page(driver, times=6)

    WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.CSS_SELECTOR, 'div[data-testid="kostRoomCard"]'))
    )

    kost_list = extract_data_from_page(driver.page_source)
    driver.quit()

    df = pd.DataFrame(kost_list, columns=[
        "Nama Kost", "Tipe Kost", "Lokasi", "Fasilitas",
        "Rating", "Harga Awal", "Harga Diskon", "Link Gambar"
    ])
    df.to_csv("kost_malang_mamikos.csv", index=False, encoding="utf-8-sig")
    print(f"✅ {len(df)} data kost berhasil disimpan.")
📄 Output
File	Deskripsi
kost_malang_mamikos.csv	Data kost Kota Malang hasil scraping

⚠️ Kendala & Penanganan
Masalah	Solusi
Konten tidak langsung muncul	Gunakan scroll_page() dan WebDriverWait()
Situs mendeteksi scraping	Gunakan user-agent & non-automasi headless
Elemen HTML tidak ditemukan	Beri delay atau try-except fallback

🔮 Pengembangan Selanjutnya
Menambahkan filter harga otomatis

Simpan hasil ke database (SQL)

Deteksi jumlah halaman & scraping seluruh kota

📚 Referensi
Selenium Official Docs

BeautifulSoup Docs

Mamikos.com