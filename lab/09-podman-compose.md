# Modul 9: Podman Compose

Untuk menjalankan aplikasi multi-service (seperti App + Database), menggunakan Docker Compose file sangat memudahkan. Podman mendukung ini melalui `podman-compose` atau integrasi native dengan `podman compose` (v4.x+).

## 1. Instalasi
Jika belum tersedia, Anda bisa menginstalnya via package manager:

**Ubuntu/Debian:**
```bash
sudo apt install podman-compose
```

**Python Pip:**
```bash
pip install podman-compose
```

---

## 2. Konversi dari Docker Compose
Podman secara native memahami file `docker-compose.yml`. Anda tidak perlu mengubah syntax-nya.

Contoh `docker-compose.yml` (App + Redis):
```yaml
version: '3.8'
services:
  web:
    image: nginx:alpine
    ports:
      - "8082:80"
  cache:
    image: redis:alpine
```

---

## 3. Menjalankan Stack Aplikasi
Gunakan perintah yang mirip dengan docker-compose:

```bash
# Menjalankan di background
podman-compose up -d

# Menampilkan container yang berjalan
podman ps
```

---

## 4. Manajemen Stack
*   **Melihat log**: `podman-compose logs -f`
*   **Menghentikan**: `podman-compose down`
*   **Membangun ulang image**: `podman-compose build`

---

## 5. Fitur Unik: Podman Compose ke Pods
Secara default, `podman-compose` akan membuat container terpisah yang terhubung lewat network. Namun, Anda juga bisa menginstruksikan Podman untuk menggabungkan semuanya ke dalam satu **Pod** demi optimasi network.

---
**Tugas Lab:**
1. Buat direktori `lab-compose`.
2. Buat file `docker-compose.yml` yang berisi service `wordpress` dan `mysql`.
3. Jalankan menggunakan `podman-compose`.
4. Verifikasi apakah WordPress bisa diakses di port yang ditentukan.
5. Hapus semua service dengan `podman-compose down`.
