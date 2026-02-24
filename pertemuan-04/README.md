# 🔍 Pertemuan 04: Code Review & Pull Request — Kolaborasi Efektif dalam Tim

<div align="center">

| 📅 Pertemuan | ⏱️ Durasi | 📊 Tingkat |
|:------------:|:---------:|:----------:|
| 04 | 2 x 50 menit | ⭐⭐ Dasar-Menengah |

</div>

---

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan praktikum ini, mahasiswa diharapkan mampu:

| No | Kemampuan yang Dicapai |
|:--:|------------------------|
| 1 | Memahami **pentingnya code review** dalam siklus pengembangan software |
| 2 | Membuat **Pull Request** yang informatif dan mudah di-review |
| 3 | Melakukan **code review** yang konstruktif dan efektif |
| 4 | Menggunakan fitur kolaborasi **GitHub/GitLab** secara profesional |

---

## 📚 Materi Pembelajaran

### 1️⃣ Mengapa Code Review Penting?

> *"Given enough eyeballs, all bugs are shallow."* — Linus's Law

Code review adalah praktik **memeriksa kode yang ditulis orang lain** sebelum digabungkan ke branch utama. Ini adalah salah satu praktik terpenting dalam DevOps.

#### 📊 Statistik Code Review

| Metrik | Tanpa Code Review | Dengan Code Review |
|--------|:-----------------:|:------------------:|
| Bug di Production | Tinggi | **↓ 60-90% lebih rendah** |
| Technical Debt | Menumpuk | Terkontrol |
| Knowledge Sharing | Minimal | **Merata dalam tim** |
| Code Consistency | Bervariasi | Konsisten |

#### 🎯 Manfaat Code Review

```
┌─────────────────────────────────────────────────────────────────┐
│                  MANFAAT CODE REVIEW                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   👥 UNTUK TIM                    🧑‍💻 UNTUK INDIVIDU              │
│   ├── Knowledge transfer          ├── Pembelajaran dari senior   │
│   ├── Standarisasi kode           ├── Feedback konstruktif       │
│   ├── Deteksi bug lebih awal      ├── Membangun kepercayaan diri │
│   └── Dokumentasi implisit        └── Meningkatkan skill coding  │
│                                                                  │
│   🏢 UNTUK ORGANISASI             📦 UNTUK PRODUK                │
│   ├── Mengurangi risiko           ├── Kualitas lebih tinggi      │
│   ├── Compliance & audit          ├── Maintainability            │
│   └── Onboarding lebih mudah      └── Fewer bugs in production   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

### 2️⃣ Anatomi Pull Request yang Baik

**Pull Request (PR)** adalah permintaan untuk menggabungkan perubahan dari satu branch ke branch lain.

#### 📝 Komponen PR yang Lengkap

```markdown
## 📋 Judul PR
feat: implement user authentication system

## 📖 Deskripsi

### Apa yang berubah?
- Menambah halaman login dan register
- Integrasi dengan JWT untuk autentikasi
- Menambah middleware untuk protected routes

### Mengapa perubahan ini diperlukan?
Fitur ini dibutuhkan untuk Issue #42 - User Authentication

### Bagaimana cara menguji?
1. Jalankan `npm run dev`
2. Akses `/login` dan coba login dengan user test
3. Cek bahwa protected routes tidak bisa diakses tanpa token

### Screenshots (jika ada UI changes)
[Lampirkan screenshot]

