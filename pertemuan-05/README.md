# 🔧 Pertemuan 05: Introduction to CI/CD — Jenkins Setup & First Pipeline

<div align="center">

| 📅 Pertemuan | ⏱️ Durasi | 📊 Tingkat |
|:------------:|:---------:|:----------:|
| 05 | 2 x 50 menit | ⭐⭐⭐ Menengah |

</div>

---

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan praktikum ini, mahasiswa diharapkan mampu:

| No | Kemampuan yang Dicapai |
|:--:|------------------------|
| 1 | Memahami konsep **Continuous Integration (CI)** dan **Continuous Delivery (CD)** |
| 2 | Menginstall dan mengkonfigurasi **Jenkins** menggunakan Docker |
| 3 | Membuat **Jenkins Pipeline** pertama menggunakan Jenkinsfile |
| 4 | Menghubungkan Jenkins dengan **repository GitHub** |

---

## 📚 Materi Pembelajaran

### 1️⃣ Apa Itu CI/CD?

> *"If it hurts, do it more frequently, and bring the pain forward."* — Martin Fowler

**CI/CD** adalah praktik DevOps yang mengotomatisasi proses integrasi dan deployment software.

#### 🔄 Continuous Integration (CI)

**CI** adalah praktik menggabungkan kode dari semua developer ke shared repository **secara sering** (minimal sekali sehari), dengan setiap integrasi diverifikasi oleh **automated build dan tests**.

```
┌─────────────────────────────────────────────────────────────────┐
│                    CONTINUOUS INTEGRATION                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   Developer A ──┐                                               │
│                 │    ┌─────────┐    ┌──────────┐    ┌────────┐ │
│   Developer B ──┼──▶│  Push   │──▶│  Build   │──▶│  Test  │  │
│                 │    │ to Repo │    │ Automated│    │Automated│ │
│   Developer C ──┘    └─────────┘    └──────────┘    └────────┘ │
│                                                         │       │
│                                          ┌──────────────┴─────┐ │
│                                          │ ✅ Pass → Ready    │ │
│                                          │ ❌ Fail → Fix ASAP │ │
│                                          └────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

**Manfaat CI:**
| Manfaat | Penjelasan |
|---------|------------|
| 🐛 Deteksi bug lebih awal | Bug ditemukan saat integrasi, bukan di production |
| ⏱️ Feedback cepat | Developer tahu dalam hitungan menit jika ada masalah |
| 🔄 Integrasi lebih mudah | Merge kecil lebih mudah daripada merge besar |
| 📊 Kualitas kode terjaga | Test otomatis memastikan kode tetap berfungsi |

#### 📦 Continuous Delivery vs Continuous Deployment

```
┌─────────────────────────────────────────────────────────────────┐
│              CD: DELIVERY vs DEPLOYMENT                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  CONTINUOUS DELIVERY (CD)                                        │
│  ┌──────┐   ┌──────┐   ┌─────────┐   ┌─────────┐   ┌────────┐  │
│  │Build │ → │ Test │ → │ Staging │ → │ MANUAL  │ → │  Prod  │  │
│  └──────┘   └──────┘   └─────────┘   │ APPROVAL│   └────────┘  │
│                                       └─────────┘               │
│  "Selalu SIAP untuk deploy, tapi deploy manual"                 │
│                                                                  │
│  ─────────────────────────────────────────────────              │
│                                                                  │
│  CONTINUOUS DEPLOYMENT                                           │
│  ┌──────┐   ┌──────┐   ┌─────────┐   ┌────────┐                 │
│  │Build │ → │ Test │ → │ Staging │ → │  Prod  │                 │
│  └──────┘   └──────┘   └─────────┘   └────────┘                 │
│                         (otomatis)   (otomatis)                  │
│  "Setiap perubahan yang lulus test langsung ke production"      │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

### 2️⃣ Mengenal Jenkins

