# 🏆 Pertemuan 08: UTS — Complete CI/CD Pipeline Project

<div align="center">

| 📅 Pertemuan | ⏱️ Durasi | 📊 Tingkat |
|:------------:|:---------:|:----------:|
| 08 (UTS) | 3 x 50 menit | ⭐⭐⭐⭐ Lanjut |

---

### 🎯 Ujian Tengah Semester
**Praktikum DevOps & CI/CD**

</div>

---

## 📋 Deskripsi Ujian

Bangun **end-to-end CI/CD pipeline** yang lengkap untuk sebuah aplikasi web. Ujian ini menguji pemahaman komprehensif Anda tentang seluruh materi yang telah dipelajari dari Pertemuan 01-07.

---

## 🎯 Tujuan Ujian

| No | Kompetensi yang Dinilai |
|:--:|-------------------------|
| 1 | Mampu membangun **aplikasi web** dengan struktur yang baik |
| 2 | Mampu membuat **Dockerfile** dan **Docker Compose** yang proper |
| 3 | Mampu mengimplementasikan **automated testing** |
| 4 | Mampu membangun **CI/CD pipeline** end-to-end |
| 5 | Mampu melakukan **deployment** ke multiple environments |
| 6 | Mampu mendokumentasikan proyek dengan baik |

---

## 📝 Requirements

### 📦 A. Aplikasi (25%)

Buat aplikasi web sederhana dengan kriteria berikut:

| Kriteria | Poin | Detail |
|----------|:----:|--------|
| REST API | 10 | Minimal 3 endpoint (CRUD operations) |
| Health Check | 5 | `/health` dan `/ready` endpoint |
| Dokumentasi API | 5 | README atau Swagger/OpenAPI |
| Clean Code | 5 | Code terstruktur dan readable |

**Pilihan Bahasa/Framework:**
- 🐍 Python (Flask/FastAPI) — *Direkomendasikan*
- 🟢 Node.js (Express)
- 🔵 Go (Gin/Echo)

**Contoh Fitur Aplikasi:**
- Todo List API
- Simple Notes API
- Product Catalog API
- User Management API

---

### 🔄 B. CI Pipeline (25%)

| Kriteria | Poin | Detail |
|----------|:----:|--------|
| Automated Build | 5 | Build Docker image on push |
| Unit Testing | 10 | Minimal 10 unit tests dengan coverage report |
| Code Linting | 5 | Automated code quality check |
| Test Reports | 5 | Test results terintegrasi di Jenkins |

```groovy
// Contoh CI Stage Structure
stages {
    stage('Build') { ... }
    stage('Lint') { ... }
    stage('Unit Test') { ... }
    stage('Coverage Report') { ... }
}
```

---

### 🚀 C. CD Pipeline (25%)

| Kriteria | Poin | Detail |
|----------|:----:|--------|
| Deploy to Staging | 8 | Automated deployment ke staging |
| Health Check | 5 | Verifikasi deployment berhasil |
| Approval Gate | 5 | Manual approval untuk production |
| Deploy to Production | 7 | Deployment dengan proper configs |

```groovy
// Contoh CD Stage Structure
stages {
    stage('Deploy Staging') { ... }
    stage('Staging Health Check') { ... }
    stage('Approval') { ... }
    stage('Deploy Production') { ... }
    stage('Production Health Check') { ... }
}
```

---

### 📚 D. Dokumentasi (15%)

| Kriteria | Poin | Detail |
|----------|:----:|--------|
| README.md | 5 | Setup instructions, API docs |
| Architecture Diagram | 5 | Diagram arsitektur aplikasi |
| Pipeline Flow Diagram | 5 | Diagram alur CI/CD pipeline |

---

### 🎤 E. Presentasi (10%)

| Kriteria | Poin | Detail |
|----------|:----:|--------|
| Demo Live | 5 | Menunjukkan pipeline berjalan |
| Penjelasan | 3 | Menjelaskan arsitektur dan keputusan |
| Q&A | 2 | Menjawab pertanyaan dengan baik |

---

## 📁 Struktur Project yang Diharapkan

