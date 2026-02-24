# 🧪 Pertemuan 06: CI Pipeline — Automated Testing & Code Quality

<div align="center">

| 📅 Pertemuan | ⏱️ Durasi | 📊 Tingkat |
|:------------:|:---------:|:----------:|
| 06 | 2 x 50 menit | ⭐⭐⭐ Menengah |

</div>

---

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan praktikum ini, mahasiswa diharapkan mampu:

| No | Kemampuan yang Dicapai |
|:--:|------------------------|
| 1 | Memahami **Testing Pyramid** dan jenis-jenis automated testing |
| 2 | Mengintegrasikan **automated testing** dalam CI pipeline |
| 3 | Membuat **test reports** yang informatif di Jenkins |
| 4 | Mengimplementasikan **code coverage** dan quality gates |

---

## 📚 Materi Pembelajaran

### 1️⃣ Mengapa Automated Testing Penting dalam CI/CD?

> *"If you don't have automated tests, you don't have CI."* — Dave Farley

Automated testing adalah **pondasi** dari CI/CD yang reliable. Tanpa tests otomatis, Anda tidak bisa yakin bahwa perubahan kode tidak merusak fungsionalitas yang sudah ada.

#### 📊 Dampak Testing terhadap Kualitas

| Metrik | Tanpa Automated Testing | Dengan Automated Testing |
|--------|:-----------------------:|:------------------------:|
| Bug di Production | Tinggi | **↓ 80% lebih rendah** |
| Waktu untuk Release | Lama (manual QA) | **Cepat (minutes)** |
| Confidence saat Deploy | Rendah | **Tinggi** |
| Regression Issues | Sering | **Jarang** |

---

### 2️⃣ Testing Pyramid

**Testing Pyramid** adalah konsep yang menunjukkan proporsi ideal dari berbagai jenis tests.

```
                          ╱╲
                         ╱  ╲
                        ╱ E2E╲       ← Few & Slow
                       ╱ Tests╲        (UI, browser)
                      ╱────────╲
                     ╱Integration╲   ← Some & Medium
                    ╱    Tests    ╲    (API, services)
                   ╱───────────────╲
                  ╱    Unit Tests    ╲← Many & Fast
                 ╱                    ╲  (functions, classes)
                ╱──────────────────────╲
```

#### 📋 Jenis-Jenis Test

| Jenis Test | Tujuan | Kecepatan | Jumlah | Contoh |
|------------|--------|:---------:|:------:|--------|
| **Unit Test** | Test satu fungsi/method | ⚡ Sangat cepat | Banyak | Test fungsi `calculate_total()` |
| **Integration Test** | Test interaksi antar komponen | 🚀 Cepat | Sedang | Test API endpoint + database |
| **E2E Test** | Test seluruh flow aplikasi | 🐌 Lambat | Sedikit | Test login → checkout → payment |
| **Smoke Test** | Verifikasi fungsi dasar berjalan | ⚡ Sangat cepat | Minimal | Test aplikasi bisa start |

---

### 3️⃣ Menulis Tests yang Baik

#### 🎯 Prinsip AAA (Arrange, Act, Assert)

```python
def test_add_item_to_cart():
    # Arrange - Setup test data
    cart = ShoppingCart()
    item = Product(name="Laptop", price=1000)
    
    # Act - Execute the action
    cart.add_item(item)
    
    # Assert - Verify the result
    assert cart.total_items() == 1
    assert cart.total_price() == 1000
```

#### ✅ Karakteristik Test yang Baik

| Karakteristik | Penjelasan |
|---------------|------------|
| **Fast** | Test harus cepat, idealnya < 1 detik |
| **Independent** | Test tidak bergantung pada test lain |
| **Repeatable** | Hasil sama setiap kali dijalankan |
| **Self-validating** | Pass atau fail, tidak perlu manual check |
| **Timely** | Ditulis bersamaan dengan kode |

---