**Jenkins** adalah automation server open-source yang paling populer untuk membangun CI/CD pipeline.

#### 🏗️ Arsitektur Jenkins

```
┌─────────────────────────────────────────────────────────────────┐
│                     JENKINS ARCHITECTURE                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│                    ┌───────────────────────┐                    │
│                    │   JENKINS CONTROLLER   │                    │
│                    │   (Master Node)        │                    │
│                    ├───────────────────────┤                    │
│                    │ • Scheduling builds    │                    │
│                    │ • Dispatching to agents│                    │
│                    │ • Monitoring agents    │                    │
│                    │ • Recording results    │                    │
│                    │ • Plugin management    │                    │
│                    └───────────┬───────────┘                    │
│                                │                                 │
│              ┌─────────────────┼─────────────────┐              │
│              ▼                 ▼                 ▼              │
│       ┌───────────┐     ┌───────────┐     ┌───────────┐        │
│       │  Agent 1  │     │  Agent 2  │     │  Agent 3  │        │
│       │  (Linux)  │     │ (Windows) │     │  (Docker) │        │
│       └───────────┘     └───────────┘     └───────────┘        │
│           │                  │                  │                │
│           ▼                  ▼                  ▼                │
│      ┌─────────┐       ┌─────────┐       ┌─────────┐           │
│      │Build Job│       │Test Job │       │Deploy   │           │
│      └─────────┘       └─────────┘       │  Job    │           │
│                                          └─────────┘           │
└─────────────────────────────────────────────────────────────────┘
```

#### 📋 Komponen Jenkins

| Komponen | Fungsi |
|----------|--------|
| **Controller** | Node utama yang mengatur semua operasi |
| **Agent/Node** | Server yang menjalankan build jobs |
| **Pipeline** | Definisi proses CI/CD dalam kode |
| **Job/Project** | Unit kerja yang bisa di-schedule |
| **Build** | Satu eksekusi dari sebuah job |
| **Plugin** | Ekstensi untuk menambah fungsionalitas |

---

### 3️⃣ Jenkins Pipeline

**Pipeline** adalah cara modern untuk mendefinisikan CI/CD workflow dalam Jenkins menggunakan kode (Pipeline as Code).

#### 📝 Declarative Pipeline Syntax

```groovy
pipeline {
    // Di mana pipeline akan dijalankan
    agent any
    
    // Environment variables
    environment {
        APP_NAME = 'my-app'
        VERSION = '1.0.0'
    }
    
    // Tahapan pipeline
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying to staging...'
                sh 'docker build -t ${APP_NAME}:${VERSION} .'
            }
        }
    }
    
    // Post-actions
    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
        always {
            echo 'Cleaning up...'
            cleanWs()
        }
    }
}
```

#### 🔑 Komponen Jenkinsfile

| Blok | Fungsi |
|------|--------|
| `pipeline {}` | Container utama |
| `agent` | Menentukan di mana pipeline dijalankan |
| `environment` | Mendefinisikan environment variables |
| `stages` | Kumpulan tahapan |
| `stage('Name')` | Satu tahapan dalam pipeline |
| `steps` | Perintah yang dijalankan dalam stage |
| `post` | Actions setelah pipeline selesai |

---

### 4️⃣ Jenkins dengan Docker

Menjalankan Jenkins menggunakan Docker memudahkan setup dan maintenance.

```yaml
# docker-compose.yml untuk Jenkins
version: '3.8'

services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    user: root
    ports:
      - "8080:8080"     # Web UI
      - "50000:50000"   # Agent communication
    volumes:
      - jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock  # Docker in Docker
    environment:
      - JAVA_OPTS=-Djenkins.install.runSetupWizard=false
    restart: unless-stopped

volumes:
  jenkins_home:
```

---

## 🔧 Tugas Praktikum

### 📋 Prasyarat

