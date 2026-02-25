# Lab 12: Podman dalam CI/CD

Dalam modul ini, kita akan mempelajari bagaimana mengintegrasikan Podman ke dalam pipeline Continuous Integration / Continuous Deployment (CI/CD). Karena Podman bersifat daemonless dan rootless, ia sangat cocok digunakan di lingkungan CI/CD yang membutuhkan keamanan tinggi dan isolasi antar build.

## Mengapa Menggunakan Podman di CI/CD?

1.  **Daemonless**: Tidak memerlukan daemon yang berjalan dengan hak akses root.
2.  **Rootless**: Pipeline dapat berjalan sebagai user biasa, meningkatkan keamanan runner.
3.  **Docker-compatible**: Perintah yang digunakan hampir sama dengan Docker, sehingga migrasi pipeline sangat mudah.
4.  **Native Kubernetes**: Bisa menghasilkan YAML Kubernetes yang siap dideploy.

---

## 1. Podman di GitHub Actions

GitHub Actions secara default menyediakan Docker. Namun, Anda bisa menggunakan Podman untuk testing atau build yang lebih aman.

### Contoh Workflow: Build & Push Image
Buat file `.github/workflows/podman-build.yml`:

```yaml
name: Build Image with Podman
on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Login to Registry
        run: |
          echo "${{ secrets.REGISTRY_PASSWORD }}" | podman login quay.io -u "${{ secrets.REGISTRY_USER }}" --password-stdin

      - name: Build Image
        run: |
          podman build -t quay.io/myuser/myapp:latest .

      - name: Push Image
        run: |
          podman push quay.io/myuser/myapp:latest
```

---

## 2. Podman di GitLab CI

GitLab CI sering menggunakan Docker-in-Docker (DinD). Dengan Podman, kita bisa menghindari DinD yang memerlukan mode `--privileged`.

### Contoh `.gitlab-ci.yml`:
```yaml
image: quay.io/podman/stable

stages:
  - build

build_image:
  stage: build
  script:
    - podman login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - podman build -t $CI_REGISTRY_IMAGE:latest .
    - podman push $CI_REGISTRY_IMAGE:latest
```

> **Catatan**: Gunakan image `quay.io/podman/stable` yang sudah terkonfigurasi untuk berjalan di dalam container.

---

## 3. Strategi Keamanan CI/CD

### Menggunakan Buildah
Untuk build yang lebih ringan di CI, Anda bisa menggunakan **Buildah** yang merupakan bagian dari ekosistem Podman. Buildah tidak memerlukan Dockerfile dan bisa memodifikasi image secara efisien.

### Podman Remote
Jika executor CI Anda berada di mesin lain, Anda bisa menggunakan `podman-remote` melalui SSH:
```bash
podman --remote system connection add my-server ssh://user@server-ip/run/user/1000/podman/podman.sock
podman --remote build -t my-app .
```

---

## Tugas: Latihan Mandiri

1.  Buatlah sebuah repository di GitHub/GitLab.
2.  Tambahkan Dockerfile sederhana.
3.  Konfigurasikan Pipeline menggunakan sintaks Podman untuk melakukan build image.
4.  Coba lakukan push ke Registry (seperti Quay.io atau Docker Hub).

---

## Kesimpulan
Selamat! Anda telah menyelesaikan seluruh rangkaian materi lab Podman. Anda sekarang dibekali kemampuan untuk mengelola container, membangun image, orkestrasi pod, integrasi CI/CD, hingga persiapan deployment ke produksi.

[Kembali ke Daftar Modul](../README.md)