### 4️⃣ Code Coverage

**Code coverage** mengukur berapa persen kode yang dijalankan oleh tests.

```
┌─────────────────────────────────────────────────────────────────┐
│                      CODE COVERAGE METRICS                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   📊 Line Coverage                                               │
│   ├── Berapa baris kode yang dieksekusi                         │
│   └── Target: minimal 80%                                        │
│                                                                  │
│   🌿 Branch Coverage                                             │
│   ├── Berapa cabang if/else yang ditest                         │
│   └── Target: minimal 70%                                        │
│                                                                  │
│   🔧 Function Coverage                                           │
│   ├── Berapa fungsi yang dipanggil                              │
│   └── Target: minimal 90%                                        │
│                                                                  │
│   ⚠️ PERINGATAN:                                                 │
│   Coverage tinggi ≠ Tests berkualitas!                          │
│   Fokus pada test yang meaningful, bukan hanya coverage.        │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

### 5️⃣ Testing dalam Jenkins Pipeline

```groovy
pipeline {
    agent any
    
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        
        stage('Unit Tests') {
            steps {
                sh '''
                    pytest tests/unit/ \
                        --junitxml=reports/unit-results.xml \
                        --cov=app \
                        --cov-report=xml:reports/coverage.xml \
                        --cov-report=html:reports/coverage-html
                '''
            }
            post {
                always {
                    // Publish test results
                    junit 'reports/unit-results.xml'
                    
                    // Publish coverage report
                    publishHTML([
                        reportDir: 'reports/coverage-html',
                        reportFiles: 'index.html',
                        reportName: 'Coverage Report'
                    ])
                }
            }
        }
        
        stage('Integration Tests') {
            steps {
                sh 'pytest tests/integration/ --junitxml=reports/integration-results.xml'
            }
            post {
                always {
                    junit 'reports/integration-results.xml'
                }
            }
        }
        
        stage('Code Quality Check') {
            steps {
                sh 'flake8 app/ --output-file=reports/flake8.txt || true'
                sh 'pylint app/ --output-format=parseable > reports/pylint.txt || true'
            }
        }
    }
}
```

---

## 🔧 Tugas Praktikum

### 📋 Prasyarat

Pastikan Anda sudah:
- ✅ Jenkins sudah berjalan (Pertemuan 05)
- ✅ Memahami dasar Python atau Node.js
- ✅ Docker terinstall

---

### Task 1: Membuat Aplikasi dengan Tests

**Tujuan:** Membuat aplikasi sederhana dengan unit tests

#### Langkah 1: Buat Struktur Proyek

```bash
mkdir -p ~/praktikum-testing
cd ~/praktikum-testing

# Struktur folder
mkdir -p app tests/unit tests/integration reports
```

#### Langkah 2: Buat Aplikasi Calculator

**File: `app/__init__.py`**
```python
# Empty file to make this a package
```

**File: `app/calculator.py`**
```python
"""
Calculator module for arithmetic operations.
"""

class Calculator:
    """A simple calculator class."""
    
    def add(self, a: float, b: float) -> float:
        """Add two numbers."""
        return a + b
    
    def subtract(self, a: float, b: float) -> float:
        """Subtract b from a."""
        return a - b
    
    def multiply(self, a: float, b: float) -> float:
        """Multiply two numbers."""
        return a * b
    
    def divide(self, a: float, b: float) -> float:
        """Divide a by b."""
        if b == 0:
            raise ValueError("Cannot divide by zero!")
        return a / b
    
    def power(self, base: float, exponent: float) -> float:
        """Calculate base raised to exponent."""
        return base ** exponent
    
    def percentage(self, value: float, percent: float) -> float:
        """Calculate percentage of a value."""
        return (value * percent) / 100
```

**File: `app/validator.py`**
```python
"""
Input validator module.
"""

def validate_number(value) -> bool:
    """Check if value is a valid number."""
    try:
        float(value)
        return True
    except (TypeError, ValueError):
        return False

