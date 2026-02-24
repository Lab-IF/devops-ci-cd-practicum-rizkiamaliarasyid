# 🔀 Pertemuan 03: Git Advanced — Branching Strategies & Collaboration

<div align="center">

| 📅 Pertemuan | ⏱️ Durasi | 📊 Tingkat |
|:------------:|:---------:|:----------:|
| 03 | 2 x 50 menit | ⭐⭐ Dasar-Menengah |

</div>

---

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan praktikum ini, mahasiswa diharapkan mampu:

| No | Kemampuan yang Dicapai |
|:--:|------------------------|
| 1 | Memahami berbagai **branching strategies** dalam pengembangan software |
| 2 | Mengimplementasikan **GitFlow workflow** pada sebuah proyek |
| 3 | Melakukan **merge, rebase**, dan menyelesaikan **merge conflicts** |
| 4 | Mengelola **branch** secara efektif dalam kolaborasi tim |

---

## 📚 Materi Pembelajaran

### 1️⃣ Mengapa Branching Strategy Penting?

> *"Without a branching strategy, you're not doing version control — you're just making backups."*

Branching strategy menentukan **bagaimana tim berkolaborasi** dalam menulis kode, kapan melakukan merge, dan bagaimana mengelola rilis software.

#### Masalah Tanpa Branching Strategy:

| ❌ Masalah | ✅ Solusi dengan Branching Strategy |
|-----------|-------------------------------------|
| Kode production rusak saat development | Branch terpisah untuk production |
| Konflik saat merge berkepanjangan | Feature branch kecil, merge sering |
| Sulit rollback saat ada bug | Release branch untuk versioning |
| Tim saling menunggu | Parallel development di branch berbeda |

---

### 2️⃣ Jenis-Jenis Branching Strategy

#### 📊 Perbandingan Strategi

| Strategy | Kompleksitas | Tim | Use Case |
|----------|:------------:|:---:|----------|
| **GitHub Flow** | ⭐ | Kecil | Continuous deployment |
| **GitFlow** | ⭐⭐⭐ | Menengah-Besar | Release versioning |
| **Trunk-Based** | ⭐⭐ | Berpengalaman | Rapid deployment |
| **GitLab Flow** | ⭐⭐ | Fleksibel | Environment-based |

---

### 3️⃣ GitHub Flow (Simple & Effective)

Strategi paling sederhana, cocok untuk **continuous deployment**.

```
main ─────●────────────●────────────●────────────●─────▶
           \          /             \          /
feature     ●────────●               ●────────●
            (PR + Review)            (PR + Review)
```

#### Aturan GitHub Flow:

1. Branch `main` selalu **deployable**
2. Buat **feature branch** dari `main`
3. Commit secara teratur dengan pesan yang jelas
4. Buka **Pull Request** untuk review
5. Setelah disetujui, **merge ke main**
6. **Deploy** segera setelah merge

```bash
# Workflow GitHub Flow
git checkout main
git pull origin main
git checkout -b feature/login-page
# ... kerjakan fitur ...
git add .
git commit -m "feat: implement login page UI"
git push origin feature/login-page
# → Buat Pull Request di GitHub
# → Setelah review & approve, merge ke main
```

---

### 4️⃣ GitFlow (Comprehensive)

