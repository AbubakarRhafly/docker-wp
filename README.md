## Menjalankan WordPress, MySQL, dan Redis dengan Docker Compose

### 1. Persiapan

Sebelum menjalankan project ini, pastikan hal berikut sudah siap:

* Docker Desktop sudah terpasang
* Docker Compose sudah bisa dipakai
* Port `8000` tidak sedang dipakai aplikasi lain

---

### 2. Ambil Source Code

Clone repository ini ke komputer lokal:

```bash
git clone https://github.com/AbubakarRhafly/docker-wp.git
cd docker-wp
```

---

### 3. Menyalakan Semua Service

Jalankan Docker Compose dengan perintah berikut:

```
docker compose up -d
```
Perintah ini akan menjalankan 3 container:
* WordPress
* MySQL
* Redis

---

### 4. Memastikan Container Berjalan

Cek container yang aktif dengan command berikut:
```
docker compose ps
```
Pastikan service wordpress, mysql, dan redis sudah dalam status berjalan.

---

### 5. Membuka WordPress

Setelah semua container aktif, buka browser lalu akses:
```
http://localhost:8000
```
Setelah itu lanjutkan proses instalasi WordPress dari halaman web yang muncul.

---

### 6. Menghentikan Project

Kalau ingin menghentikan semua container, gunakan:
```
docker compose down
```

---

### 7. Keterangan Tambahan

* Database MySQL disimpan di volume mysql_data
* File WordPress disimpan di volume wordpress_data
* Redis berjalan sebagai service cache internal
* WordPress diakses dari browser melalui port 8000


### Screenshot Dashboard WordPress
<img width="1915" height="1075" alt="Screenshot 2026-04-05 110921" src="https://github.com/user-attachments/assets/333496c7-a476-4528-ad71-f266ee587dcb" />
<img width="1906" height="914" alt="Screenshot 2026-04-05 110950" src="https://github.com/user-attachments/assets/a89ba873-fcf6-48e7-b73a-92816efdf281" />
<img width="1909" height="1024" alt="Screenshot 2026-04-05 134659" src="https://github.com/user-attachments/assets/b93d955e-1b77-468b-81b3-37b722004e16" />

### Screenshot Hasil docker compose ps
<img width="1917" height="1022" alt="image" src="https://github.com/user-attachments/assets/a7dc2e4a-ff6c-4c2a-8d08-d60fc69f8c54" />

### Screenshot Redis CLI ping test
<img width="1916" height="999" alt="image" src="https://github.com/user-attachments/assets/316fa35b-34cb-40fd-af02-15c6c9d490ed" />

1. Kenapa perlu volume untuk MySQL?
Volume diperlukan agar data database tetap tersimpan walaupun container dihentikan, dihapus, atau dijalankan ulang. Jika tidak menggunakan volume, data MySQL akan hilang karena penyimpanan di dalam container bersifat sementara.

2. Apa fungsi depends_on?
depends_on digunakan untuk mengatur urutan saat service dijalankan. Dalam project ini, WordPress dibuat bergantung pada MySQL dan Redis agar service pendukung dijalankan lebih dulu. Namun, depends_on tidak menjamin bahwa service tersebut sudah benar-benar siap digunakan.

3. Bagaimana WordPress container connect ke MySQL?
WordPress terhubung ke MySQL melalui environment variable WORDPRESS_DB_HOST yang diarahkan ke nama service MySQL, yaitu mysql:3306. Karena semua container berada di network yang sama, Docker memungkinkan komunikasi internal antar service menggunakan nama tersebut.

4. Apa keuntungan pakai Redis untuk WordPress?
Redis membantu meningkatkan performa WordPress dengan menyimpan cache di memori. Dengan begitu, WordPress tidak selalu harus mengambil data langsung dari MySQL, sehingga akses website menjadi lebih cepat dan beban database menjadi lebih ringan.