def validate_positive(value: float) -> bool:
    """Check if value is positive."""
    return value > 0

def validate_range(value: float, min_val: float, max_val: float) -> bool:
    """Check if value is within range."""
    return min_val <= value <= max_val
```

#### Langkah 3: Buat Unit Tests

**File: `tests/__init__.py`**
```python
# Empty file
```

**File: `tests/unit/__init__.py`**
```python
# Empty file
```

**File: `tests/unit/test_calculator.py`**
```python
"""
Unit tests for Calculator class.
"""
import pytest
from app.calculator import Calculator


class TestCalculatorAdd:
    """Tests for add method."""
    
    def setup_method(self):
        """Setup test fixtures."""
        self.calc = Calculator()
    
    def test_add_positive_numbers(self):
        """Test adding two positive numbers."""
        result = self.calc.add(2, 3)
        assert result == 5
    
    def test_add_negative_numbers(self):
        """Test adding two negative numbers."""
        result = self.calc.add(-2, -3)
        assert result == -5
    
    def test_add_mixed_numbers(self):
        """Test adding positive and negative numbers."""
        result = self.calc.add(5, -3)
        assert result == 2
    
    def test_add_with_zero(self):
        """Test adding with zero."""
        result = self.calc.add(5, 0)
        assert result == 5
    
    def test_add_floats(self):
        """Test adding floating point numbers."""
        result = self.calc.add(1.5, 2.5)
        assert result == 4.0


class TestCalculatorSubtract:
    """Tests for subtract method."""
    
    def setup_method(self):
        self.calc = Calculator()
    
    def test_subtract_positive_numbers(self):
        result = self.calc.subtract(5, 3)
        assert result == 2
    
    def test_subtract_resulting_negative(self):
        result = self.calc.subtract(3, 5)
        assert result == -2


class TestCalculatorMultiply:
    """Tests for multiply method."""
    
    def setup_method(self):
        self.calc = Calculator()
    
    def test_multiply_positive_numbers(self):
        result = self.calc.multiply(3, 4)
        assert result == 12
    
    def test_multiply_with_zero(self):
        result = self.calc.multiply(5, 0)
        assert result == 0
    
    def test_multiply_negative_numbers(self):
        result = self.calc.multiply(-3, -4)
        assert result == 12


class TestCalculatorDivide:
    """Tests for divide method."""
    
    def setup_method(self):
        self.calc = Calculator()
    
    def test_divide_evenly(self):
        result = self.calc.divide(10, 2)
        assert result == 5
    
    def test_divide_with_remainder(self):
        result = self.calc.divide(7, 2)
        assert result == 3.5
    
    def test_divide_by_zero_raises_error(self):
        """Test that dividing by zero raises ValueError."""
        with pytest.raises(ValueError) as excinfo:
            self.calc.divide(10, 0)
        assert "Cannot divide by zero" in str(excinfo.value)


class TestCalculatorPower:
    """Tests for power method."""
    
    def setup_method(self):
        self.calc = Calculator()
    
    def test_power_positive_exponent(self):
        result = self.calc.power(2, 3)
        assert result == 8
    
    def test_power_zero_exponent(self):
        result = self.calc.power(5, 0)
        assert result == 1
    
    def test_power_negative_exponent(self):
        result = self.calc.power(2, -1)
        assert result == 0.5


class TestCalculatorPercentage:
    """Tests for percentage method."""
    
    def setup_method(self):
        self.calc = Calculator()
    
    def test_percentage_basic(self):
        result = self.calc.percentage(100, 25)
        assert result == 25
    
    def test_percentage_half(self):
        result = self.calc.percentage(200, 50)
        assert result == 100
```

**File: `tests/unit/test_validator.py`**
```python
"""
Unit tests for validator module.
"""
import pytest
from app.validator import validate_number, validate_positive, validate_range


