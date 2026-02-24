# 🐳 Pertemuan 02: Docker Fundamentals — Kontainerisasi Aplikasi

<div align="center">

| 📅 Pertemuan | ⏱️ Durasi | 📊 Tingkat |
|:------------:|:---------:|:----------:|
| 02 | 2 x 50 menit | ⭐⭐ Dasar-Menengah |

</div>

---

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan praktikum ini, mahasiswa diharapkan mampu:

| No | Kemampuan yang Dicapai |
|:--:|------------------------|
| 1 | Memahami **konsep kontainerisasi** dan perbedaannya dengan virtualisasi |
| 2 | Menjalankan dan mengelola **Docker containers** |
| 3 | Membuat **Docker image** menggunakan Dockerfile |
| 4 | Menggunakan **Docker Compose** untuk aplikasi multi-container |

---

## 📚 Materi Pembelajaran

### 1️⃣ Mengapa Docker Penting dalam DevOps?

> *"Works on my machine"* — masalah klasik yang diselesaikan Docker.

Docker memastikan aplikasi berjalan **konsisten** di semua environment: development, staging, dan production.

#### 🔄 Container vs Virtual Machine

```
┌─────────────────────────────────────────────────────────────────┐
│              VIRTUAL MACHINE vs CONTAINER                        │
├────────────────────────────┬────────────────────────────────────┤
│      Virtual Machine       │           Container                │
├────────────────────────────┼────────────────────────────────────┤
│  ┌─────┐ ┌─────┐ ┌─────┐  │   ┌─────┐ ┌─────┐ ┌─────┐        │
│  │App A│ │App B│ │App C│  │   │App A│ │App B│ │App C│        │
│  ├─────┤ ├─────┤ ├─────┤  │   ├─────┴─┴─────┴─┴─────┤        │
│  │Bins │ │Bins │ │Bins │  │   │   Container Runtime   │        │
│  ├─────┤ ├─────┤ ├─────┤  │   │      (Docker)         │        │
│  │Guest│ │Guest│ │Guest│  │   ├───────────────────────┤        │
│  │ OS  │ │ OS  │ │ OS  │  │   │      Host OS          │        │
│  ├─────┴─┴─────┴─┴─────┤  │   ├───────────────────────┤        │
│  │     Hypervisor      │  │   │     Infrastructure    │        │
│  ├─────────────────────┤  │   └───────────────────────┘        │
│  │      Host OS        │  │                                     │
│  ├─────────────────────┤  │   ✅ Ringan (MB vs GB)              │
│  │   Infrastructure    │  │   ✅ Start dalam detik              │
│  └─────────────────────┘  │   ✅ Berbagi kernel host            │
│                            │   ✅ Portabel antar environment    │
│  ❌ Berat (GB)             │                                     │
│  ❌ Start dalam menit      │                                     │
└────────────────────────────┴────────────────────────────────────┘
```

---

### 2️⃣ Arsitektur Docker

```
┌─────────────────────────────────────────────────────────────────┐
│                      DOCKER ARCHITECTURE                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   ┌──────────────┐         ┌──────────────────────────────┐    │
│   │ Docker CLI   │ ──────▶ │      Docker Daemon           │    │
│   │ (docker ...) │         │      (dockerd)               │    │
│   └──────────────┘         └──────────────────────────────┘    │
│                                       │                         │
│                    ┌──────────────────┼──────────────────┐     │
│                    ▼                  ▼                  ▼     │
│            ┌────────────┐    ┌────────────┐    ┌────────────┐ │
│            │  Images    │    │ Containers │    │  Networks  │ │
│            └────────────┘    └────────────┘    └────────────┘ │
│                    │                                           │
│                    ▼                                           │
│            ┌────────────────────────────────┐                  │
│            │       Docker Registry          │                  │
│            │   (Docker Hub, Private Reg)    │                  │
│            └────────────────────────────────┘                  │
└─────────────────────────────────────────────────────────────────┘
```

#### Komponen Utama:

| Komponen | Fungsi |
|----------|--------|
| **Docker Daemon** | Service yang menjalankan container |
| **Docker CLI** | Command line untuk berinteraksi dengan Docker |
| **Docker Image** | Template read-only untuk membuat container |
| **Docker Container** | Instance yang berjalan dari sebuah image |
| **Docker Registry** | Tempat menyimpan dan berbagi images |

---

### 3️⃣ Perintah Docker Dasar

#### 🖼️ Manajemen Image

```bash
# Menarik image dari Docker Hub
docker pull nginx:latest

# Melihat daftar image yang ada
docker images

# Menghapus image
docker rmi nginx:latest

# Mencari image di Docker Hub
docker search python
```

#### 📦 Manajemen Container

