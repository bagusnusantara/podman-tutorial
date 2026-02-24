# Modul 3: Manajemen Container

## 1. Siklus Hidup Container
Belajar mengontrol status container.

### Start, Stop, Restart
```bash
# Hentikan container
podman stop my-web

# Jalankan kembali
podman start my-web

# Restart
podman restart my-web
```

---

## 2. Logs & Exec
Melihat apa yang terjadi di dalam container.

### Melihat Log
```bash
podman logs -f my-web
```
Flag `-f` (follow) untuk melihat log secara real-time.

### Masuk ke dalam Container
Menjalankan perintah di dalam container yang sedang berjalan:
```bash
podman exec -it my-web bash
```
Atau jika image minimal (seperti Alpine):
```bash
podman exec -it my-web sh
```

---

## 3. Inspect Container
Melihat konfigurasi runtime container (IP Address, Mounts, Ports):
```bash
podman inspect my-web
```
Cek IP Address container:
```bash
podman inspect my-web --format '{{.NetworkSettings.IPAddress}}'
```

---

## 4. Resource Limit
Membatasi penggunaan CPU dan Memory agar container tidak "memakan" seluruh resource host.

### Membatasi Memory
```bash
podman run -d --name limited-nginx --memory 256m nginx
```

### Membatasi CPU
```bash
podman run -d --name cpu-limited --cpus 0.5 nginx
```
*0.5 berarti container hanya boleh menggunakan 50% dari 1 core CPU.*

### Verifikasi Resource
Cek statistik pemakaian resource secara real-time:
```bash
podman stats
```

---

## 5. Membersihkan Container
Menghapus container:
```bash
podman rm -f my-web
```

Menghapus semua container yang sudah berhenti (cleanup):
```bash
podman container prune
```

---
**Tugas Lab:**
1. Jalankan container `httpd` (Apache) dengan nama `apache-lab` dan limitasi memory `128MB`.
2. Masuk ke dalam container tersebut menggunakan `exec` dan buat file `test.txt` di folder `/var/www/html/`.
3. Cek log dari container tersebut.
4. Hapus container tersebut secara paksa.