class TestValidateNumber:
    """Tests for validate_number function."""
    
    def test_valid_integer(self):
        assert validate_number(42) is True
    
    def test_valid_float(self):
        assert validate_number(3.14) is True
    
    def test_valid_string_number(self):
        assert validate_number("123") is True
    
    def test_invalid_string(self):
        assert validate_number("hello") is False
    
    def test_none_value(self):
        assert validate_number(None) is False


class TestValidatePositive:
    """Tests for validate_positive function."""
    
    def test_positive_number(self):
        assert validate_positive(5) is True
    
    def test_zero(self):
        assert validate_positive(0) is False
    
    def test_negative_number(self):
        assert validate_positive(-5) is False


class TestValidateRange:
    """Tests for validate_range function."""
    
    def test_within_range(self):
        assert validate_range(5, 1, 10) is True
    
    def test_at_min_boundary(self):
        assert validate_range(1, 1, 10) is True
    
    def test_at_max_boundary(self):
        assert validate_range(10, 1, 10) is True
    
    def test_below_range(self):
        assert validate_range(0, 1, 10) is False
    
    def test_above_range(self):
        assert validate_range(11, 1, 10) is False
```

#### Langkah 4: Buat Requirements

**File: `requirements.txt`**
```
pytest==7.4.0
pytest-cov==4.1.0
flake8==6.1.0
```

#### Langkah 5: Jalankan Tests Lokal

```bash
# Install dependencies
pip install -r requirements.txt

# Run tests dengan coverage
pytest tests/unit/ -v --cov=app --cov-report=term-missing