Strategi lengkap untuk tim yang membutuhkan **version control ketat**.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           GITFLOW WORKFLOW                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  main       ●─────────────────────●─────────────────────●──────▶        │
│  (prod)     │                     ↑                     ↑               │
│             │                     │ (merge)             │ (hotfix)      │
│             │              ┌──────┴──────┐       ┌──────┴──────┐        │
│  release    │              │ release/1.0 │       │             │        │
│             │              └──────┬──────┘       │             │        │
│             │                     ↑              │             │        │
│             │                     │ (merge)      │             │        │
│  develop    ●───●───●───●───●───●─┴───●───●───●──┴──●───●───●──▶       │
│             │   ↑   ↑       ↑               ↑                           │
│             │   │   │       │               │                           │
│  feature    │   ●───●       ●───────────────●                           │
│             │ feature/A       feature/B                                  │
│             │                                                            │
│  hotfix     │                                     ●───● hotfix/urgent    │
│             │                                         ↓                  │
│             │                                    (merge to main & dev)   │
└─────────────────────────────────────────────────────────────────────────┘
```

#### Branch dalam GitFlow:

| Branch | Tujuan | Asal | Merge ke |
|--------|--------|------|----------|
| `main` | Production code | - | - |
| `develop` | Integration branch | `main` | `release` |
| `feature/*` | Fitur baru | `develop` | `develop` |
| `release/*` | Persiapan rilis | `develop` | `main` & `develop` |
| `hotfix/*` | Perbaikan urgent | `main` | `main` & `develop` |

---

### 5️⃣ Perintah Git untuk Branching

#### 🌿 Manajemen Branch

```bash
# Melihat semua branch
git branch -a

# Membuat branch baru
git branch feature/login

# Pindah ke branch
git checkout feature/login

# Membuat dan langsung pindah (shortcut)
git checkout -b feature/login

# Menghapus branch lokal
git branch -d feature/login

# Menghapus branch remote
git push origin --delete feature/login

# Rename branch
git branch -m old-name new-name
```

#### 🔄 Merge vs Rebase

**Merge** — Menggabungkan dengan membuat commit baru:
```bash
git checkout develop
git merge feature/login
# Membuat merge commit
```

**Rebase** — Memindahkan commit ke atas branch target:
```bash
git checkout feature/login
git rebase develop
# History linear, tanpa merge commit
```

```
MERGE:                              REBASE:
                                    
    A───B───C (develop)                A───B───C (develop)
         \                                      \
          D───E (feature)                        D'───E' (feature)
         /
    A───B───C───M (setelah merge)      A───B───C───D'───E' (setelah rebase)
```

#### ⚡ Kapan Menggunakan Apa?

| Situasi | Gunakan |
|---------|---------|
| Merge ke `main`/`develop` | **Merge** |
| Update feature branch dengan perubahan terbaru | **Rebase** |
| Branch sudah di-push dan digunakan orang lain | **Merge** |
| Branch lokal, belum di-push | **Rebase** |

---

### 6️⃣ Menyelesaikan Merge Conflicts

Konflik terjadi ketika **dua branch mengubah baris yang sama**.

#### Contoh Konflik:

```
<<<<<<< HEAD
const greeting = "Hello from develop!";
=======
const greeting = "Hello from feature!";
>>>>>>> feature/greeting
```

#### Langkah Menyelesaikan:

```bash
# 1. Merge dan temui konflik
git merge feature/greeting
# Auto-merging file.js
# CONFLICT (content): Merge conflict in file.js

# 2. Buka file, pilih perubahan yang benar
# Hapus marker <<<<<<<, =======, >>>>>>>
# Simpan file dengan konten yang diinginkan

# 3. Tandai sebagai resolved
git add file.js

# 4. Selesaikan merge
git commit -m "resolve merge conflict in file.js"
```

#### Tips Menyelesaikan Konflik:

| Tip | Penjelasan |
|-----|------------|
| 🔍 Pahami kedua perubahan | Jangan asal pilih satu |
| 🧪 Test setelah resolve | Pastikan kode tetap berjalan |
| 💬 Komunikasi | Tanya pemilik perubahan jika ragu |
| 📏 Commit kecil | Kurangi kemungkinan konflik besar |

---

## 🔧 Tugas Praktikum

### Task 1: Implementasi GitHub Flow

**Tujuan:** Memahami workflow sederhana untuk kolaborasi

```bash
# 1. Clone atau buat repository baru
git init github-flow-practice
cd github-flow-practice

# 2. Buat file awal di main
echo "# My Project" > README.md
git add README.md
git commit -m "initial commit"

# 3. Buat feature branch
git checkout -b feature/add-homepage

# 4. Tambah file baru
echo '<!DOCTYPE html>
<html>
<head><title>Homepage</title></head>
<body><h1>Welcome!</h1></body>
</html>' > index.html

git add index.html
git commit -m "feat: add homepage"

# 5. Push branch
git push -u origin feature/add-homepage

# 6. Buat Pull Request di GitHub (screenshot ini!)

# 7. Merge ke main (setelah review)
git checkout main
git merge feature/add-homepage
git push origin main

# 8. Hapus feature branch
git branch -d feature/add-homepage
git push origin --delete feature/add-homepage
```

📸 **Screenshot yang diperlukan:**
- Output `git log --oneline --graph`
- Tampilan Pull Request di GitHub
- Branch list sebelum dan sesudah merge

---

### Task 2: Implementasi GitFlow

**Tujuan:** Memahami workflow lengkap untuk release management

```bash
# 1. Setup GitFlow
git checkout main
git checkout -b develop
git push -u origin develop

# 2. Buat feature branch
git checkout -b feature/user-auth develop

# 3. Kerjakan fitur (buat beberapa commit)
echo "login functionality" > auth.js
git add auth.js
git commit -m "feat: add login function"

echo "logout functionality" >> auth.js
git add auth.js
git commit -m "feat: add logout function"

# 4. Merge ke develop
git checkout develop
git merge --no-ff feature/user-auth -m "Merge feature/user-auth into develop"
git push origin develop

# 5. Buat release branch
git checkout -b release/1.0.0 develop

# 6. Bump version (simulasi)
echo "version: 1.0.0" > version.txt
git add version.txt
git commit -m "chore: bump version to 1.0.0"

# 7. Merge release ke main
git checkout main
git merge --no-ff release/1.0.0 -m "Release 1.0.0"
git tag -a v1.0.0 -m "Version 1.0.0"
git push origin main --tags

# 8. Merge release ke develop
git checkout develop
git merge --no-ff release/1.0.0 -m "Merge release/1.0.0 into develop"
git push origin develop

# 9. Cleanup
git branch -d feature/user-auth
git branch -d release/1.0.0
```

📸 **Screenshot yang diperlukan:**
- Output `git log --oneline --graph --all` (menampilkan semua branch)
- Output `git tag` menampilkan tag yang dibuat
- Visualisasi branch (gunakan GitLens atau `gitk`)

---

### Task 3: Resolve Merge Conflict

**Tujuan:** Berlatih menyelesaikan konflik merge

```bash
# 1. Setup skenario konflik
git checkout main
echo "Original content" > conflict.txt
git add conflict.txt
git commit -m "add original file"

# 2. Buat branch A dan ubah file
git checkout -b branch-a
echo "Content from Branch A" > conflict.txt
git add conflict.txt
git commit -m "change from branch A"

# 3. Kembali ke main, buat branch B
git checkout main
git checkout -b branch-b
echo "Content from Branch B" > conflict.txt
git add conflict.txt
git commit -m "change from branch B"

# 4. Merge branch-a ke main
git checkout main
git merge branch-a  # Sukses, tidak ada konflik

# 5. Coba merge branch-b (akan konflik!)
git merge branch-b
# CONFLICT!

# 6. Lihat status
git status

# 7. Resolve konflik (edit file secara manual)
# Pilih konten yang diinginkan, hapus conflict markers

# 8. Selesaikan merge
git add conflict.txt
git commit -m "resolve: merge conflict between branch-a and branch-b"
```

📸 **Screenshot yang diperlukan:**
- Output `git status` saat konflik
- Isi file dengan conflict markers
- Isi file setelah resolve
- Output `git log` setelah merge berhasil

---

## 📤 Format Submission

```
📁 NIM_Nama_Pertemuan03/
│
├── 📄 README.md                      # Laporan praktikum
│
├── 📁 task1-github-flow/
│   ├── 📄 commands.md                # Daftar commands yang dijalankan
│   └── 📄 pr-link.txt                # Link ke Pull Request
│
├── 📁 task2-gitflow/
│   ├── 📄 commands.md
│   └── 📄 branch-diagram.md          # Penjelasan setiap branch
│
├── 📁 task3-conflict/
│   ├── 📄 conflict-before.txt        # File sebelum resolve
│   ├── 📄 conflict-after.txt         # File setelah resolve
│   └── 📄 resolution-steps.md        # Langkah-langkah resolve
│
└── 📁 screenshots/
    ├── 🖼️ 01-github-flow-pr.png
    ├── 🖼️ 02-github-flow-graph.png
    ├── 🖼️ 03-gitflow-branches.png
    ├── 🖼️ 04-gitflow-tags.png
    ├── 🖼️ 05-conflict-status.png
    ├── 🖼️ 06-conflict-markers.png
    └── 🖼️ 07-conflict-resolved.png
```

---

## ✅ Checklist Sebelum Submit

- [ ] Memahami perbedaan GitHub Flow dan GitFlow
- [ ] Berhasil membuat dan merge feature branch
- [ ] Berhasil membuat release dengan GitFlow
- [ ] Berhasil menyelesaikan merge conflict
- [ ] Semua screenshot lengkap dan jelas
- [ ] Commands terdokumentasi dengan baik

---

## 💡 Tips & Best Practices

| Tip | Penjelasan |
|-----|------------|
| 📝 Commit message yang jelas | Gunakan format: `type: description` |
| 🌿 Nama branch deskriptif | `feature/login-page`, bukan `branch1` |
| 🔄 Pull sebelum push | Hindari konflik dengan `git pull --rebase` |
| 🗑️ Hapus branch setelah merge | Jaga kebersihan repository |
| 📊 Review sebelum merge | Jangan merge tanpa code review |

#### Konvensi Commit Message:

```
feat: menambah fitur baru
fix: memperbaiki bug
docs: perubahan dokumentasi
style: formatting, tanpa perubahan logic
refactor: restructuring code tanpa mengubah behavior
test: menambah atau memperbaiki test
chore: maintenance tasks
```

---

## 📚 Referensi

| Sumber | Link |
|--------|------|
| Atlassian Git Tutorials | [atlassian.com/git/tutorials](https://www.atlassian.com/git/tutorials) |
| GitFlow Original Post | [nvie.com/posts/a-successful-git-branching-model](https://nvie.com/posts/a-successful-git-branching-model/) |
| GitHub Flow Guide | [docs.github.com/en/get-started/quickstart/github-flow](https://docs.github.com/en/get-started/quickstart/github-flow) |
| Learn Git Branching | [learngitbranching.js.org](https://learngitbranching.js.org/) |

---

## ⏰ Deadline

<div align="center">

| 📅 Batas Pengumpulan |
|:--------------------:|
| **Sebelum Pertemuan 04** |

</div>

---

<div align="center">

**Happy Branching! 🌿**

*"Branching is cheap in Git. Use it liberally."*

</div>
