# 📦 Pertemuan 07: Continuous Delivery Pipeline — Deploy ke Multiple Environments

<div align="center">

| 📅 Pertemuan | ⏱️ Durasi | 📊 Tingkat |
|:------------:|:---------:|:----------:|
| 07 | 2 x 50 menit | ⭐⭐⭐⭐ Menengah-Lanjut |

</div>

---

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan praktikum ini, mahasiswa diharapkan mampu:

| No | Kemampuan yang Dicapai |
|:--:|------------------------|
| 1 | Membangun **CD pipeline** lengkap dengan multiple environments |
| 2 | Mengimplementasikan **automated deployment** ke staging |
| 3 | Mengkonfigurasi **approval gates** untuk production deployment |
| 4 | Memahami **health checks** dan **rollback mechanism** |

---

## 📚 Materi Pembelajaran

### 1️⃣ Continuous Delivery vs Continuous Deployment

> *"Continuous Delivery is about always being ready to deploy. Continuous Deployment is about actually deploying continuously."*

```
┌─────────────────────────────────────────────────────────────────┐
│                  CD: DELIVERY vs DEPLOYMENT                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  🔄 CONTINUOUS DELIVERY                                          │
│  ┌──────┐  ┌──────┐  ┌─────────┐  ┌─────────┐  ┌──────────┐    │
│  │Build │→│ Test │→│ Staging │→│ Approval │→│Production│     │
│  └──────┘  └──────┘  └─────────┘  │ (Manual) │  └──────────┘    │
│                                    └─────────┘                   │
│                                         👆                       │
│                                   Human Decision                 │
│                                                                  │
│  ─────────────────────────────────────────────────────────      │
│                                                                  │
│  🚀 CONTINUOUS DEPLOYMENT                                        │
│  ┌──────┐  ┌──────┐  ┌─────────┐  ┌──────────┐                  │
│  │Build │→│ Test │→│ Staging │→│Production│                    │
│  └──────┘  └──────┘  └─────────┘  └──────────┘                  │
│                           │              │                       │
│                      Automated      Automated                    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

### 2️⃣ Deployment Environments

Environment adalah **lingkungan terpisah** tempat aplikasi berjalan dalam berbagai tahap development.

```
┌─────────────────────────────────────────────────────────────────┐
│                    DEPLOYMENT ENVIRONMENTS                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   ┌───────────────┐    ┌───────────────┐    ┌───────────────┐  │
│   │  DEVELOPMENT  │    │    STAGING    │    │  PRODUCTION   │  │
│   │    (Dev)      │ → │    (Stage)    │ → │    (Prod)     │  │
│   └───────────────┘    └───────────────┘    └───────────────┘  │
│                                                                  │
│   🎯 Tujuan:           🎯 Tujuan:           🎯 Tujuan:          │
│   Development &        Pre-production       Live users          │
│   Testing              Testing                                   │
│                                                                  │
│   👥 User:             👥 User:             👥 User:             │
│   Developers           QA Team, Testers    End Users            │
│                                                                  │
│   📊 Data:             📊 Data:             📊 Data:             │
│   Mock/Test Data       Production-like     Real Data            │
│                                                                  │
│   🔄 Deploy:           🔄 Deploy:           🔄 Deploy:           │
│   Very Frequent        On PR Merge         After Approval       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

| Environment | Tujuan | Siapa yang Akses | Frekuensi Deploy |
|-------------|--------|------------------|------------------|
| **Development** | Testing oleh developer | Developers | Setiap commit |
| **Staging** | Pre-production testing | QA, Product Team | Setiap merge ke develop |
| **Production** | Aplikasi live | End users | Setelah approval |

---

### 3️⃣ Deployment Strategies

#### 🔄 Rolling Deployment

Mengganti instance satu per satu tanpa downtime.

```
Time 0:  [v1] [v1] [v1] [v1]
Time 1:  [v2] [v1] [v1] [v1]  ← 1 instance updated
Time 2:  [v2] [v2] [v1] [v1]  ← 2 instances updated
Time 3:  [v2] [v2] [v2] [v1]  ← 3 instances updated
Time 4:  [v2] [v2] [v2] [v2]  ← All updated ✅
```

#### 🔵🟢 Blue-Green Deployment

Dua environment identik, switch traffic saat deploy.