# Generate HTML report
pytest tests/unit/ --cov=app --cov-report=html
```

📸 **Screenshot yang diperlukan:**
- Output pytest dengan semua tests PASSED
- Coverage report di terminal

---

### Task 2: Integrasi Testing ke Jenkins

**Tujuan:** Membuat pipeline yang menjalankan tests otomatis

#### Langkah 1: Buat Dockerfile untuk Testing

**File: `Dockerfile`**
```dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Default command
CMD ["pytest", "tests/", "-v"]
```

#### Langkah 2: Buat Jenkinsfile

**File: `Jenkinsfile`**
```groovy
pipeline {
    agent any
    
    environment {
        PYTHON_IMAGE = 'python:3.11-slim'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }
        
        stage('Setup') {
            steps {
                echo '📦 Installing dependencies...'
                sh '''
                    docker run --rm -v $(pwd):/app -w /app ${PYTHON_IMAGE} \
                        pip install -r requirements.txt --target=./deps
                '''
            }
        }
        
        stage('Lint') {
            steps {
                echo '🔍 Running code linting...'
                sh '''
                    docker run --rm -v $(pwd):/app -w /app ${PYTHON_IMAGE} \
                        sh -c "pip install flake8 && flake8 app/ --max-line-length=100 || true"
                '''
            }
        }
        
        stage('Unit Tests') {
            steps {
                echo '🧪 Running unit tests...'
                sh '''
                    docker run --rm -v $(pwd):/app -w /app ${PYTHON_IMAGE} \
                        sh -c "pip install pytest pytest-cov && \
                               pytest tests/unit/ -v \
                               --junitxml=/app/reports/unit-results.xml \
                               --cov=app \
                               --cov-report=xml:/app/reports/coverage.xml \
                               --cov-report=html:/app/reports/coverage-html"
                '''
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'reports/unit-results.xml'
                }
            }
        }
        
        stage('Coverage Report') {
            steps {
                echo '📊 Publishing coverage report...'
                publishHTML([
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reports/coverage-html',
                    reportFiles: 'index.html',
                    reportName: 'Code Coverage Report'
                ])
            }
        }
        
        stage('Quality Gate') {
            steps {
                echo '🚦 Checking quality gates...'
                script {
                    // Parse coverage and check threshold
                    def coverageOk = sh(
                        script: '''
                            docker run --rm -v $(pwd):/app -w /app ${PYTHON_IMAGE} \
                                sh -c "pip install pytest pytest-cov && \
                                       pytest tests/unit/ --cov=app --cov-fail-under=70"
                        ''',
                        returnStatus: true
                    ) == 0
                    
                    if (!coverageOk) {
                        echo '⚠️ Coverage below 70% threshold!'
                        // Uncomment to fail pipeline: error("Coverage below threshold")
                    } else {
                        echo '✅ Coverage meets threshold!'
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo '''
            ╔════════════════════════════════════════════════════╗
            ║  ✅ ALL TESTS PASSED!                              ║
            ║  Code is ready for the next stage.                 ║
            ╚════════════════════════════════════════════════════╝
            '''
        }
        failure {
            echo '''
            ╔════════════════════════════════════════════════════╗
            ║  ❌ TESTS FAILED!                                  ║
            ║  Please check the test reports for details.        ║
            ╚════════════════════════════════════════════════════╝
            '''
        }
        always {
            echo "Build finished: ${currentBuild.currentResult}"
            // Clean up workspace
            cleanWs()
        }
    }
}
```

#### Langkah 3: Push dan Jalankan Pipeline

```bash
# Initialize git jika belum
git init
git add .
git commit -m "feat: add calculator app with unit tests"

# Push ke GitHub
git remote add origin https://github.com/[username]/praktikum-testing.git
git push -u origin main
```

Di Jenkins:
1. Buat Pipeline job baru
2. Arahkan ke repository
3. Klik **Build Now**

📸 **Screenshot yang diperlukan:**
- Pipeline stages di Jenkins
- Test results di Jenkins
- Coverage report HTML

---

### Task 3: Tambahkan Test Failure Scenario

**Tujuan:** Memahami bagaimana Jenkins menangani test failure

#### Tambahkan Failing Test

**File: `tests/unit/test_failing.py`**
```python
"""
Intentionally failing test for demonstration.
"""
import pytest


class TestIntentionalFailure:
    """Tests that will fail - for demonstration only."""
    
    @pytest.mark.skip(reason="Remove this skip to see failure handling")
    def test_that_will_fail(self):
        """This test will fail intentionally."""
        assert 1 == 2, "This should fail!"
    
    def test_that_passes(self):
        """This test will pass."""
        assert 1 == 1
```

Jalankan pipeline dan perhatikan:
1. Bagaimana Jenkins menampilkan failing tests
2. Bagaimana test report menunjukkan detail failure
3. Pipeline status (unstable vs failed)

📸 **Screenshot yang diperlukan:**
- Test failure di Jenkins (setelah menghapus @skip)
- Detail error message

---

### Task 4: Integration Test Sederhana

**Tujuan:** Membuat integration test

**File: `tests/integration/__init__.py`**
```python
# Empty file
```

**File: `tests/integration/test_calculator_integration.py`**
```python
"""
Integration tests for Calculator - testing multiple operations together.
"""
import pytest
from app.calculator import Calculator
from app.validator import validate_number, validate_positive


class TestCalculatorIntegration:
    """Integration tests combining Calculator with Validator."""
    
    def setup_method(self):
        self.calc = Calculator()
    
    def test_validated_calculation(self):
        """Test calculation with input validation."""
        # Arrange
        input_a = "10"
        input_b = "5"
        
        # Validate inputs
        assert validate_number(input_a)
        assert validate_number(input_b)
        
        # Act - Convert and calculate
        a = float(input_a)
        b = float(input_b)
        
        # Multiple operations
        sum_result = self.calc.add(a, b)
        product = self.calc.multiply(a, b)
        
        # Assert
        assert sum_result == 15
        assert product == 50
    
    def test_chain_calculations(self):
        """Test chaining multiple calculations."""
        # Calculate ((10 + 5) * 2) - 10 = 20
        result = self.calc.add(10, 5)
        result = self.calc.multiply(result, 2)
        result = self.calc.subtract(result, 10)
        
        assert result == 20
    
    def test_percentage_of_sum(self):
        """Test calculating percentage of a sum."""
        # Calculate 25% of (100 + 100) = 50
        total = self.calc.add(100, 100)
        assert validate_positive(total)
        
        result = self.calc.percentage(total, 25)
        assert result == 50
```

Update Jenkinsfile untuk include integration tests:

```groovy
stage('Integration Tests') {
    steps {
        echo '🔗 Running integration tests...'
        sh '''
            docker run --rm -v $(pwd):/app -w /app ${PYTHON_IMAGE} \
                sh -c "pip install pytest && \
                       pytest tests/integration/ -v \
                       --junitxml=/app/reports/integration-results.xml"
        '''
    }
    post {
        always {
            junit allowEmptyResults: true, testResults: 'reports/integration-results.xml'
        }
    }
}
```

📸 **Screenshot yang diperlukan:**
- Integration test results di Jenkins
- Combined test report (unit + integration)

---

## 📤 Format Submission

```
📁 NIM_Nama_Pertemuan06/
│
├── 📄 README.md                      # Laporan praktikum
│
├── 📁 app/
│   ├── 📄 __init__.py
│   ├── 📄 calculator.py
│   └── 📄 validator.py
│
├── 📁 tests/
│   ├── 📄 __init__.py
│   ├── 📁 unit/
│   │   ├── 📄 __init__.py
│   │   ├── 📄 test_calculator.py
│   │   └── 📄 test_validator.py
│   └── 📁 integration/
│       ├── 📄 __init__.py
│       └── 📄 test_calculator_integration.py
│
├── 📄 Jenkinsfile
├── 📄 Dockerfile
├── 📄 requirements.txt
│
└── 📁 screenshots/
    ├── 🖼️ 01-local-tests.png
    ├── 🖼️ 02-coverage-terminal.png
    ├── 🖼️ 03-jenkins-pipeline.png
    ├── 🖼️ 04-jenkins-test-results.png
    ├── 🖼️ 05-coverage-report.png
    └── 🖼️ 06-integration-tests.png
```

---

## ✅ Checklist Sebelum Submit

- [ ] Aplikasi Calculator dengan minimal 5 fungsi
- [ ] Minimal 15 unit tests yang PASS
- [ ] Integration tests berjalan sukses
- [ ] Coverage report ter-generate
- [ ] Pipeline Jenkins berjalan tanpa error
- [ ] Test reports dapat diakses di Jenkins
- [ ] Semua screenshot lengkap dan jelas

---

## 💡 Tips Testing

| Tip | Penjelasan |
|-----|------------|
| 🎯 Test edge cases | Jangan hanya test happy path |
| 📝 Nama test deskriptif | `test_divide_by_zero_raises_error` |
| 🔄 Independent tests | Setiap test bisa jalan sendiri |
| 🚀 Fast feedback | Tests harus cepat (< 1 detik per test) |
| 📊 Meaningful coverage | Fokus pada logic kompleks |

---

## 📚 Referensi

| Sumber | Link |
|--------|------|
| Pytest Documentation | [docs.pytest.org](https://docs.pytest.org/) |
| pytest-cov | [pytest-cov.readthedocs.io](https://pytest-cov.readthedocs.io/) |
| Testing Best Practices | [martinfowler.com/testing](https://martinfowler.com/testing/) |
| Jenkins Test Reports | [jenkins.io/doc/book/pipeline/test](https://www.jenkins.io/doc/book/pipeline/test/) |

---

## ⏰ Deadline

<div align="center">

| 📅 Batas Pengumpulan |
|:--------------------:|
| **Sebelum Pertemuan 07** |

</div>

---

<div align="center">

**Happy Testing! 🧪**

*"Code without tests is broken by design."* — Jacob Kaplan-Moss

</div>