Pastikan Anda sudah:
- ✅ Docker dan Docker Compose terinstall (Pertemuan 02)
- ✅ Memiliki repository GitHub (Pertemuan 03)
- ✅ Memahami konsep branching dan PR (Pertemuan 03-04)

---

### Task 1: Install Jenkins dengan Docker

**Tujuan:** Menjalankan Jenkins server menggunakan Docker

#### Langkah 1: Buat Direktori Proyek

```bash
mkdir -p ~/praktikum-jenkins
cd ~/praktikum-jenkins
```

#### Langkah 2: Buat Docker Compose File

Buat file `docker-compose.yml`:

```yaml
version: '3.8'

services:
  jenkins:
    image: jenkins/jenkins:lts-jdk17
    container_name: jenkins-praktikum
    user: root
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
    restart: unless-stopped

volumes:
  jenkins_home:
    name: jenkins_home_praktikum
```

#### Langkah 3: Jalankan Jenkins

```bash
# Jalankan Jenkins
docker compose up -d

# Tunggu Jenkins siap (sekitar 1-2 menit)
docker compose logs -f jenkins

# Dapatkan initial admin password
docker exec jenkins-praktikum cat /var/jenkins_home/secrets/initialAdminPassword
```

#### Langkah 4: Setup Jenkins

1. Buka browser: `http://localhost:8080`
2. Masukkan **initial admin password**
3. Pilih **Install suggested plugins**
4. Tunggu instalasi selesai
5. Buat **admin user**:
   - Username: `admin`
   - Password: `[password Anda]`
   - Full name: `[Nama Anda]`
   - Email: `[email Anda]`
6. Konfirmasi Jenkins URL: `http://localhost:8080/`
7. Klik **Start using Jenkins**

📸 **Screenshot yang diperlukan:**
- Halaman unlock Jenkins
- Plugin installation progress
- Jenkins dashboard setelah setup

---

### Task 2: Install Plugin Tambahan

**Tujuan:** Menambahkan plugin yang dibutuhkan untuk pipeline

Di Jenkins:
1. Pergi ke **Manage Jenkins** → **Plugins** → **Available plugins**
2. Install plugin berikut:
   - **Docker Pipeline** - Untuk integrasi Docker
   - **GitHub Integration** - Untuk webhook GitHub
   - **Pipeline Stage View** - Visualisasi pipeline
   - **Blue Ocean** - UI modern untuk pipeline (opsional)

3. Klik **Install** dan tunggu selesai
4. Restart Jenkins jika diminta

📸 **Screenshot yang diperlukan:**
- Installed plugins list

---

### Task 3: Membuat Pipeline Pertama

**Tujuan:** Membuat dan menjalankan Jenkins Pipeline sederhana

#### Langkah 1: Buat Repository dengan Jenkinsfile

Di GitHub, buat repository baru atau gunakan yang sudah ada, lalu tambahkan file `Jenkinsfile`:

```groovy
pipeline {
    agent any
    
    environment {
        STUDENT_NAME = 'Nama Anda'
        STUDENT_NIM = 'NIM Anda'
    }
    
    stages {
        stage('Hello') {
            steps {
                echo "=========================================="
                echo "  🎓 Praktikum CI/CD - Jenkins Pipeline"
                echo "=========================================="
                echo "Nama  : ${STUDENT_NAME}"
                echo "NIM   : ${STUDENT_NIM}"
                echo "Build : #${BUILD_NUMBER}"
                echo "=========================================="
            }
        }
        
        stage('Check Environment') {
            steps {
                echo '📋 Checking environment...'
                sh 'whoami'
                sh 'pwd'
                sh 'ls -la'
                sh 'docker --version || echo "Docker not available in this agent"'
            }
        }
        
        stage('Build') {
            steps {
                echo '🔨 Building application...'
                sh '''
                    echo "Simulating build process..."
                    sleep 2
                    echo "Build completed!"
                '''
            }
        }
        
        stage('Test') {
            steps {
                echo '🧪 Running tests...'
                sh '''
                    echo "Running unit tests..."
                    sleep 2
                    echo "Tests passed: 10/10"
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                echo '🚀 Deploying application...'
                sh '''
                    echo "Deploying to staging environment..."
                    sleep 2
                    echo "Deployment successful!"
                '''
            }
        }
    }
    
    post {
        success {
            echo '''
            ╔════════════════════════════════════════╗
            ║  ✅ PIPELINE COMPLETED SUCCESSFULLY!   ║
            ╚════════════════════════════════════════╝
            '''
        }
        failure {
            echo '''
            ╔════════════════════════════════════════╗
            ║  ❌ PIPELINE FAILED!                   ║
            ╚════════════════════════════════════════╝
            '''
        }
        always {
            echo "Pipeline finished at: ${new Date()}"
        }
    }
}
```

