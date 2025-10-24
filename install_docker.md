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

### 🔓 2. Jadikan Skrip Bisa Dieksekusi

```bash
chmod +x install-atom.sh
```

---

### ▶️ 3. Jalankan Skrip Instalasi

```bash
./install-atom.sh
```

---

### 🌐 4. Akses AtoM di Browser

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
