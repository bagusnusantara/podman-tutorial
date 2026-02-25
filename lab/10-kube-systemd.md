# Modul 10: Generate Kubernetes YAML & Systemd

Podman dirancang agar kompatibel dengan ekosistem orkestrasi modern. Anda bisa dengan mudah memindahkan beban kerja dari local development ke produksi (Kubernetes atau Systemd).

## 1. Podman Generate Kube
Podman bisa membuat definisi file YAML standar yang bisa dibaca oleh Kubernetes.

1.  **Jalankan Container/Pod:**
    ```bash
    podman run -d --name kube-web -p 8080:80 nginx
    ```
2.  **Generate YAML:**
    ```bash
    podman generate kube kube-web > web-deployment.yaml
    ```
3.  **Deploy ke Kubernetes (K3S/Minikube):**
    ```bash
    kubectl apply -f web-deployment.yaml
    ```
    *Keuntungannya: Anda bisa bereksperimen di lokal dengan Podman, lalu deploy ke cluster tanpa menulis YAML dari nol.*

---

## 2. Podman Play Kube
Kebalikannya, Anda juga bisa menjalankan file YAML Kubernetes langsung di dalam Podman lokal.
```bash
podman play kube web-deployment.yaml
```

---

## 3. Podman & Systemd
Di server produksi, kita ingin container otomatis berjalan kembali jika server restart. Podman mendukung pembuatan file unit Systemd.

1.  **Generate Systemd Unit:**
    ```bash
    # Masuk ke folder unit systemd user
    mkdir -p ~/.config/systemd/user/
    cd ~/.config/systemd/user/

    podman generate systemd --name kube-web --new --files
    ```
2.  **Aktivasi Service:**
    ```bash
    systemctl --user daemon-reload
    systemctl --user enable --now container-kube-web.service
    ```

---

## 4. Keuntungan Systemd (Rootless)
Dengan Systemd, container Anda akan dikelola selayaknya service native OS:
*   Auto-restart jika aplikasi crash.
*   Manajemen dependensi antar service.
*   Logging terpusat melalui `journalctl`.

Agar container tetap berjalan meskipun user logout, aktifkan **lingering**:
```bash
sudo loginctl enable-linger $USER
```

---
**Tugas Lab:**
1. Buat sebuah container `redis` dengan nama `my-redis`.
2. Generate file YAML Kubernetes dari container tersebut.
3. Hapus containernya, lalu jalankan kembali menggunakan `podman play kube`.
4. Generate file Systemd untuk container tersebut dan pastikan service-nya `active (running)`.

---
---
[Lanjut ke Lab 11: Podman Desktop](./11-podman-desktop.md)
[Kembali ke Daftar Modul](../README.md)

