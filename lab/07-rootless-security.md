# Modul 7: Rootless Container & Security

Salah satu keunggulan utama Podman adalah kemampuannya menjalankan container tanpa akses root (**Rootless**).

## 1. Bagaimana Rootless Bekerja?
Podman menggunakan fitur kernel Linux bernama **User Namespaces**. Fitur ini memetakan user biasa di mesin host menjadi user "root" (UID 0) di dalam container.

### SubUID dan SubGID
File `/etc/subuid` dan `/etc/subgid` menyimpan jangkauan (range) ID yang boleh digunakan oleh setiap user untuk proses rootless.
Cek isi file tersebut:
```bash
cat /etc/subuid
cat /etc/subgid
```
Contoh isi: `nusantara:100000:65536`. Ini artinya user `nusantara` punya 65.536 ID tambahan mulai dari 100.000.

---

## 2. Root vs Rootless
| Fitur | Root Mode (Docker-like) | Rootless Mode (Podman Default) |
| :--- | :--- | :--- |
| **Keamanan** | Berisiko (Privilege Escalation) | Lebih Aman (Limited Access) |
| **Network** | Bridge asli, performa tinggi | Menggunakan `slirp4netns` (overhead sedikit) |
| **Mengkases Port < 1024** | Bisa langsung | Harus pakai port forwarding > 1024 |
| **Akses File Host** | Sesuai izin root | Sesuai izin user yang menjalankan |

---

## 3. Security Hardening
Podman memberikan kontrol ketat untuk membatasi apa yang bisa dilakukan container.

### Read-Only Root Filesystem
Mencegah aplikasi di container mengubah file system-nya sendiri (mencegah malware):
```bash
podman run --read-only nginx
```

### No New Privileges
Mencegah proses di dalam container mendapatkan hak akses lebih tinggi:
```bash
podman run --security-opt no-new-privileges nginx
```

### Menghapus Capability
Secara default, container punya beberapa izin khusus (capabilities). Kita bisa menghapus semuanya:
```bash
podman run --cap-drop=all alpine ps
```

---

## 4. Rootless Mapping (Unshare)
Jika Anda butuh masuk ke environment namespace yang digunakan Podman:
```bash
podman unshare cat /proc/self/uid_map
```
Ini berguna untuk melakukan debugging permission file tanpa menjadi root asli.

---
**Tugas Lab:**
1. Cek isi file `/etc/subuid` di server Anda.
2. Jalankan container `nginx` dengan flag `--read-only`. Cobalah masuk ke dalam container (`exec`) dan buat file baru. Apa yang terjadi?
3. Jalankan container dengan menghapus semua capabilities (`--cap-drop=all`) dan diskusikan dampaknya pada aplikasi kompleks.