#### Langkah 2: Buat Pipeline Job di Jenkins

1. Di Jenkins Dashboard, klik **New Item**
2. Masukkan nama: `praktikum-pipeline`
3. Pilih **Pipeline**, klik OK
4. Di bagian **Pipeline**:
   - Definition: **Pipeline script from SCM**
   - SCM: **Git**
   - Repository URL: `https://github.com/[username]/[repo].git`
   - Branch: `*/main`
   - Script Path: `Jenkinsfile`
5. Klik **Save**

#### Langkah 3: Jalankan Pipeline

1. Klik **Build Now**
2. Lihat progress di **Build History**
3. Klik build number untuk melihat detail
4. Lihat **Console Output** untuk log lengkap

📸 **Screenshot yang diperlukan:**
- Konfigurasi pipeline job
- Pipeline stage view (visualisasi stages)
- Console output yang sukses

---

### Task 4: Pipeline dengan Docker Build

**Tujuan:** Membuat pipeline yang build Docker image

#### Langkah 1: Tambahkan Dockerfile ke Repository

```dockerfile
FROM nginx:alpine

LABEL maintainer="nama@email.com"
LABEL description="Praktikum Jenkins Pipeline"

COPY index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

#### Langkah 2: Buat file `index.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>Jenkins CI/CD Demo</title>
    <style>
        body { 
            font-family: Arial; 
            display: flex; 
            justify-content: center; 
            align-items: center; 
            height: 100vh;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            color: white;
            margin: 0;
        }
        .container { 
            text-align: center; 
            padding: 2rem;
            background: rgba(255,255,255,0.1);
            border-radius: 15px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🚀 Jenkins CI/CD Pipeline</h1>
        <p>Build Number: BUILD_NUMBER_PLACEHOLDER</p>
        <p>Praktikum DevOps - Pertemuan 05</p>
    </div>
</body>
</html>
```

#### Langkah 3: Update Jenkinsfile

```groovy
pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'praktikum-app'
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo '📥 Checking out source code...'
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh '''
                    # Replace placeholder with actual build number
                    sed -i "s/BUILD_NUMBER_PLACEHOLDER/${BUILD_NUMBER}/g" index.html
                    
                    # Build image
                    docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                    docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest
                '''
            }
        }
        
        stage('Test Container') {
            steps {
                echo '🧪 Testing container...'
                sh '''
                    # Run container
                    docker run -d --name test-${BUILD_NUMBER} -p 8888:80 ${DOCKER_IMAGE}:${DOCKER_TAG}
                    
                    # Wait for container to start
                    sleep 3
                    
                    # Test if container is responding
                    curl -f http://localhost:8888 || exit 1
                    
                    echo "✅ Container test passed!"
                '''
            }
            post {
                always {
                    sh '''
                        docker stop test-${BUILD_NUMBER} || true
                        docker rm test-${BUILD_NUMBER} || true
                    '''
                }
            }
        }
        
        stage('Deploy') {
            steps {
                echo '🚀 Deploying application...'
                sh '''
                    # Stop existing container if running
                    docker stop praktikum-running || true
                    docker rm praktikum-running || true
                    
                    # Run new container
                    docker run -d --name praktikum-running -p 9090:80 ${DOCKER_IMAGE}:latest
                    
                    echo "✅ Application deployed at http://localhost:9090"
                '''
            }
        }
    }
    
    post {
        success {
            echo '🎉 Pipeline completed successfully!'
            echo "Application available at: http://localhost:9090"
        }
        failure {
            echo '❌ Pipeline failed. Check the logs for details.'
        }
        always {
            echo "Pipeline finished. Build number: ${BUILD_NUMBER}"
        }
    }
}
```

#### Langkah 4: Jalankan Pipeline

1. Commit dan push semua perubahan ke GitHub
2. Di Jenkins, klik **Build Now**
3. Setelah sukses, akses `http://localhost:9090`