### Checklist
- [x] Code sudah di-test secara lokal
- [x] Tidak ada console.log atau debug code
- [x] Sudah menambah unit tests
- [ ] Dokumentasi sudah diupdate
```

#### ✅ Best Practices PR

| Aspek | ❌ Hindari | ✅ Lakukan |
|-------|-----------|-----------|
| **Ukuran** | PR dengan 1000+ baris | PR kecil, 200-400 baris |
| **Scope** | Mencampur banyak fitur | 1 PR = 1 tujuan |
| **Judul** | "Update code" | "feat: add login validation" |
| **Deskripsi** | Kosong | Konteks lengkap |
| **Testing** | "Works on my machine" | Bukti testing |

---

### 3️⃣ Cara Melakukan Code Review yang Efektif

#### 🔍 Apa yang Harus Di-Review?

```
┌─────────────────────────────────────────────────────────────────┐
│                    CODE REVIEW CHECKLIST                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  🎯 FUNCTIONALITY                                                │
│  □ Apakah kode melakukan apa yang seharusnya?                   │
│  □ Apakah edge cases sudah ditangani?                           │
│  □ Apakah error handling sudah proper?                          │
│                                                                  │
│  📖 READABILITY                                                  │
│  □ Apakah nama variabel/fungsi deskriptif?                      │
│  □ Apakah kode mudah dipahami?                                  │
│  □ Apakah ada komentar untuk bagian kompleks?                   │
│                                                                  │
│  🏗️ ARCHITECTURE                                                 │
│  □ Apakah struktur kode mengikuti pattern project?              │
│  □ Apakah tidak ada code duplication?                           │
│  □ Apakah separation of concerns terjaga?                       │
│                                                                  │
│  🧪 TESTING                                                      │
│  □ Apakah ada unit tests untuk fitur baru?                      │
│  □ Apakah tests meaningful (bukan hanya coverage)?              │
│                                                                  │
│  🔒 SECURITY                                                     │
│  □ Apakah tidak ada secrets yang ter-commit?                    │
│  □ Apakah input validation sudah dilakukan?                     │
│  □ Apakah tidak ada SQL injection / XSS vulnerability?          │
│                                                                  │
│  ⚡ PERFORMANCE                                                  │
│  □ Apakah tidak ada N+1 query problem?                          │
│  □ Apakah tidak ada memory leaks?                               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

#### 💬 Cara Memberikan Feedback

| Jenis | ❌ Tidak Konstruktif | ✅ Konstruktif |
|-------|---------------------|----------------|
| **Kritik** | "Kode ini jelek" | "Mungkin lebih readable jika menggunakan early return pattern" |
| **Pertanyaan** | "Kenapa begini?" | "Bisa jelaskan alasan memilih pendekatan ini? Saya ingin memahami konteksnya" |
| **Saran** | "Ganti ini" | "Pertimbangkan menggunakan `map()` di sini untuk readability. Contoh: `[code snippet]`" |
| **Pujian** | (tidak ada) | "Nice! Pendekatan ini elegant untuk menangani edge case" |

#### 🏷️ Prefix untuk Review Comments

```
[MUST]     - Harus diperbaiki sebelum merge
[SHOULD]   - Sangat disarankan untuk diperbaiki
[NIT]      - Nitpick, minor improvement (opsional)
[QUESTION] - Pertanyaan untuk klarifikasi
[PRAISE]   - Pujian untuk kode yang bagus
```

**Contoh:**
```
[MUST] Ini akan menyebabkan null pointer exception jika user tidak ada.
Tambahkan pengecekan: `if (!user) return res.status(404).json({error: 'User not found'})`

[NIT] Bisa gunakan template literal untuk readability:
`Welcome, ${user.name}!` daripada "Welcome, " + user.name + "!"
```

---

### 4️⃣ Review Process Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    PULL REQUEST LIFECYCLE                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   Author                              Reviewer                   │
│     │                                    │                       │
│     │  1. Buat PR                        │                       │
│     ├──────────────────────────────────▶│                       │
│     │                                    │                       │
│     │  2. Request Review                 │                       │
│     ├──────────────────────────────────▶│                       │
│     │                                    │                       │
│     │                   3. Review Code   │                       │
│     │◀──────────────────────────────────┤                       │
│     │         (Approve / Request Changes)│                       │
│     │                                    │                       │
│     │  4. Address Feedback               │                       │
│     ├──────────────────────────────────▶│                       │
│     │                                    │                       │
│     │                   5. Re-review     │                       │
│     │◀──────────────────────────────────┤                       │
│     │             (Approve ✅)            │                       │
│     │                                    │                       │
│     │  6. Merge PR 🎉                    │                       │
│     ▼                                    │                       │
│  [main branch updated]                   │                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

### 5️⃣ GitHub Features untuk Code Review

#### 🔧 Fitur-Fitur Penting

