# Modul 11: Podman Desktop (GUI)

Podman Desktop adalah antarmuka grafis (GUI) yang memudahkan pengelolaan container, image, dan pod tanpa harus selalu menggunakan command line.

## 1. Instalasi Podman Desktop
Podman Desktop tersedia untuk Windows, macOS, dan Linux.

1.  Buka situs resmi [podman-desktop.io](https://podman-desktop.io/).
2.  Download installer sesuai OS Anda.
3.  Jalankan installer dan ikuti petunjuk setup.
4.  Podman Desktop akan otomatis mendeteksi instalasi Podman di sistem Anda atau menawarkan untuk menginstalnya jika belum ada.

---

## 2. Fitur Utama
Setelah dibuka, Anda akan melihat beberapa menu utama di sidebar kiri:

### Dashboard
Melihat status "Podman Machine" (pada Windows/macOS) dan ringkasan resource yang berjalan.

### Images
-   **Pull Image:** Mencari dan mendownload image dari registry (seperti Docker Hub atau Quay.io).
-   **Build Image:** Membuat image dari Dockerfile secara visual.
-   **Run Container:** Menjalankan container langsung dari list image.

### Containers
-   Melihat status container (Running, Stopped).
-   Melihat logs secara real-time.
-   Membuka Terminal ke dalam container dengan satu klik.
-   Me-restart, men-stop, atau menghapus container.

### Pods
Fitur unik Podman untuk mengelola grup container. Anda bisa membuat Pod baru atau menggabungkan container yang sudah ada ke dalam satu Pod.

---

## 3. Manajemen Podman Machine (Windows/macOS)
Pada sistem non-Linux, Podman berjalan di dalam Virtual Machine kecil.
-   Anda bisa men-start/stop VM ini melalui Podman Desktop di bagian **Settings > Resources**.
-   Anda bisa menambah resource (CPU/Memory) yang dialokasikan untuk Podman.

---

## 4. Integrasi Kubernetes
Podman Desktop memudahkan transisi ke Kubernetes:
-   **Local Kubernetes:** Anda bisa mengaktifkan cluster lokal seperti Kind atau Minikube langsung dari settings.
-   **Deploy to Kubernetes:** Klik kanan pada Container atau Pod, lalu pilih "Deploy to Kubernetes" untuk menghasilkan file YAML secara otomatis.

---

**Tugas Lab:**
1.  Install Podman Desktop di komputer Anda.
2.  Gunakan fitur **Images** untuk mencari image `nginx` dan jalankan menggunakan GUI.
3.  Cek bagian **Containers**, buka tab **Logs** dan sertakan screenshot-nya.
4.  Coba stop dan hapus container tersebut melalui Podman Desktop.