```
📁 NIM_Nama_UTS/
│
├── 📄 README.md                      # Dokumentasi utama
│
├── 📁 app/                           # Source code aplikasi
│   ├── 📄 __init__.py
│   ├── 📄 main.py                    # Entry point
│   ├── 📄 routes.py                  # API routes
│   ├── 📄 models.py                  # Data models
│   └── 📄 utils.py                   # Utility functions
│
├── 📁 tests/                         # Test files
│   ├── 📄 __init__.py
│   ├── 📄 test_routes.py
│   ├── 📄 test_models.py
│   └── 📄 conftest.py                # pytest fixtures
│
├── 📄 Dockerfile                     # Docker image definition
├── 📄 docker-compose.yml             # Development environment
├── 📄 docker-compose.staging.yml     # Staging environment
├── 📄 docker-compose.prod.yml        # Production environment
│
├── 📄 Jenkinsfile                    # CI/CD Pipeline
├── 📄 requirements.txt               # Python dependencies
├── 📄 .flake8                        # Linting config
│
├── 📁 docs/                          # Documentation
│   ├── 📄 architecture.md            # Architecture explanation
│   ├── 🖼️ architecture-diagram.png
│   └── 🖼️ pipeline-flow-diagram.png
│
└── 📁 screenshots/                   # Evidence
    ├── 🖼️ 01-app-running.png
    ├── 🖼️ 02-api-endpoints.png
    ├── 🖼️ 03-unit-tests.png
    ├── 🖼️ 04-coverage-report.png
    ├── 🖼️ 05-jenkins-pipeline.png
    ├── 🖼️ 06-staging-deployment.png
    ├── 🖼️ 07-approval-gate.png
    └── 🖼️ 08-production-deployment.png
```

---

## 💡 Template Starter

Untuk membantu Anda memulai, berikut template yang dapat digunakan:

### 📝 Template README.md

```markdown
# 🚀 [Nama Project] - UTS DevOps CI/CD

## 📋 Informasi Mahasiswa

| Item | Detail |
|------|--------|
| Nama | [Nama Lengkap] |
| NIM | [NIM] |
| Kelas | [Kelas] |

## 📖 Deskripsi Project

[Jelaskan aplikasi yang Anda buat dalam 2-3 paragraf]

## 🛠️ Tech Stack

- **Language**: Python 3.11
- **Framework**: Flask
- **Testing**: pytest
- **CI/CD**: Jenkins
- **Container**: Docker

## 🚀 Quick Start

### Prerequisites
- Docker & Docker Compose
- Python 3.11+
- Git

### Running Locally
\`\`\`bash
# Clone repository
git clone [repo-url]
cd [project-name]

# Run with Docker Compose
docker compose up -d

# Access application
open http://localhost:5000
\`\`\`

## 📡 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/health` | Health check |
| GET | `/api/items` | Get all items |
| POST | `/api/items` | Create new item |
| GET | `/api/items/{id}` | Get item by ID |
| PUT | `/api/items/{id}` | Update item |
| DELETE | `/api/items/{id}` | Delete item |

## 🧪 Running Tests

\`\`\`bash
# Run tests with coverage
pytest --cov=app --cov-report=html

# View coverage report
open htmlcov/index.html
\`\`\`

## 🔄 CI/CD Pipeline

[Jelaskan pipeline Anda di sini atau referensikan ke docs/]

## 📊 Screenshots

[Referensikan screenshots di folder screenshots/]

## 📚 References

- [Link ke resource yang digunakan]
```

### 📝 Template Jenkinsfile Lengkap

