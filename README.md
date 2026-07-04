# Kisisel Harcama Analizi
Bu, harcamalarını takip etmeni, kategorilere ayırmanı ve ne kadar para harcadığını görmeni sağlayan basit bir Python programıdır. Verilerini bilgisayarında bir dosyada (SQLite) saklar, böylece programı kapatsan bile kayıtların silinmez.

import sqlite3
from datetime import datetime

# Veritabanı bağlantısı
def baglan():
    conn = sqlite3.connect('harcamalar.db')
    cursor = conn.cursor()
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS harcamalar (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            kategori TEXT,
            miktar REAL,
            tarih TEXT
        )
    ''')
    conn.commit()
    return conn

# Veri ekleme fonksiyonu
def harcama_ekle(kategori, miktar):
    conn = baglan()
    cursor = conn.cursor()
    tarih = datetime.now().strftime("%Y-%m-%d")
    cursor.execute('INSERT INTO harcamalar (kategori, miktar, tarih) VALUES (?, ?, ?)', 
                   (kategori, miktar, tarih))
    conn.commit()
    conn.close()
    print("Harcama başarıyla eklendi!")

# Analiz fonksiyonu
def rapor_al():
    conn = baglan()
    cursor = conn.cursor()
    cursor.execute('SELECT kategori, SUM(miktar) FROM harcamalar GROUP BY kategori')
    veriler = cursor.fetchall()
    conn.close()
    
    print("\n--- Kategori Bazlı Harcamalar ---")
    for satir in veriler:
        print(f"{satir[0]}: {satir[1]} TL")

# Basit Menü
if __name__ == "__main__":
    while True:
        print("\n1. Harcama Ekle\n2. Raporu Görüntüle\n3. Çıkış")
        secim = input("Seçiminiz: ")
        
        if secim == '1':
            kat = input("Kategori (örn: Gıda, Kira): ")
            mik = float(input("Miktar: "))
            harcama_ekle(kat, mik)
        elif secim == '2':
            rapor_al()
        elif secim == '3':
            break
git init
git add .
git commit -m "Kişisel Harcama Analizörü projesi eklendi"
git branch -M main
# GitHub'da oluşturduğun boş deponun linkini buraya yapıştır
git remote add origin https://github.com/KULLANICI_ADIN/REPO_ADIN.git
git push -u origin main