```
┌─────────────────────────────────────────────────────────────────┐
│                    BLUE-GREEN DEPLOYMENT                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   BEFORE:                           AFTER:                       │
│                                                                  │
│   Users ──▶ [BLUE v1] ✅            Users ──────────▶ [GREEN v2]│
│             [GREEN v1] 💤                  [BLUE v1] 💤         │
│                                                                  │
│   1. Deploy v2 to GREEN (inactive)                              │
│   2. Test v2 on GREEN                                           │
│   3. Switch traffic to GREEN                                     │
│   4. BLUE becomes standby (rollback option)                     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

#### 🐤 Canary Deployment

Release ke sebagian kecil users dulu.

```
Time 0:  100% users → [v1]
Time 1:   90% users → [v1]  |  10% users → [v2]  ← Canary
Time 2:   70% users → [v1]  |  30% users → [v2]  ← Expanding
Time 3:   50% users → [v1]  |  50% users → [v2]
Time 4:  100% users → [v2]  ← Full rollout ✅
```

| Strategy | Downtime | Rollback | Risk | Use Case |
|----------|:--------:|:--------:|:----:|----------|
| **Rolling** | No | Medium | Low | General purpose |
| **Blue-Green** | No | Fast | Low | Critical apps |
| **Canary** | No | Fast | Very Low | High-traffic apps |

---

### 4️⃣ Health Checks

**Health check** adalah endpoint untuk memverifikasi bahwa aplikasi berjalan dengan benar.

```
┌─────────────────────────────────────────────────────────────────┐
│                       HEALTH CHECK TYPES                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   🏥 LIVENESS CHECK                                              │
│   └── "Apakah aplikasi hidup?"                                  │
│       Endpoint: GET /health atau /healthz                        │
│       Response: 200 OK                                           │
│                                                                  │
│   📊 READINESS CHECK                                             │
│   └── "Apakah aplikasi siap menerima traffic?"                  │
│       Endpoint: GET /ready                                       │
│       Checks: Database connection, dependencies                  │
│                                                                  │
│   🔬 DEEP HEALTH CHECK                                           │
│   └── "Apakah semua komponen berfungsi?"                        │
│       Endpoint: GET /health/detailed                             │
│       Response: Status setiap dependency                         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Contoh Health Check Endpoint:**
```python
# Flask health check example
@app.route('/health')
def health():
    return jsonify({
        "status": "healthy",
        "timestamp": datetime.now().isoformat()
    }), 200

@app.route('/health/detailed')
def health_detailed():
    checks = {
        "database": check_database(),
        "redis": check_redis(),
        "external_api": check_external_api()
    }
    
    all_healthy = all(checks.values())
    status_code = 200 if all_healthy else 503
    
    return jsonify({
        "status": "healthy" if all_healthy else "unhealthy",
        "checks": checks,
        "timestamp": datetime.now().isoformat()
    }), status_code
```

---

### 5️⃣ Jenkins Pipeline dengan CD

```groovy
pipeline {
    agent any
    
    environment {
        DOCKER_REGISTRY = 'your-registry'
        APP_NAME = 'myapp'
        VERSION = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Build') {
            steps {
                sh "docker build -t ${DOCKER_REGISTRY}/${APP_NAME}:${VERSION} ."
            }
        }
        
        stage('Test') {
            steps {
                sh "docker run --rm ${DOCKER_REGISTRY}/${APP_NAME}:${VERSION} npm test"
            }
        }
        
        stage('Push to Registry') {
            steps {
                sh "docker push ${DOCKER_REGISTRY}/${APP_NAME}:${VERSION}"
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                sh "docker-compose -f docker-compose.staging.yml up -d"
            }
            post {
                success {
                    // Health check
                    sh "curl -f http://staging:8080/health || exit 1"
                }
            }
        }
        
        stage('Staging Tests') {
            steps {
                // Run smoke tests on staging
                sh "npm run test:e2e -- --env=staging"
            }
        }
        
        stage('Approval for Production') {
            steps {
                input message: 'Deploy to Production?', 
                      ok: 'Deploy',
                      submitter: 'admin,devops-team'
            }
        }
        
        stage('Deploy to Production') {
            steps {
                sh "docker-compose -f docker-compose.prod.yml up -d"
            }
            post {
                success {
                    sh "curl -f http://production:8080/health || exit 1"
                    echo "✅ Production deployment successful!"
                }
                failure {
                    // Rollback
                    sh "docker-compose -f docker-compose.prod.yml down"
                    sh "docker-compose -f docker-compose.prod.yml up -d --rollback"
                    error "Deployment failed! Rolled back to previous version."
                }
            }
        }
    }
}
```

