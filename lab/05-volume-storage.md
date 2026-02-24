# Modul 5: Volume & Persistent Storage

Secara default, data di dalam container bersifat **ephemeral** (hilang saat container dihapus). Untuk menyimpannya secara permanen, kita menggunakan **Volumes**.

## 1. Bind Mount
Memetakan folder di host langsung ke folder di dalam container.
```bash
mkdir ~/html-data
podman run -d -p 8081:80 -v ~/html-data:/usr/share/nginx/html:Z nginx
```
*Flag `:Z` sangat penting di sistem dengan SELinux (seperti RHEL/Fedora) agar Podman bisa mengatur label keamanan folder tersebut.*

---

## 2. Named Volume
Volume yang dikelola sepenuhnya oleh Podman.
```bash
# Membuat volume
podman volume create my-db-data

# Melihat daftar volume
podman volume ls
```

Gunakan volume tersebut:
```bash
podman run -d --name db -v my-db-data:/var/lib/mysql mariadb
```

---

## 3. Izin (Permission) pada Rootless Mode
Karena Podman berjalan sebagai user biasa, mapping User ID (UID) antara host dan container sering menjadi kendala.

Jika Anda mengalami error `Permission Denied`, gunakan flag `--userns=keep-id` (khusus rootless):
```bash
podman run -it --userns=keep-id -v ./data:/data alpine sh
```
Ini memastikan UID user di host sama dengan UID user di dalam container.

---

## 4. Studi Kasus: Database Persisten
Mari kita coba menjalankan Database MariaDB dengan penyimpanan permanen.

1.  **Siapkan Volume:**
    ```bash
    podman volume create db_vol
    ```
2.  **Jalankan Container:**
    ```bash
    podman run -d \
      --name simple-db \
      -e MARIADB_ROOT_PASSWORD=rahasia \
      -v db_vol:/var/lib/mysql:Z \
      mariadb
    ```
3.  **Uji Coba:**
    Hapus container tersebut (`podman rm -f simple-db`), lalu jalankan kembali dengan perintah yang sama. Data database Anda akan tetap ada (persisten).

---
**Tugas Lab:**
1. Buat named volume bernama `app_logs`.
2. Jalankan container `alpine` yang menulis waktu ke file `/var/log/app.log` setiap 5 detik menggunakan loop, dan mounting volume `app_logs` ke `/var/log`.
3. Hapus containernya, lalu jalankan container baru dan cek apakah file `/var/log/app.log` masih ada.