```groovy
pipeline {
    agent any
    
    environment {
        APP_NAME = 'uts-app'
        DOCKER_IMAGE = "${APP_NAME}"
        STAGING_PORT = '8081'
        PROD_PORT = '8080'
    }
    
    stages {
        // ==================== CI STAGES ====================
        
        stage('📥 Checkout') {
            steps {
                echo '📥 Checking out source code...'
                checkout scm
            }
        }
        
        stage('🔨 Build') {
            steps {
                echo '🔨 Building Docker image...'
                sh '''
                    docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .
                    docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest
                '''
            }
        }
        
        stage('🔍 Lint') {
            steps {
                echo '🔍 Running code linting...'
                sh '''
                    docker run --rm ${DOCKER_IMAGE}:${BUILD_NUMBER} \
                        sh -c "pip install flake8 && flake8 app/ --max-line-length=120" || true
                '''
            }
        }
        
        stage('🧪 Unit Tests') {
            steps {
                echo '🧪 Running unit tests...'
                sh '''
                    docker run --rm \
                        -v $(pwd)/reports:/app/reports \
                        ${DOCKER_IMAGE}:${BUILD_NUMBER} \
                        sh -c "pip install pytest pytest-cov && \
                               pytest tests/ -v \
                               --junitxml=reports/test-results.xml \
                               --cov=app \
                               --cov-report=xml:reports/coverage.xml \
                               --cov-report=html:reports/coverage-html"
                '''
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'reports/test-results.xml'
                }
            }
        }
        
        stage('📊 Coverage Report') {
            steps {
                echo '📊 Publishing coverage report...'
                publishHTML([
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reports/coverage-html',
                    reportFiles: 'index.html',
                    reportName: 'Coverage Report'
                ])
            }
        }
        
        // ==================== CD STAGES ====================
        
        stage('🚀 Deploy to Staging') {
            steps {
                echo '🚀 Deploying to Staging...'
                sh '''
                    docker compose -f docker-compose.staging.yml down || true
                    BUILD_NUMBER=${BUILD_NUMBER} docker compose -f docker-compose.staging.yml up -d
                    sleep 10
                '''
            }
        }
        
        stage('🏥 Staging Health Check') {
            steps {
                echo '🏥 Verifying staging deployment...'
                sh '''
                    curl -f http://localhost:${STAGING_PORT}/health || exit 1
                    echo "✅ Staging is healthy!"
                '''
            }
        }
        
        stage('🧪 Staging Smoke Tests') {
            steps {
                echo '🧪 Running smoke tests on staging...'
                sh '''
                    # Test all endpoints
                    curl -f http://localhost:${STAGING_PORT}/health
                    curl -f http://localhost:${STAGING_PORT}/api/items || true
                    echo "✅ Smoke tests passed!"
                '''
            }
        }
        
        stage('⏸️ Production Approval') {
            steps {
                input message: '''
                    ═══════════════════════════════════════
                    🚨 PRODUCTION DEPLOYMENT APPROVAL
                    ═══════════════════════════════════════
                    
                    Build: ${BUILD_NUMBER}
                    Staging: http://localhost:8081
                    
                    Proceed to production?
                ''',
                ok: '✅ Deploy to Production'
            }
        }
        
        stage('🚀 Deploy to Production') {
            steps {
                echo '🚀 Deploying to Production...'
                sh '''
                    docker compose -f docker-compose.prod.yml down || true
                    BUILD_NUMBER=${BUILD_NUMBER} docker compose -f docker-compose.prod.yml up -d
                    sleep 10
                '''
            }
        }
        
        stage('🏥 Production Health Check') {
            steps {
                echo '🏥 Verifying production deployment...'
                sh '''
                    for i in 1 2 3 4 5; do
                        if curl -f http://localhost:${PROD_PORT}/health; then
                            echo "✅ Production is healthy!"
                            exit 0
                        fi
                        sleep 5
                    done
                    exit 1
                '''
            }
            post {
                failure {
                    sh '''
                        echo "🔙 Rolling back production..."
                        docker compose -f docker-compose.prod.yml down
                    '''
                }
            }
        }
        
        stage('✅ Deployment Summary') {
            steps {
                echo '''
                ═══════════════════════════════════════════
                        🎉 DEPLOYMENT COMPLETE!
                ═══════════════════════════════════════════
                
                🔷 Staging:    http://localhost:8081
                🟢 Production: http://localhost:8080
                
                ═══════════════════════════════════════════
                '''
            }
        }
    }
    
    post {
        success {
            echo '🎉 Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs for details.'
        }
        always {
            cleanWs()
        }
    }
}
```

---

## 📊 Kriteria Penilaian Detail

<div align="center">