---

## 🔧 Tugas Praktikum

### 📋 Prasyarat

Pastikan Anda sudah:
- ✅ Jenkins berjalan dengan Docker (Pertemuan 05)
- ✅ Memahami automated testing (Pertemuan 06)
- ✅ Docker dan Docker Compose terinstall

---

### Task 1: Membuat Aplikasi dengan Health Check

**Tujuan:** Membuat aplikasi web sederhana dengan health check endpoint

#### Langkah 1: Buat Struktur Proyek

```bash
mkdir -p ~/praktikum-cd
cd ~/praktikum-cd
mkdir -p app templates
```

#### Langkah 2: Buat Aplikasi Flask

**File: `app/main.py`**
```python
"""
Simple Flask application with health check endpoints.
"""
from flask import Flask, jsonify, render_template
from datetime import datetime
import os

app = Flask(__name__, template_folder='../templates')

# Configuration from environment
APP_VERSION = os.getenv('APP_VERSION', '1.0.0')
ENVIRONMENT = os.getenv('ENVIRONMENT', 'development')

@app.route('/')
def home():
    """Home page."""
    return render_template('index.html', 
                          version=APP_VERSION, 
                          environment=ENVIRONMENT)

@app.route('/api/info')
def info():
    """Application info endpoint."""
    return jsonify({
        "app": "Praktikum CD Pipeline",
        "version": APP_VERSION,
        "environment": ENVIRONMENT,
        "timestamp": datetime.now().isoformat()
    })

@app.route('/health')
def health():
    """Basic health check - liveness probe."""
    return jsonify({
        "status": "healthy",
        "version": APP_VERSION
    }), 200

@app.route('/ready')
def ready():
    """Readiness check - is app ready to serve traffic."""
    # In real app, check database, cache, etc.
    checks = {
        "app": True,
        "config_loaded": bool(APP_VERSION)
    }
    
    all_ready = all(checks.values())
    
    return jsonify({
        "status": "ready" if all_ready else "not_ready",
        "checks": checks
    }), 200 if all_ready else 503

@app.route('/health/detailed')
def health_detailed():
    """Detailed health check with all dependencies."""
    checks = {
        "application": {
            "status": "healthy",
            "version": APP_VERSION
        },
        "environment": {
            "status": "configured",
            "name": ENVIRONMENT
        },
        "timestamp": datetime.now().isoformat()
    }
    
    return jsonify(checks), 200

if __name__ == '__main__':
    port = int(os.getenv('PORT', 5000))
    debug = ENVIRONMENT == 'development'
    app.run(host='0.0.0.0', port=port, debug=debug)
```

