# Modul 8: Pod (Konsep Unik Podman)

Berbeda dengan Docker yang unit terkecilnya adalah **Container**, Podman mendukung konsep **Pod** (mirip dengan Kubernetes).

## 1. Apa itu Pod?
Pod adalah grup dari satu atau lebih container yang berbagi network namespace, IP Address, dan storage volume yang sama. Container di dalam Pod yang sama bisa saling berkomunikasi via `localhost`.

---

## 2. Membuat Pod
1.  **Buat Pod:**
    ```bash
    podman pod create --name my-app-stack -p 8080:80
    ```
    *Catatan: Port mapping dilakukan di level Pod, bukan Container.*

2.  **Menambahkan Container ke dalam Pod:**
    ```bash
    # Container Utama (Nginx)
    podman run -d --pod my-app-stack --name nginx-container nginx

    # Container Sidecar (Alpine untuk testing)
    podman run -d --pod my-app-stack --name sidecar-container alpine top
    ```

---

## 3. Komunikasi Antar Container
Karena berbagi network yang sama, container di dalam Pod bisa saling memanggil lewat `localhost`.

Uji coba dari `sidecar-container`:
```bash
podman exec -it sidecar-container wget -qO- localhost
```
Anda akan melihat output HTML dari `nginx-container`.

---

## 4. Perintah Manajemen Pod
*   **List Pod:** `podman pod ps`
*   **Statistik Pod:** `podman pod stats`
*   **Stop/Start Pod:** `podman pod stop my-app-stack`
*   **Hapus Pod:** `podman pod rm -f my-app-stack`

---

## 5. Pod vs Kubernetes
*   **Persamaan**: Keduanya menggunakan konsep grup container yang berbagi port/IP.
*   **Perbedaan**: Podman menjalankan Pod di satu node (lokal), sedangkan Kubernetes mengelola Pod di banyak node (cluster).
*   Podman bisa mengekspor definisi Pod menjadi file YAML yang bisa langsung dijalankan di Kubernetes.

---
**Tugas Lab:**
1. Buat sebuah Pod bernama `db-pod`.
2. Masukkan container `mariadb` ke dalam `db-pod`.
3. Masukkan container `adminer` (tool GUI database) ke dalam `db-pod`.
4. Akses `adminer` dari host dan coba hubungkan ke database menggunakan hostname `localhost`.