| Komponen | Bobot | Kriteria Nilai A (85-100) | Kriteria Nilai B (70-84) | Kriteria Nilai C (55-69) |
|----------|:-----:|---------------------------|--------------------------|--------------------------|
| **Aplikasi** | 25% | API lengkap, clean code, health check | API berfungsi, code cukup baik | API minimal, code kurang terstruktur |
| **CI Pipeline** | 25% | Build + test + lint + coverage | Build + test + lint | Build + test saja |
| **CD Pipeline** | 25% | Multi-env + approval + health check | Multi-env + approval | Single environment |
| **Dokumentasi** | 15% | Lengkap dengan diagram | Cukup lengkap | Minimal |
| **Presentasi** | 10% | Demo lancar, penjelasan jelas | Demo lancar | Demo dengan bantuan |

</div>

---

## ⚠️ Peraturan Ujian

### ✅ Diperbolehkan:
- Menggunakan referensi dari materi praktikum sebelumnya
- Menggunakan dokumentasi resmi (Docker, Jenkins, Flask, dll)
- Menggunakan template yang disediakan

### ❌ Tidak Diperbolehkan:
- Meng-copy pekerjaan mahasiswa lain
- Menggunakan project yang sudah jadi dari internet
- Meminta bantuan dari pihak luar

### 📌 Ketentuan Khusus:
- Kode harus ditulis sendiri
- Pipeline harus dapat dijalankan dari awal
- Semua environment harus dapat diakses saat presentasi

---

## 📤 Submission

### Cara Submit:

1. **Push ke GitHub Repository**
   ```bash
   git add .
   git commit -m "UTS: Complete CI/CD Pipeline - [NIM] [Nama]"
   git push origin main
   ```

2. **Isi Google Form** (link akan diberikan saat ujian)
   - Link GitHub Repository
   - Link Jenkins (jika public)
   - Screenshot bukti

3. **Persiapkan Presentasi**
   - Demo pipeline berjalan
   - Penjelasan arsitektur
   - Siap menjawab pertanyaan

---

## 📅 Timeline

| Waktu | Aktivitas |
|-------|-----------|
| **Jam 1** | Setup environment, mulai coding aplikasi |
| **Jam 2** | Selesaikan aplikasi, buat tests, setup CI |
| **Jam 3** | Setup CD pipeline, dokumentasi |
| **+30 menit** | Persiapan presentasi |
| **Setelahnya** | Presentasi bergiliran |

---

## ❓ FAQ

<details>
<summary><strong>Q: Boleh menggunakan bahasa selain Python?</strong></summary>
A: Ya, boleh menggunakan Node.js atau Go. Namun pastikan Anda sudah familiar dengan testing framework-nya.
</details>

<details>
<summary><strong>Q: Bagaimana jika Jenkins tidak bisa diakses?</strong></summary>
A: Pastikan Anda sudah setup Jenkins sebelum hari H. Hubungi asisten jika ada masalah teknis.
</details>

<details>
<summary><strong>Q: Apakah harus ada database?</strong></summary>
A: Tidak wajib. Anda bisa menggunakan in-memory storage untuk menyederhanakan.
</details>

<details>
<summary><strong>Q: Berapa minimal test yang harus dibuat?</strong></summary>
A: Minimal 10 unit tests dengan coverage minimal 60%.
</details>

---

## 📚 Referensi yang Berguna

| Resource | Link |
|----------|------|
| Flask Documentation | [flask.palletsprojects.com](https://flask.palletsprojects.com/) |
| pytest Documentation | [docs.pytest.org](https://docs.pytest.org/) |
| Jenkins Pipeline | [jenkins.io/doc/book/pipeline](https://www.jenkins.io/doc/book/pipeline/) |
| Docker Compose | [docs.docker.com/compose](https://docs.docker.com/compose/) |
| Materi Pertemuan 01-07 | [Repository ini] |

---

<div align="center">

## 🍀 Good Luck!

*"The only way to do great work is to love what you do."* — Steve Jobs

---

**Percaya pada kemampuan Anda!**

Anda sudah mempelajari semua materi yang dibutuhkan dari Pertemuan 01-07.
Sekarang saatnya menunjukkan apa yang Anda bisa! 💪

</div>