**File: `templates/index.html`**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CD Pipeline Demo</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            color: white;
        }
        .container {
            text-align: center;
            padding: 3rem;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            backdrop-filter: blur(10px);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            max-width: 500px;
        }
        h1 { 
            font-size: 2.5rem; 
            margin-bottom: 1rem;
            background: linear-gradient(90deg, #00d9ff, #00ff88);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .badge {
            display: inline-block;
            padding: 0.5rem 1rem;
            margin: 0.5rem;
            border-radius: 20px;
            font-size: 0.9rem;
        }
        .version { background: #00d9ff; color: #1a1a2e; }
        .environment { background: #00ff88; color: #1a1a2e; }
        .staging { background: #ffaa00; }
        .production { background: #ff4444; }
        p { margin: 1rem 0; opacity: 0.8; }
        .endpoints {
            margin-top: 2rem;
            text-align: left;
            background: rgba(0,0,0,0.2);
            padding: 1rem;
            border-radius: 10px;
        }
        .endpoints h3 { margin-bottom: 0.5rem; font-size: 1rem; }
        .endpoints ul { list-style: none; }
        .endpoints li { 
            padding: 0.3rem 0; 
            font-family: monospace;
            font-size: 0.85rem;
        }
        .endpoints a { color: #00d9ff; text-decoration: none; }
        .endpoints a:hover { text-decoration: underline; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🚀 CD Pipeline Demo</h1>
        <p>Praktikum DevOps - Pertemuan 07</p>
        
        <div>
            <span class="badge version">Version: {{ version }}</span>
            <span class="badge environment {{ environment }}">{{ environment | upper }}</span>
        </div>
        
        <div class="endpoints">
            <h3>📍 Available Endpoints:</h3>
            <ul>
                <li><a href="/api/info">/api/info</a> - Application info</li>
                <li><a href="/health">/health</a> - Health check</li>
                <li><a href="/ready">/ready</a> - Readiness check</li>
                <li><a href="/health/detailed">/health/detailed</a> - Detailed health</li>
            </ul>
        </div>
    </div>
</body>
</html>
```

**File: `requirements.txt`**
```
flask==3.0.0
gunicorn==21.2.0
```

📸 **Screenshot yang diperlukan:**
- Aplikasi berjalan di browser
- Response dari `/health` endpoint

---

### Task 2: Setup Docker untuk Multiple Environments

**Tujuan:** Membuat konfigurasi Docker untuk staging dan production

#### Langkah 1: Buat Dockerfile

**File: `Dockerfile`**
```dockerfile
# Build stage
FROM python:3.11-slim as builder

WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Production stage
FROM python:3.11-slim

WORKDIR /app

# Copy from builder
COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages

# Copy application
COPY app/ ./app/
COPY templates/ ./templates/

# Create non-root user
RUN useradd -m appuser && chown -R appuser:appuser /app
USER appuser

# Environment variables (can be overridden)
ENV PORT=5000
ENV ENVIRONMENT=development
ENV APP_VERSION=1.0.0

EXPOSE ${PORT}

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:${PORT}/health || exit 1

# Run with gunicorn for production
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "2", "app.main:app"]
```

#### Langkah 2: Buat Docker Compose untuk Staging

**File: `docker-compose.staging.yml`**
```yaml
version: '3.8'

services:
  app-staging:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: cd-demo-staging
    ports:
      - "8081:5000"
    environment:
      - ENVIRONMENT=staging
      - APP_VERSION=${BUILD_NUMBER:-1.0.0}
      - PORT=5000
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:5000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 10s
    restart: unless-stopped
    networks:
      - cd-network

networks:
  cd-network:
    driver: bridge
```

#### Langkah 3: Buat Docker Compose untuk Production

**File: `docker-compose.prod.yml`**
```yaml
version: '3.8'

services:
  app-production:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: cd-demo-production
    ports:
      - "8080:5000"
    environment:
      - ENVIRONMENT=production
      - APP_VERSION=${BUILD_NUMBER:-1.0.0}
      - PORT=5000
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:5000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 10s
    restart: always
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 256M
    networks:
      - cd-network

networks:
  cd-network:
    driver: bridge
```

#### Langkah 4: Test Lokal

```bash
# Test staging
docker compose -f docker-compose.staging.yml up -d
curl http://localhost:8081/health
curl http://localhost:8081/api/info

# Stop staging
docker compose -f docker-compose.staging.yml down

# Test production
docker compose -f docker-compose.prod.yml up -d
curl http://localhost:8080/health
curl http://localhost:8080/api/info

# Stop production
docker compose -f docker-compose.prod.yml down
```

📸 **Screenshot yang diperlukan:**
- Output `docker compose ps` untuk staging
- Output `docker compose ps` untuk production
- `/api/info` menunjukkan environment yang berbeda

---

### Task 3: Buat Complete CD Pipeline

**Tujuan:** Membuat Jenkinsfile dengan CD pipeline lengkap

**File: `Jenkinsfile`**
```groovy
pipeline {
    agent any
    
    environment {
        APP_NAME = 'cd-demo'
        STAGING_PORT = '8081'
        PROD_PORT = '8080'
    }
    
    stages {
        stage('🔍 Checkout') {
            steps {
                echo '📥 Checking out source code...'
                checkout scm
                sh 'ls -la'
            }
        }
        
        stage('🔨 Build') {
            steps {
                echo '🐳 Building Docker image...'
                sh '''
                    docker build -t ${APP_NAME}:${BUILD_NUMBER} .
                    docker tag ${APP_NAME}:${BUILD_NUMBER} ${APP_NAME}:latest
                '''
            }
        }
        
        stage('🧪 Unit Tests') {
            steps {
                echo '🧪 Running unit tests...'
                sh '''
                    docker run --rm ${APP_NAME}:${BUILD_NUMBER} \
                        python -c "from app.main import app; print('✅ App imports successfully')"
                '''
            }
        }
        
        stage('🚀 Deploy to Staging') {
            steps {
                echo '🚀 Deploying to Staging environment...'
                sh '''
                    # Stop existing staging if running
                    docker compose -f docker-compose.staging.yml down || true
                    
                    # Deploy with build number
                    BUILD_NUMBER=${BUILD_NUMBER} docker compose -f docker-compose.staging.yml up -d --build
                    
                    # Wait for container to be healthy
                    echo "⏳ Waiting for staging to be ready..."
                    sleep 10
                '''
            }
        }
        
        stage('🏥 Staging Health Check') {
            steps {
                echo '🏥 Checking staging health...'
                sh '''
                    # Health check
                    curl -f http://localhost:${STAGING_PORT}/health || exit 1
                    
                    # Check readiness
                    curl -f http://localhost:${STAGING_PORT}/ready || exit 1
                    
                    # Verify version
                    curl -s http://localhost:${STAGING_PORT}/api/info | grep -q "staging" || exit 1
                    
                    echo "✅ Staging health checks passed!"
                '''
            }
        }
        
        stage('🧪 Staging Smoke Tests') {
            steps {
                echo '🧪 Running smoke tests on staging...'
                sh '''
                    # Test home page
                    curl -f http://localhost:${STAGING_PORT}/ || exit 1
                    
                    # Test API endpoint
                    curl -f http://localhost:${STAGING_PORT}/api/info || exit 1
                    
                    # Test health endpoints
                    curl -f http://localhost:${STAGING_PORT}/health || exit 1
                    curl -f http://localhost:${STAGING_PORT}/health/detailed || exit 1
                    
                    echo "✅ All smoke tests passed!"
                '''
            }
        }
        
        stage('⏸️ Approval for Production') {
            steps {
                script {
                    // Get staging info for approval message
                    def stagingInfo = sh(
                        script: "curl -s http://localhost:${STAGING_PORT}/api/info",
                        returnStdout: true
                    ).trim()
                    
                    input message: """
                        ═══════════════════════════════════════════
                        🚨 PRODUCTION DEPLOYMENT APPROVAL REQUIRED
                        ═══════════════════════════════════════════
                        
                        Build Number: ${BUILD_NUMBER}
                        Staging URL: http://localhost:${STAGING_PORT}
                        
                        Staging Info:
                        ${stagingInfo}
                        
                        ═══════════════════════════════════════════
                        Are you sure you want to deploy to PRODUCTION?
                    """,
                    ok: '✅ Yes, Deploy to Production'
                }
            }
        }
        
        stage('🚀 Deploy to Production') {
            steps {
                echo '🚀 Deploying to Production environment...'
                sh '''
                    # Stop existing production if running
                    docker compose -f docker-compose.prod.yml down || true
                    
                    # Deploy with build number
                    BUILD_NUMBER=${BUILD_NUMBER} docker compose -f docker-compose.prod.yml up -d --build
                    
                    # Wait for container to be healthy
                    echo "⏳ Waiting for production to be ready..."
                    sleep 10
                '''
            }
        }
        
        stage('🏥 Production Health Check') {
            steps {
                echo '🏥 Checking production health...'
                sh '''
                    # Health check with retries
                    for i in 1 2 3 4 5; do
                        if curl -f http://localhost:${PROD_PORT}/health; then
                            echo "✅ Production is healthy!"
                            exit 0
                        fi
                        echo "⏳ Attempt $i failed, retrying in 5 seconds..."
                        sleep 5
                    done
                    
                    echo "❌ Production health check failed!"
                    exit 1
                '''
            }
            post {
                failure {
                    echo '🔙 Rolling back production deployment...'
                    sh '''
                        docker compose -f docker-compose.prod.yml down
                        echo "⚠️ Production rolled back. Please investigate."
                    '''
                }
            }
        }
        
        stage('✅ Verification') {
            steps {
                echo '✅ Final verification...'
                sh '''
                    echo ""
                    echo "═══════════════════════════════════════════"
                    echo "         DEPLOYMENT SUMMARY                "
                    echo "═══════════════════════════════════════════"
                    echo ""
                    echo "📦 Build Number: ${BUILD_NUMBER}"
                    echo ""
                    echo "🔷 Staging Environment:"
                    echo "   URL: http://localhost:${STAGING_PORT}"
                    curl -s http://localhost:${STAGING_PORT}/api/info | head -5
                    echo ""
                    echo "🟢 Production Environment:"
                    echo "   URL: http://localhost:${PROD_PORT}"
                    curl -s http://localhost:${PROD_PORT}/api/info | head -5
                    echo ""
                    echo "═══════════════════════════════════════════"
                '''
            }
        }
    }
    
    post {
        success {
            echo '''
            ╔═══════════════════════════════════════════════════════╗
            ║                                                       ║
            ║   ✅ CD PIPELINE COMPLETED SUCCESSFULLY!              ║
            ║                                                       ║
            ║   🔷 Staging:    http://localhost:8081               ║
            ║   🟢 Production: http://localhost:8080               ║
            ║                                                       ║
            ╚═══════════════════════════════════════════════════════╝
            '''
        }
        failure {
            echo '''
            ╔═══════════════════════════════════════════════════════╗
            ║                                                       ║
            ║   ❌ CD PIPELINE FAILED!                              ║
            ║                                                       ║
            ║   Please check the logs for details.                  ║
            ║                                                       ║
            ╚═══════════════════════════════════════════════════════╝
            '''
        }
        always {
            echo "Pipeline finished at: ${new Date()}"
        }
    }
}
```

📸 **Screenshot yang diperlukan:**
- Jenkins pipeline view dengan semua stages
- Approval gate (input step)
- Pipeline success dengan staging dan production deployed

---

### Task 4: Test Rollback Scenario

**Tujuan:** Memahami mekanisme rollback

#### Simulasikan Deployment yang Gagal

1. Buat branch baru dengan bug:
```bash
git checkout -b feature/buggy-release
```

2. Edit `app/main.py` untuk membuat `/health` return error:
```python
@app.route('/health')
def health():
    """Broken health check for testing rollback."""
    return jsonify({"status": "unhealthy"}), 500  # Intentionally broken
```

3. Commit dan push, trigger pipeline
4. Observe rollback behavior

📸 **Screenshot yang diperlukan:**
- Pipeline failure di production health check
- Rollback message di console output

---

## 📤 Format Submission

```
📁 NIM_Nama_Pertemuan07/
│
├── 📄 README.md                      # Laporan praktikum
│
├── 📁 app/
│   └── 📄 main.py
│
├── 📁 templates/
│   └── 📄 index.html
│
├── 📄 Dockerfile
├── 📄 docker-compose.staging.yml
├── 📄 docker-compose.prod.yml
├── 📄 Jenkinsfile
├── 📄 requirements.txt
│
└── 📁 screenshots/
    ├── 🖼️ 01-app-staging.png
    ├── 🖼️ 02-app-production.png
    ├── 🖼️ 03-health-check.png
    ├── 🖼️ 04-pipeline-stages.png
    ├── 🖼️ 05-approval-gate.png
    ├── 🖼️ 06-deployment-success.png
    └── 🖼️ 07-rollback-scenario.png
```

---

## ✅ Checklist Sebelum Submit

- [ ] Aplikasi Flask dengan health check endpoints berfungsi
- [ ] Docker Compose untuk staging dan production tersedia
- [ ] Pipeline Jenkins berjalan lengkap dengan approval gate
- [ ] Health check berhasil di kedua environment
- [ ] Memahami dan mendokumentasikan rollback scenario
- [ ] Semua screenshot lengkap dan jelas

---

## 💡 Tips & Best Practices

| Tip | Penjelasan |
|-----|------------|
| 🏥 Health checks wajib | Selalu implement health check endpoint |
| 🔐 Secrets management | Jangan hardcode credentials |
| 📊 Monitoring | Tambahkan logging dan metrics |
| 🔙 Rollback ready | Selalu punya strategi rollback |
| 🧪 Test di staging | Staging harus mirror production |

---

## 📚 Referensi

| Sumber | Link |
|--------|------|
| Jenkins Pipeline Syntax | [jenkins.io/doc/book/pipeline/syntax](https://www.jenkins.io/doc/book/pipeline/syntax/) |
| Docker Health Checks | [docs.docker.com/engine/reference/builder/#healthcheck](https://docs.docker.com/engine/reference/builder/#healthcheck) |
| Deployment Strategies | [martinfowler.com/bliki/BlueGreenDeployment](https://martinfowler.com/bliki/BlueGreenDeployment.html) |
| 12 Factor App | [12factor.net](https://12factor.net/) |

---

## ⏰ Deadline

<div align="center">

| 📅 Batas Pengumpulan |
|:--------------------:|
| **Sebelum Pertemuan 08 (UTS)** |

</div>

---

<div align="center">

**Happy Deploying! 🚀**

*"Release early, release often, and listen to your customers."* — Eric S. Raymond

</div>