| Fitur | Fungsi |
|-------|--------|
| **Review Comments** | Komentar pada baris kode spesifik |
| **Suggestions** | Saran perubahan yang bisa langsung di-apply |
| **Conversations** | Diskusi yang bisa di-resolve |
| **Required Reviews** | Wajib ada approval sebelum merge |
| **CODEOWNERS** | Auto-assign reviewer berdasarkan file |
| **Draft PR** | PR yang masih WIP |
| **Labels** | Kategorisasi PR |

#### 💡 GitHub Suggestion Feature

Di GitHub, Anda bisa memberikan saran kode yang bisa langsung di-apply:

~~~markdown
```suggestion
const greeting = `Hello, ${user.name}!`;
```
~~~

Author bisa langsung klik "Apply suggestion" tanpa perlu edit manual.

---

## 🔧 Tugas Praktikum

### 📋 Prasyarat

Untuk praktikum ini, Anda membutuhkan:
- Akun GitHub
- Partner praktikum (akan dipasangkan oleh asisten lab)
- Repository dari pertemuan sebelumnya

---

### Task 1: Membuat Pull Request yang Baik

**Tujuan:** Praktik membuat PR dengan deskripsi lengkap

#### Langkah-langkah:

```bash
# 1. Clone repository (jika belum)
git clone https://github.com/[username]/[repo].git
cd [repo]

# 2. Buat feature branch
git checkout -b feature/improve-documentation

# 3. Buat perubahan (contoh: tambah file)
cat > CONTRIBUTING.md << 'EOF'
# Contributing Guidelines

## How to Contribute

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a Pull Request

## Code Style

- Use meaningful variable names
- Add comments for complex logic
- Write tests for new features

## Commit Message Format

```
type: subject

body (optional)
```

Types: feat, fix, docs, style, refactor, test, chore
EOF

# 4. Commit dengan message yang baik
git add CONTRIBUTING.md
git commit -m "docs: add contributing guidelines"

# 5. Push dan buat PR
git push -u origin feature/improve-documentation
```

#### Buat PR di GitHub dengan template:

```markdown
## 📋 Description

Menambahkan file CONTRIBUTING.md untuk memudahkan kontributor baru memahami:
- Cara berkontribusi ke project
- Code style yang digunakan
- Format commit message

## 🔗 Related Issue

Closes #[nomor_issue] (jika ada)

## ✅ Checklist

- [x] Dokumentasi sudah di-review
- [x] Tidak ada typo
- [x] Format markdown sudah benar

## 📸 Screenshots

N/A (documentation only)
```

📸 **Screenshot yang diperlukan:**
- Tampilan form pembuatan PR
- PR yang sudah dibuat dengan deskripsi lengkap

---

### Task 2: Melakukan Code Review

**Tujuan:** Praktik memberikan feedback konstruktif

#### Skenario:

Anda akan di-assign untuk me-review PR dari partner. Berikan minimal:
- 2 komentar **konstruktif** (improvement suggestions)
- 1 komentar **question** (untuk klarifikasi)
- 1 komentar **praise** (untuk kode yang bagus)

#### Contoh Review Comments:

```markdown
### Comment 1 - [SHOULD]
Line 15: 
Pertimbangkan untuk menambahkan contoh command untuk setiap langkah.
Ini akan memudahkan kontributor baru yang belum familiar dengan Git.

```suggestion
1. Fork the repository
   ```bash
   # Click "Fork" button on GitHub
   ```
```

### Comment 2 - [QUESTION]
Line 25:
Apakah ada alasan khusus memilih format commit message ini?
Apakah mengikuti Conventional Commits specification?

### Comment 3 - [PRAISE]
Section "Code Style" sangat informatif! 
Ini akan sangat membantu menjaga konsistensi codebase.
```

📸 **Screenshot yang diperlukan:**
- Review comments yang diberikan
- Conversation threads

---

### Task 3: Respond dan Merge

**Tujuan:** Praktik merespons feedback dan menyelesaikan PR

#### Untuk Author (merespons review):

