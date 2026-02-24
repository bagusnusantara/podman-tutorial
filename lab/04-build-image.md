# Modul 4: Build Custom Image

## 1. Membuat Dockerfile
Dockerfile adalah file teks yang berisi instruksi untuk membuat sebuah image. Podman juga mendukung file bernama `Containerfile`.

Contoh aplikasi web sederhana (HTML):
```bash
mkdir my-app && cd my-app
echo "<h1>Hello from Podman Build!</h1>" > index.html
```

Buat file bernama `Dockerfile`:
```dockerfile
# Menggunakan image base yang ringan
FROM docker.io/library/nginx:alpine

# Menyalin file lokal ke dalam folder web server di container
COPY index.html /usr/share/nginx/html/index.html

# Port yang akan digunakan
EXPOSE 80
```

---

## 2. Membangun Image (Build)
Gunakan perintah `podman build` untuk memproses Dockerfile:
```bash
podman build -t my-custom-nginx:v1 .
```
*Tip: Tanda titik `.` berarti context build adalah direktori saat ini.*

Cek hasilnya:
```bash
podman images | grep my-custom-nginx
```

---

## 3. Multi-stage Build
Digunakan untuk membuat image yang sangat kecil dengan memisahkan proses build/compile dan proses runtime.

Contoh `Dockerfile` untuk aplikasi Go:
```dockerfile
# Stage 1: Build / Compile
FROM docker.io/library/golang:1.21-alpine AS builder
WORKDIR /app
COPY . .
RUN go build -o main .

# Stage 2: Runtime (Final Image)
FROM docker.io/library/alpine:latest
WORKDIR /root/
COPY --from=builder /app/main .
CMD ["./main"]
```
*Hasilnya, image final tidak akan berisi compiler Go, sehingga jauh lebih kecil dan aman.*

---

## 4. Best Practice
1.  **Gunakan Base Image Minimal**: Gunakan `alpine` atau `distroless` daripada `ubuntu` atau `centos`.
2.  **Kurangi Layer**: Gabungkan perintah `RUN` menggunakan `&&`.
3.  **Gunakan .dockerignore**: Jangan sertakan file yang tidak perlu (seperti `.git` atau `node_modules`).
4.  **No Root**: Usahakan aplikasi di dalam container tidak berjalan sebagai root (USER directive).

---
**Tugas Lab:**
1. Buat Dockerfile sederhana menggunakan base image `python:3.9-slim`.
2. Masukkan script Python `hello.py` yang mencetak "Hello Podman".
3. Build image tersebut dengan nama `python-hello:latest`.
4. Jalankan containernya dan pastikan output muncul.
