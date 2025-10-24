## 📘 Panduan Lengkap Instalasi AtoM dari Repo Pribadi

### 🖥️ Persiapan Awal

1. **Buka terminal di Ubuntu**.
2. Pastikan kamu punya koneksi internet dan akses `sudo`.

---

### 📥 1. Clone Repositori AtoM Milikmu

```bash
git clone https://github.com/anlaotz/atom.git
cd atom
```

---

### 📄 2. Buat Skrip Otomatis `install-atom.sh`

Jalankan ini dari dalam direktori `atom/`:

```bash
nano install-atom.sh
```

Lalu tempel isi skrip berikut:

```bash
#!/bin/bash

set -e

echo "Memulai instalasi AtoM 2.9.x dengan Docker..."

# 1. Install Docker & Compose
echo "Memeriksa & menginstal Docker jika belum ada..."
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin git

# 2. Jalankan kontainer
echo "Menjalankan Docker Compose..."
sudo docker compose -f docker/docker-compose.dev.yml up -d

# 3. Tunggu kontainer siap
echo "Menunggu 10 detik agar semua kontainer siap..."
sleep 10

# 4. Purge database dan isi data demo
echo "Menghapus database dan mengisi data demo..."
sudo docker compose -f docker/docker-compose.dev.yml exec atom php -d memory_limit=-1 symfony tools:purge --demo

# 5. Restart worker
echo "Merestart atom_worker..."
sudo docker compose -f docker/docker-compose.dev.yml restart atom_worker

# 6. Kompilasi tema Bootstrap 5
echo "Mengompilasi tema Bootstrap 5..."
sudo docker compose -f docker/docker-compose.dev.yml exec atom npm install
sudo docker compose -f docker/docker-compose.dev.yml exec atom npm run build

# 7. Kompilasi tema Bootstrap 2
echo "Mengompilasi tema Bootstrap 2..."
sudo docker compose -f docker/docker-compose.dev.yml exec atom make -C plugins/arDominionPlugin

# 8. Clear cache
echo "Menghapus cache Symfony..."
sudo docker compose -f docker/docker-compose.dev.yml exec atom php symfony cc

# 9. Restart semua
echo "Merestart semua kontainer..."
sudo docker compose -f docker/docker-compose.dev.yml restart

# 10. Selesai
echo "Instalasi selesai. AtoM sekarang dapat diakses di:"
echo "http://localhost:63001"
echo "Login: demo@example.com / demo"
```

Tekan:

* `Ctrl + O` lalu `Enter` untuk menyimpan,
* `Ctrl + X` untuk keluar dari `nano`.

---

### 🔓 3. Jadikan Skrip Bisa Dieksekusi

```bash
chmod +x install-atom.sh
```

---

### ▶️ 4. Jalankan Skrip Instalasi

```bash
./install-atom.sh
```

---

### 🌐 5. Akses AtoM di Browser

Setelah instalasi selesai, buka:

```
http://localhost:63001
```

Gunakan kredensial default:

* **Email:** `demo@example.com`
* **Password:** `demo`

---

### 📌 Catatan Tambahan

* Jika muncul error permission docker, gunakan `sudo` di awal perintah.
* Jika clone dari repo `anlaotz/atom` menimbulkan error, pastikan struktur file-nya mengikuti repo `artefactual/atom`.