```bash
# Menjalankan container
docker run nginx

# Menjalankan container di background (-d = detached)
docker run -d --name webserver nginx

# Menjalankan container dengan port mapping
docker run -d -p 8080:80 --name webserver nginx

# Melihat container yang berjalan
docker ps

# Melihat semua container (termasuk yang stopped)
docker ps -a

# Menghentikan container
docker stop webserver

# Menjalankan kembali container
docker start webserver

# Menghapus container
docker rm webserver

# Melihat log container
docker logs webserver

# Masuk ke dalam container
docker exec -it webserver bash
```

#### 🔑 Penjelasan Flags Penting

| Flag | Kepanjangan | Fungsi |
|------|-------------|--------|
| `-d` | `--detach` | Jalankan container di background |
| `-p` | `--publish` | Mapping port (host:container) |
| `-v` | `--volume` | Mounting volume/direktori |
| `-e` | `--env` | Set environment variable |
| `-it` | `--interactive --tty` | Mode interaktif dengan terminal |
| `--name` | - | Memberi nama container |
| `--rm` | - | Hapus container otomatis setelah stop |

---

### 4️⃣ Dockerfile — Membuat Image Sendiri

**Dockerfile** adalah blueprint untuk membuat Docker image.

#### Contoh Dockerfile untuk Aplikasi Python:

```dockerfile
# Base image
FROM python:3.11-slim

# Metadata
LABEL maintainer="nama@email.com"
LABEL description="Aplikasi Flask sederhana"

# Set working directory
WORKDIR /app

# Copy requirements dan install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy source code
COPY . .

# Expose port
EXPOSE 5000

# Command untuk menjalankan aplikasi
CMD ["python", "app.py"]
```

#### Instruksi Dockerfile yang Umum:

| Instruksi | Fungsi | Contoh |
|-----------|--------|--------|
| `FROM` | Base image | `FROM python:3.11` |
| `WORKDIR` | Set direktori kerja | `WORKDIR /app` |
| `COPY` | Copy file dari host | `COPY . .` |
| `RUN` | Jalankan command saat build | `RUN pip install flask` |
| `ENV` | Set environment variable | `ENV PORT=5000` |
| `EXPOSE` | Dokumentasi port | `EXPOSE 5000` |
| `CMD` | Command default saat run | `CMD ["python", "app.py"]` |
| `ENTRYPOINT` | Executable utama | `ENTRYPOINT ["python"]` |

#### Build dan Run Image:

```bash
# Build image dari Dockerfile
docker build -t myapp:v1 .

# Jalankan container dari image yang dibuat
docker run -d -p 5000:5000 myapp:v1
```

---

### 5️⃣ Docker Compose — Aplikasi Multi-Container

**Docker Compose** memudahkan menjalankan aplikasi yang terdiri dari beberapa container.

#### Contoh `docker-compose.yml`:

```yaml
version: '3.8'

services:
  # Service web application
  web:
    build: .
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=postgresql://postgres:password@db:5432/myapp
    depends_on:
      - db
    volumes:
      - .:/app
    restart: unless-stopped

  # Service database
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: myapp
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

# Named volumes
volumes:
  postgres_data:
```

#### Perintah Docker Compose:

```bash
# Menjalankan semua services
docker compose up

# Menjalankan di background
docker compose up -d

# Melihat status services
docker compose ps

# Melihat logs
docker compose logs -f

# Menghentikan semua services
docker compose down

# Menghentikan dan hapus volumes
docker compose down -v

# Build ulang images
docker compose build
```

---

## 🔧 Tugas Praktikum

### Task 1: Eksplorasi Docker Dasar

**Tujuan:** Memahami lifecycle container Docker

```bash
# 1. Pull image nginx
docker pull nginx:alpine

# 2. Jalankan container dengan nama
docker run -d --name web-praktikum -p 8080:80 nginx:alpine

# 3. Akses di browser: http://localhost:8080

# 4. Lihat logs container
docker logs web-praktikum

# 5. Masuk ke dalam container
docker exec -it web-praktikum sh

# Di dalam container, coba:
ls -la /usr/share/nginx/html
cat /etc/nginx/nginx.conf
exit

# 6. Stop dan hapus container
docker stop web-praktikum
docker rm web-praktikum
```

📸 **Screenshot yang diperlukan:**
- Output `docker ps` saat container berjalan
- Tampilan browser mengakses nginx
- Output dari `docker logs`

---

### Task 2: Membuat Docker Image

**Tujuan:** Membuat custom Docker image menggunakan Dockerfile

#### Langkah 1: Buat Struktur Proyek

```bash
mkdir -p ~/praktikum-docker/app
cd ~/praktikum-docker
```

#### Langkah 2: Buat Aplikasi Sederhana