```bash
# 1. Baca semua review comments

# 2. Address feedback yang diperlukan
git checkout feature/improve-documentation

# 3. Buat perubahan berdasarkan feedback
# Edit file sesuai saran

# 4. Commit changes
git add CONTRIBUTING.md
git commit -m "docs: address review feedback - add examples"

# 5. Push updates
git push origin feature/improve-documentation

# 6. Reply ke setiap comment di GitHub
# 7. Request re-review
```

#### Untuk Reviewer (setelah perubahan):

1. Re-review perubahan
2. Jika sudah OK, berikan **Approval**
3. Author bisa **Merge** PR

📸 **Screenshot yang diperlukan:**
- Conversation yang sudah resolved
- Approval dari reviewer
- PR yang sudah di-merge

---

### Task 4: Setup Branch Protection Rules

**Tujuan:** Memahami cara enforce code review

Di repository GitHub:

1. Pergi ke **Settings** → **Branches**
2. Klik **Add rule** untuk branch `main`
3. Aktifkan:
   - ✅ Require a pull request before merging
   - ✅ Require approvals (minimal 1)
   - ✅ Dismiss stale pull request approvals when new commits are pushed
   - ✅ Require conversation resolution before merging

📸 **Screenshot yang diperlukan:**
- Halaman Branch protection rules
- Rules yang sudah dikonfigurasi

---

## 📤 Format Submission

```
📁 NIM_Nama_Pertemuan04/
│
├── 📄 README.md                      # Laporan praktikum
│
├── 📁 task1-create-pr/
│   ├── 📄 pr-description.md          # Copy dari PR description
│   └── 📄 pr-link.txt                # Link ke PR
│
├── 📁 task2-code-review/
│   ├── 📄 review-comments.md         # Dokumentasi comments yang diberikan
│   └── 📄 reviewed-pr-link.txt       # Link ke PR yang di-review
│
├── 📁 task3-respond-merge/
│   └── 📄 conversation-log.md        # Log diskusi dan resolusi
│
├── 📁 task4-branch-protection/
│   └── 📄 rules-configured.md        # Daftar rules yang diaktifkan
│
└── 📁 screenshots/
    ├── 🖼️ 01-pr-form.png
    ├── 🖼️ 02-pr-created.png
    ├── 🖼️ 03-review-comments.png
    ├── 🖼️ 04-conversations.png
    ├── 🖼️ 05-approval.png
    ├── 🖼️ 06-merged.png
    └── 🖼️ 07-branch-protection.png
```

---

## ✅ Checklist Sebelum Submit

- [ ] PR dibuat dengan deskripsi yang lengkap
- [ ] Memberikan minimal 4 review comments (sesuai kategori)
- [ ] Merespons semua feedback dengan baik
- [ ] PR berhasil di-merge setelah approval
- [ ] Branch protection rules sudah dikonfigurasi
- [ ] Semua screenshot lengkap dan jelas

---

## 💡 Tips untuk Code Review yang Efektif

| Tip | Penjelasan |
|-----|------------|
| ⏰ Review dalam 24 jam | Jangan biarkan PR menunggu terlalu lama |
| 🎯 Fokus pada yang penting | Prioritaskan bugs dan security issues |
| 📚 Berikan konteks | Jelaskan "mengapa", bukan hanya "apa" |
| 🤝 Bersikap respectful | Kritik kode, bukan orangnya |
| 🔄 Iterate jika perlu | OK untuk meminta perubahan berkali-kali |
| ✨ Jangan lupa pujian | Acknowledge good code |

---

## 📚 Referensi

| Sumber | Link |
|--------|------|
| Google's Code Review Guidelines | [google.github.io/eng-practices/review](https://google.github.io/eng-practices/review/) |
| GitHub Pull Request Tutorial | [docs.github.com/en/pull-requests](https://docs.github.com/en/pull-requests) |
| Conventional Commits | [conventionalcommits.org](https://www.conventionalcommits.org/) |
| The Art of Giving Code Reviews | [phauer.com/code-reviews](https://phauer.com/2018/code-review-guidelines/) |

---

## ⏰ Deadline

<div align="center">

| 📅 Batas Pengumpulan |
|:--------------------:|
| **Sebelum Pertemuan 05** |

</div>

---

<div align="center">

**Happy Reviewing! 🔍**

*"Code review is not about finding fault, it's about building better software together."*

</div>
