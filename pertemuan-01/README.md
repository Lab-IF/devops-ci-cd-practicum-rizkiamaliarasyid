# 🎭 Pertemuan 01: Pengantar DevOps — Filosofi, Budaya, dan Persiapan Lingkungan

<div align="center">

| 📅 Pertemuan | ⏱️ Durasi | 📊 Tingkat |
|:------------:|:---------:|:----------:|
| 01 | 2 x 50 menit | ⭐ Pemula |

</div>

---

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan praktikum ini, mahasiswa diharapkan mampu:

| No | Kemampuan yang Dicapai |
|:--:|------------------------|
| 1 | Memahami **filosofi dan budaya DevOps** sebagai pendekatan modern dalam pengembangan perangkat lunak |
| 2 | Menjelaskan **DevOps lifecycle** dan hubungan antar tahapannya |
| 3 | Membedakan pendekatan **Development vs Operations tradisional** dengan pendekatan DevOps |
| 4 | Menyiapkan **development environment** yang dibutuhkan untuk praktikum selanjutnya |

---

## 📚 Materi Pembelajaran

### 1️⃣ Apa Itu DevOps?

> *"DevOps bukan hanya tentang tools, melainkan tentang bagaimana tim bekerja sama untuk menghasilkan software yang lebih baik, lebih cepat, dan lebih andal."*

**DevOps** adalah singkatan dari **Development** dan **Operations** — sebuah pendekatan yang menyatukan tim pengembang (*developers*) dan tim operasional (*operations*) untuk bekerja secara kolaboratif sepanjang siklus hidup aplikasi.

#### 🔄 Perbedaan Pendekatan Tradisional vs DevOps

| Aspek | Tradisional | DevOps |
|-------|-------------|--------|
| **Tim** | Terpisah (silo) | Terintegrasi |
| **Deployment** | Manual, jarang | Otomatis, sering |
| **Feedback** | Lambat (mingguan/bulanan) | Cepat (harian/per jam) |
| **Tanggung Jawab** | "Bukan urusan saya" | Tanggung jawab bersama |
| **Infrastruktur** | Statis | Dinamis (Infrastructure as Code) |

---

### 2️⃣ Prinsip Utama DevOps (CALMS)

DevOps dibangun di atas **5 pilar utama** yang dikenal sebagai **CALMS**:

```
┌─────────────────────────────────────────────────────────────┐
│                        CALMS Framework                       │
├─────────────┬─────────────┬─────────────┬─────────────┬─────┤
│   Culture   │  Automation │    Lean     │ Measurement │Share│
│      🤝     │      🤖     │     🎯      │      📊     │  📢 │
└─────────────┴─────────────┴─────────────┴─────────────┴─────┘
```

| Prinsip | Penjelasan | Contoh Praktik |
|---------|------------|----------------|
| **Culture** | Membangun budaya kolaborasi dan kepercayaan | Daily standup, blameless postmortem |
| **Automation** | Mengotomatisasi proses yang berulang | CI/CD pipeline, automated testing |
| **Lean** | Menghilangkan pemborosan dalam alur kerja | Mengurangi handoff, batch size kecil |
| **Measurement** | Mengukur segala sesuatu untuk perbaikan | Metrics, logging, monitoring |
| **Sharing** | Berbagi pengetahuan dan tanggung jawab | Documentation, knowledge base |

---

### 3️⃣ DevOps Lifecycle

Siklus DevOps adalah proses **berkelanjutan** (*continuous*) yang tidak pernah berhenti:

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                    ♾️ DevOps Infinity Loop                 ║
    ╠═══════════════════════════════════════════════════════════╣
    ║                                                           ║
    ║         DEVELOPMENT                    OPERATIONS         ║
    ║    ┌─────────────────────┐      ┌─────────────────────┐  ║
    ║    │  Plan → Code → Build│  →   │ Deploy → Operate →  │  ║
    ║    │         ↓           │      │        ↓            │  ║
    ║    │       Test          │  ←   │     Monitor         │  ║
    ║    └─────────────────────┘      └─────────────────────┘  ║
    ║                                                           ║
    ║    Continuous Integration ←──→ Continuous Delivery        ║
    ╚═══════════════════════════════════════════════════════════╝
```

#### Penjelasan Setiap Tahap:

| Tahap | Deskripsi | Tools yang Umum Digunakan |
|-------|-----------|---------------------------|
| **Plan** | Perencanaan fitur dan sprint | Jira, Trello, GitHub Issues |
| **Code** | Penulisan kode dan version control | Git, VS Code, GitHub |
| **Build** | Kompilasi dan packaging aplikasi | Maven, npm, Docker |
| **Test** | Pengujian otomatis | Jest, pytest, Selenium |
| **Deploy** | Deployment ke environment | Jenkins, GitLab CI, ArgoCD |
| **Operate** | Menjalankan aplikasi di production | Kubernetes, Docker Swarm |
| **Monitor** | Pemantauan performa dan kesehatan | Prometheus, Grafana, ELK |

---

### 4️⃣ Mengapa DevOps Penting?

<div align="center">

| 📈 Manfaat DevOps |
|-------------------|
| ⚡ **Deployment lebih cepat** — dari bulanan menjadi harian |
| 🐛 **Deteksi bug lebih awal** — melalui automated testing |
| 🔄 **Recovery lebih cepat** — saat terjadi masalah |
| 🤝 **Kolaborasi lebih baik** — antar tim |
| 😊 **Kepuasan tim meningkat** — mengurangi pekerjaan repetitif |

</div>

---

## 🔧 Tugas Praktikum

### 📋 Prasyarat

Sebelum memulai, pastikan Anda memiliki:
- Laptop/PC dengan RAM minimal **8GB**
- Koneksi internet yang stabil
- Akses administrator pada komputer Anda

---

### Task 1: Instalasi Development Environment

#### 1.1 Install Git

**Windows:**
```powershell
# Menggunakan winget
winget install Git.Git

