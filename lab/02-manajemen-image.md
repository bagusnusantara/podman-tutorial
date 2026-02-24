# Modul 2: Manajemen Image

## 1. Mencari Image (Searching)
Podman dapat mencari image dari berbagai registry (Docker Hub, Quay.io, etc.).
```bash
podman search nginx
```
Anda akan melihat daftar image yang tersedia. Podman v4+ biasanya meminta konfirmasi registry mana yang ingin digunakan jika ada lebih dari satu opsi.

---

## 2. Menarik Image (Pulling)
Tarik image ke mesin lokal tanpa menjalankannya:
```bash
podman pull docker.io/library/nginx:latest
```
*Tips: Selalu gunakan full qualified image name (misal: `docker.io/...`) untuk menghindari ambiguitas.*

Cek daftar image lokal:
```bash
podman images
```

---

## 3. Inspect Image
Melihat detail metadata sebuah image (format JSON):
```bash
podman inspect nginx
```
Gunakan `--format` untuk mengambil data spesifik (misal: Architecture):
```bash
podman inspect nginx --format '{{.Architecture}}'
```

---

## 4. Tagging Image
Memberikan nama alias atau versi pada image:
```bash
podman tag nginx:latest my-local-nginx:v1
```

---

## 5. Menghapus Image (Removing)
Hapus image yang tidak digunakan:
```bash
podman rmi my-local-nginx:v1
```
Jika image sedang digunakan oleh container, gunakan flag `-f` (force) atau hapus containernya terlebih dahulu.

---

## 6. Konsep Registry
Podman dikonfigurasi melalui file `/etc/containers/registries.conf` (Linux).

### Registry Terkenal:
*   **Docker Hub**: `docker.io`
*   **Quay.io**: `quay.io` (Default Red Hat/Podman)
*   **GitHub Container Registry**: `ghcr.io`

### Login ke Private Registry:
```bash
podman login target.registry.com
```

---
**Tugas Lab:**
1. Cari image `alpine` di registry.
2. Pull image `alpine` versi `3.18`.
3. Beri tag image tersebut menjadi `my-alpine:testing`.
4. Hapus image `my-alpine:testing` tanpa menghapus image aslinya.
