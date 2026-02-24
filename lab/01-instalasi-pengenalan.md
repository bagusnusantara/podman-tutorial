# Modul 1: Instalasi & Pengenalan Podman

## 1. Apa itu Podman?
Podman (**Pod Manager**) adalah tool container engine open-source untuk mengembangkan, mengelola, dan menjalankan container di sistem Linux/macOS/Windows.

### Arsitektur Daemonless
Berbeda dengan Docker yang menggunakan daemon (`dockerd`) dengan hak akses root, Podman bersifat **daemonless**. Setiap container dijalankan sebagai proses anak dari user yang memanggilnya.

### Perbedaan Utama dengan Docker
| Fitur | Docker | Podman |
| :--- | :--- | :--- |
| **Daemon** | Ya (`dockerd`) | Tidak (Daemonless) |
| **Privilege** | Membutuhkan user root | Rootless by default |
| **Unit Terkecil** | Container | Pod (Grup Container) |
| **Kompatibilitas** | - | Mendukung Docker CLI (`alias docker=podman`) |

---

## 2. Instalasi Podman
Pilih sesuai sistem operasi yang Anda gunakan:

### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install -y podman
```

### Windows (WSL2)
1. Install [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install).
2. Download Podman Installer dari [podman.io](https://podman.io/).
3. Jalankan `podman machine init` dan `podman machine start`.

---

## 3. Menjalankan Container Pertama
Mari kita coba menjalankan container sederhana.

### Hello World
```bash
podman run hello-world
```
Podman akan menarik image dari registry (jika belum ada) dan menjalankannya.

### Menjalankan Nginx
Jalankan web server Nginx di background:
```bash
podman run -d -p 8080:80 --name my-web docker.io/library/nginx:latest
```
*   `-d`: Detached mode (background).
*   `-p 8080:80`: Mapping port 8080 host ke port 80 container.

Buka browser dan akses `http://localhost:8080`.

### Verifikasi Container
Cek container yang sedang berjalan:
```bash
podman ps
```

Untuk melihat semua container (termasuk yang mati):
```bash
podman ps -a
```

---
**Tugas Lab:**
1. Install Podman di komputer Anda.
2. Jalankan container `nginx` dan pastikan bisa diakses dari browser.
