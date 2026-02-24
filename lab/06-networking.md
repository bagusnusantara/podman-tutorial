# Modul 6: Networking

Podman menggunakan CNI (Container Network Interface) atau Netavark untuk mengelola jaringan.

## 1. Bridge Network (Default)
Secara default, container yang dijalankan tanpa menentukan network akan masuk ke bridge network `podman`.

Cek daftar network:
```bash
podman network ls
```

---

## 2. Port Mapping
Kita sudah sering menggunakan `-p [host_port]:[container_port]`.
```bash
podman run -d -p 8080:80 nginx
```
Ini memetakan port 8080 pada semua interface host ke port 80 container.

---

## 3. Custom Network
Untuk memungkinkan antar container saling berkomunikasi menggunakan **nama container** (DNS resolution), kita harus membuat network kustom.

1.  **Buat Network:**
    ```bash
    podman network create internal-net
    ```
2.  **Jalankan Container di Network Tersebut:**
    ```bash
    podman run -d --name web --network internal-net nginx
    podman run -it --name client --network internal-net alpine sh
    ```
3.  **Uji DNS:**
    Di dalam container `client`, coba lakukan ping ke `web`:
    ```bash
    ping web
    ```
    *Podman akan otomatis memberikan resolusi IP berdasarkan nama container.*

---

## 4. Host Network Mode
Jika Anda ingin container menggunakan stack network yang sama dengan host (tanpa isolasi):
```bash
podman run --network host ...
```
*Catatan: Ini kurang aman dan sering digunakan untuk container yang membutuhkan performa network tinggi atau akses khusus ke hardware.*

---

## 5. Inspeksi Network
Melihat siapa saja yang terhubung ke sebuah network:
```bash
podman network inspect internal-net
```

---
**Tugas Lab:**
1. Buat network baru bernama `backend-net`.
2. Jalankan container `mariadb` di network `backend-net` dengan nama `db-server`.
3. Jalankan container `alpine` di network yang sama, lalu gunakan command `nc -zv db-server 3306` untuk memastikan konektivitas ke database.