📸 **Screenshot yang diperlukan:**
- Pipeline berhasil dengan Docker stages
- Aplikasi berjalan di browser (port 9090)
- Output `docker images` menampilkan image yang dibuat

---

## 📤 Format Submission

```
📁 NIM_Nama_Pertemuan05/
│
├── 📄 README.md                      # Laporan praktikum
│
├── 📁 jenkins-setup/
│   ├── 📄 docker-compose.yml
│   └── 📄 setup-notes.md             # Catatan proses setup
│
├── 📁 pipeline-project/
│   ├── 📄 Jenkinsfile
│   ├── 📄 Dockerfile
│   ├── 📄 index.html
│   └── 📄 repository-link.txt
│
└── 📁 screenshots/
    ├── 🖼️ 01-jenkins-unlock.png
    ├── 🖼️ 02-plugin-install.png
    ├── 🖼️ 03-jenkins-dashboard.png
    ├── 🖼️ 04-pipeline-config.png
    ├── 🖼️ 05-pipeline-stages.png
    ├── 🖼️ 06-console-output.png
    ├── 🖼️ 07-docker-build.png
    └── 🖼️ 08-app-running.png
```

---

## ✅ Checklist Sebelum Submit

- [ ] Jenkins berhasil dijalankan dengan Docker
- [ ] Semua plugin yang diminta terinstall
- [ ] Pipeline pertama berhasil dijalankan
- [ ] Pipeline dengan Docker build berhasil
- [ ] Aplikasi dapat diakses di browser
- [ ] Semua screenshot lengkap dan jelas
- [ ] Kode Jenkinsfile terdokumentasi dengan baik

---

## 🔧 Troubleshooting

| Masalah | Solusi |
|---------|--------|
| Jenkins tidak bisa akses Docker | Pastikan volume `/var/run/docker.sock` ter-mount |
| Port 8080 sudah digunakan | Ganti port di docker-compose: `"8081:8080"` |
| Pipeline gagal di stage Docker | Install Docker CLI di Jenkins atau gunakan Docker agent |
| GitHub tidak bisa diakses | Pastikan repository public atau tambahkan credentials |

---

## 📚 Referensi

| Sumber | Link |
|--------|------|
| Jenkins Documentation | [jenkins.io/doc](https://www.jenkins.io/doc/) |
| Pipeline Syntax | [jenkins.io/doc/book/pipeline/syntax](https://www.jenkins.io/doc/book/pipeline/syntax/) |
| Jenkins Docker Hub | [hub.docker.com/r/jenkins/jenkins](https://hub.docker.com/r/jenkins/jenkins) |
| Blue Ocean | [jenkins.io/doc/book/blueocean](https://www.jenkins.io/doc/book/blueocean/) |

---

## ⏰ Deadline

<div align="center">

| 📅 Batas Pengumpulan |
|:--------------------:|
| **Sebelum Pertemuan 06** |

</div>

---

<div align="center">

**Happy Building! 🔨**

*"Automation is the key to efficiency in software delivery."*

</div>