# Atau download dari https://git-scm.com/download/win
```

**macOS:**
```bash
brew install git
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt update && sudo apt install git -y
```

#### 1.2 Konfigurasi Git

```bash
# Konfigurasi identitas (WAJIB)
git config --global user.name "Nama Lengkap Anda"
git config --global user.email "email@student.unismuh.ac.id"

# Konfigurasi tambahan (DISARANKAN)
git config --global init.defaultBranch main
git config --global core.editor "code --wait"

# Verifikasi konfigurasi
git config --list
```

#### 1.3 Install Docker Desktop

| Platform | Langkah Instalasi |
|----------|-------------------|
| **Windows** | Download dari [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop), aktifkan WSL2 |
| **macOS** | `brew install --cask docker` atau download dari website |
| **Linux** | Jalankan: `curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh` |

**Verifikasi instalasi:**
```bash
docker --version
docker run hello-world
```

#### 1.4 Install Visual Studio Code + Extensions

1. Download VS Code dari [code.visualstudio.com](https://code.visualstudio.com)
2. Install extensions berikut:

| Extension | Fungsi |
|-----------|--------|
| **Docker** (`ms-azuretools.vscode-docker`) | Manajemen container |
| **GitLens** (`eamodio.gitlens`) | Git supercharged |
| **YAML** (`redhat.vscode-yaml`) | Syntax highlighting YAML |
| **Remote - Containers** (`ms-vscode-remote.remote-containers`) | Development dalam container |

**Install via terminal:**
```bash
code --install-extension ms-azuretools.vscode-docker
code --install-extension eamodio.gitlens
code --install-extension redhat.vscode-yaml
code --install-extension ms-vscode-remote.remote-containers
```

#### 1.5 Buat Akun GitHub

Jika belum memiliki akun GitHub:
1. Kunjungi [github.com](https://github.com)
2. Klik **Sign up**
3. Gunakan email institusi untuk mendapat **GitHub Education Pack**

---

### Task 2: Dokumentasi dan Refleksi

Buat laporan praktikum dengan format berikut:

#### 📝 Isi Laporan

1. **Pemahaman DevOps** (minimal 200 kata)
   - Jelaskan dengan bahasa Anda sendiri: Apa itu DevOps?
   - Mengapa DevOps penting dalam industri software saat ini?
   - Berikan contoh perusahaan yang sukses menerapkan DevOps

2. **Screenshot Bukti Instalasi**
   - Output `git --version` dan `git config --list`
   - Output `docker --version` dan `docker run hello-world`
   - Tampilan VS Code dengan extensions yang terinstall

3. **Refleksi Pribadi** (minimal 100 kata)
   - Apa harapan Anda dari praktikum DevOps ini?
   - Skill apa yang ingin Anda kuasai di akhir semester?

---

## 📤 Format Submission

```
📁 NIM_Nama_Pertemuan01/
│
├── 📄 README.md              # Laporan utama (format Markdown)
│
├── 📁 screenshots/
│   ├── 🖼️ 01-git-version.png
│   ├── 🖼️ 02-git-config.png
│   ├── 🖼️ 03-docker-version.png
│   ├── 🖼️ 04-docker-hello-world.png
│   └── 🖼️ 05-vscode-extensions.png
│
└── 📄 refleksi.md            # Refleksi pribadi
```

---

## ✅ Checklist Sebelum Submit

- [ ] Git terinstall dan terkonfigurasi dengan benar
- [ ] Docker dapat menjalankan container `hello-world`
- [ ] VS Code terinstall dengan semua extensions yang diminta
- [ ] Laporan ditulis dengan bahasa yang baik dan benar
- [ ] Semua screenshot jelas dan terbaca
- [ ] File disusun sesuai struktur yang diminta

---

## 📚 Referensi Bacaan

| Sumber | Link |
|--------|------|
| The Phoenix Project (Book) | Novel tentang transformasi DevOps |
| DevOps Handbook | [itrevolution.com](https://itrevolution.com/the-devops-handbook/) |
| Atlassian DevOps Guide | [atlassian.com/devops](https://www.atlassian.com/devops) |
| Docker Get Started | [docs.docker.com/get-started](https://docs.docker.com/get-started/) |

---

## ⏰ Deadline

<div align="center">

| 📅 Batas Pengumpulan |
|:--------------------:|
| **Sebelum Pertemuan 02** |

</div>

---

<div align="center">

**Selamat mengerjakan! 🚀**

*"The journey of a thousand miles begins with a single step."*

</div>