**File: `app/index.html`**
```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <title>Praktikum Docker - DevOps</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        .container {
            text-align: center;
            padding: 2rem;
            background: rgba(255,255,255,0.1);
            border-radius: 10px;
        }
        h1 { margin-bottom: 0.5rem; }
        p { opacity: 0.8; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🐳 Hello Docker!</h1>
        <p>Praktikum DevOps - Pertemuan 02</p>
        <p>NIM: [ISI_NIM_ANDA]</p>
        <p>Nama: [ISI_NAMA_ANDA]</p>
    </div>
</body>
</html>
```

#### Langkah 3: Buat Dockerfile

**File: `Dockerfile`**
```dockerfile
# Gunakan nginx alpine sebagai base image
FROM nginx:alpine

# Copy file HTML ke direktori nginx
COPY app/index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

# Command default nginx
CMD ["nginx", "-g", "daemon off;"]
```

#### Langkah 4: Build dan Run

```bash
# Build image
docker build -t praktikum-docker:v1 .

# Jalankan container
docker run -d -p 8080:80 --name praktikum-web praktikum-docker:v1

# Akses di browser: http://localhost:8080
```

📸 **Screenshot yang diperlukan:**
- Output `docker build`
- Output `docker images` menampilkan image yang dibuat
- Tampilan browser hasil deployment

---

### Task 3: Docker Compose Multi-Container

**Tujuan:** Menjalankan aplikasi dengan database menggunakan Docker Compose

#### Buat file `docker-compose.yml`:

```yaml
version: '3.8'

services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./app:/usr/share/nginx/html:ro
    depends_on:
      - api
    networks:
      - praktikum-net

  api:
    image: httpd:alpine
    ports:
      - "8081:80"
    networks:
      - praktikum-net

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: praktikum
      POSTGRES_PASSWORD: devops123
      POSTGRES_DB: praktikum_db
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - praktikum-net

networks:
  praktikum-net:
    driver: bridge

volumes:
  db_data:
```

#### Jalankan dengan Docker Compose:

```bash
# Jalankan semua services
docker compose up -d

# Cek status
docker compose ps

# Lihat logs
docker compose logs -f

# Akses web: http://localhost:8080
# Akses api: http://localhost:8081

# Cleanup
docker compose down -v
```

📸 **Screenshot yang diperlukan:**
- Output `docker compose ps`
- Output `docker compose logs`
- Browser mengakses kedua services

---

## 📤 Format Submission

```
📁 NIM_Nama_Pertemuan02/
│
├── 📄 README.md                    # Laporan praktikum
│
├── 📁 task1-docker-basic/
│   └── 📄 commands.md              # Daftar commands yang dijalankan
│
├── 📁 task2-dockerfile/
│   ├── 📄 Dockerfile
│   └── 📁 app/
│       └── 📄 index.html
│
├── 📁 task3-compose/
│   ├── 📄 docker-compose.yml
│   └── 📁 app/
│       └── 📄 index.html
│
└── 📁 screenshots/
    ├── 🖼️ 01-container-running.png
    ├── 🖼️ 02-nginx-browser.png
    ├── 🖼️ 03-docker-build.png
    ├── 🖼️ 04-custom-image.png
    ├── 🖼️ 05-compose-ps.png
    └── 🖼️ 06-compose-services.png
```

---

## ✅ Checklist Sebelum Submit

- [ ] Memahami perbedaan container vs virtual machine
- [ ] Dapat menjalankan dan mengelola container Docker
- [ ] Berhasil membuat custom Docker image dengan Dockerfile
- [ ] Berhasil menjalankan multi-container dengan Docker Compose
- [ ] Semua screenshot lengkap dan jelas
- [ ] Laporan ditulis dengan rapi

---

## 💡 Tips & Troubleshooting

| Masalah | Solusi |
|---------|--------|
| Port sudah digunakan | Ganti port host: `-p 8081:80` |
| Permission denied | Linux: tambah user ke group docker: `sudo usermod -aG docker $USER` |
| Image tidak ditemukan | Pastikan nama image benar, gunakan `docker search` |
| Container langsung exit | Cek logs: `docker logs [container_name]` |

---

## 📚 Referensi

| Sumber | Link |
|--------|------|
| Docker Documentation | [docs.docker.com](https://docs.docker.com/) |
| Dockerfile Best Practices | [docs.docker.com/develop/develop-images/dockerfile_best-practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) |
| Docker Compose Docs | [docs.docker.com/compose](https://docs.docker.com/compose/) |
| Play with Docker | [labs.play-with-docker.com](https://labs.play-with-docker.com/) |

---

## ⏰ Deadline

<div align="center">

| 📅 Batas Pengumpulan |
|:--------------------:|
| **Sebelum Pertemuan 03** |

</div>

---

<div align="center">

**Happy Containerizing! 🐳**

*"Containers are the future of software deployment."*

</div>
