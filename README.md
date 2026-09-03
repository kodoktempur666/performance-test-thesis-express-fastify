<div align="center">

# ⚡ Performance Test Thesis

### Benchmark Express.js vs Fastify untuk REST API E-Commerce

![Node.js](https://img.shields.io/badge/Node.js-24.x-5FA04E?style=for-the-badge&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express.js-5.x-000000?style=for-the-badge&logo=express&logoColor=white)
![Fastify](https://img.shields.io/badge/Fastify-5.x-202020?style=for-the-badge&logo=fastify&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-5432-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-Queue-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![k6](https://img.shields.io/badge/k6-Load_Testing-7D64FF?style=for-the-badge&logo=k6&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-Dashboard-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-Metrics-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)

</div>

---

## 📌 Ringkasan Project

Repository ini dibuat untuk kebutuhan penelitian/skripsi mengenai perbandingan performa backend REST API berbasis Express.js dan Fastify. Sistem yang diuji mensimulasikan alur e-commerce sederhana, seperti autentikasi user, browsing produk, aktivitas keranjang, checkout, dan monitoring performa server.

Project terdiri dari dua versi utama:

1. Baseline / Skripsi
   - Express API berjalan di port 3000
   - Fastify API berjalan di port 3005

2. Optimized / Optimasi
   - Express API optimasi berjalan di port 3010
   - Fastify API optimasi berjalan di port 3015
   - Dilengkapi queue/worker untuk proses asynchronous seperti checkout, email, stock, dan cart activity

Monitoring dilakukan dengan Prometheus dan Grafana, sedangkan load testing dilakukan menggunakan k6.

---

## ✨ Tujuan Pengujian

- Membandingkan performa Express.js dan Fastify pada skenario REST API yang serupa.
- Mengukur latency, throughput, error rate, dan stabilitas server saat menerima beban bertahap maupun spike.
- Mengevaluasi dampak optimasi asynchronous worker/queue terhadap performa aplikasi.
- Menyediakan data pendukung untuk analisis performa backend pada penelitian akademik.

---

## 🧱 Arsitektur Singkat

```text
┌────────────────────┐
│      k6 Tests      │
│  load-test-script  │
└─────────┬──────────┘
          │ HTTP request
          ▼
┌────────────────────┐       ┌────────────────────┐
│    Express API     │       │    Fastify API     │
│  Baseline / Opti   │       │  Baseline / Opti   │
└─────────┬──────────┘       └─────────┬──────────┘
          │                            │
          ├──────────────┬─────────────┤
          ▼              ▼             ▼
┌────────────────┐ ┌──────────────┐ ┌────────────────┐
│  PostgreSQL    │ │    Redis     │ │ Queue Workers  │
│  Database      │ │ BullMQ Queue │ │ Checkout/Stock │
└────────────────┘ └──────────────┘ └────────────────┘
          │
          ▼
┌────────────────────┐
│ Prometheus Metrics │
└─────────┬──────────┘
          ▼
┌────────────────────┐
│  Grafana Dashboard │
└────────────────────┘
```

---

## 📁 Struktur Folder

```text
.
├── fastify-express-skripsi/
│   ├── express/              # Express baseline
│   ├── fastify/              # Fastify baseline
│   └── docker-compose.yaml   # Container baseline
│
├── fastify-express-opti/
│   ├── express/              # Express versi optimasi
│   ├── fastify/              # Fastify versi optimasi
│   └── docker-compose.yaml   # Container versi optimasi + worker
│
├── grafana-monitoring/
│   ├── prometheus/           # Konfigurasi scrape target Prometheus
│   ├── grafana/              # Dashboard dan datasource Grafana
│   └── docker-compose.yaml   # Service Grafana + Prometheus
│
└── load-test-script/
    ├── auth-spike-test.js        # Test register dan login
    ├── browsing-test.js          # Test browsing produk dan kategori
    ├── cart-activit-test.js      # Test aktivitas cart
    ├── mass-checkout-test.js     # Test checkout massal/spike
    └── latency_avg.json          # Contoh output/query latency Prometheus
```

---

## 🛠️ Tech Stack

| Komponen | Teknologi |
|---|---|
| Runtime | Node.js 24 |
| Backend Framework | Express.js 5 dan Fastify 5 |
| Database | PostgreSQL |
| Queue / Worker | BullMQ + Redis |
| Metrics | prom-client |
| Monitoring | Prometheus + Grafana |
| Load Testing | k6 |
| Containerization | Docker dan Docker Compose |

---

## 🚀 Step-by-Step Penginstalan

### 1. Clone repository

```bash
git clone <url-repository>
cd performance-test-thesis
```

Jika folder ini sudah ada di lokal, cukup masuk ke direktori project:

```bash
cd performance-test-thesis
```

---

### 2. Install dependency sistem

Pastikan tools berikut sudah tersedia:

```bash
node --version
npm --version
docker --version
docker compose version
k6 version
```

Rekomendasi versi:

- Node.js 24.x
- Docker versi terbaru
- Docker Compose v2
- k6 versi terbaru
- PostgreSQL dan Redis jika menjalankan service tanpa container penuh

---

### 3. Siapkan database PostgreSQL

Project menggunakan konfigurasi default berikut:

```env
DB_HOST=127.0.0.1
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=psql
DB_NAME=mydb
```

Buat database:

```bash
createdb mydb
```

Atau melalui psql:

```bash
psql -U postgres
CREATE DATABASE mydb;
\q
```

Catatan: sesuaikan username, password, dan host database jika environment lokal berbeda.

---

### 4. Jalankan Redis

Redis dibutuhkan untuk versi optimasi yang menggunakan queue/worker.

Menggunakan Docker:

```bash
docker run -d \
  --name redis-performance-test \
  -p 6379:6379 \
  redis:latest
```

Cek status Redis:

```bash
docker ps
```

---

### 5. Install dependency aplikasi baseline

Express baseline:

```bash
cd fastify-express-skripsi/express
npm install
```

Fastify baseline:

```bash
cd ../fastify
npm install
```

Kembali ke root project:

```bash
cd ../../
```

---

### 6. Install dependency aplikasi optimasi

Express optimasi:

```bash
cd fastify-express-opti/express
npm install
```

Fastify optimasi:

```bash
cd ../fastify
npm install
```

Kembali ke root project:

```bash
cd ../../
```

---

### 7. Buat file environment `.env`

Buat file `.env` pada masing-masing folder aplikasi jika menjalankan via `npm run dev`.

Contoh untuk Express baseline (`fastify-express-skripsi/express/.env`):

```env
PORT=3000
DB_HOST=127.0.0.1
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=psql
DB_NAME=mydb
```

Contoh untuk Fastify baseline (`fastify-express-skripsi/fastify/.env`):

```env
PORT=3005
DB_HOST=127.0.0.1
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=psql
DB_NAME=mydb
```

Contoh untuk Express optimasi (`fastify-express-opti/express/.env`):

```env
PORT=3010
DB_HOST=127.0.0.1
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=psql
DB_NAME=mydb
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
```

Contoh untuk Fastify optimasi (`fastify-express-opti/fastify/.env`):

```env
PORT=3015
DB_HOST=127.0.0.1
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=psql
DB_NAME=mydb
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
```

---

### 8. Jalankan aplikasi secara lokal

Express baseline:

```bash
cd fastify-express-skripsi/express
npm run dev
```

Fastify baseline:

```bash
cd fastify-express-skripsi/fastify
npm run dev
```

Express optimasi:

```bash
cd fastify-express-opti/express
npm run dev
```

Fastify optimasi:

```bash
cd fastify-express-opti/fastify
npm run dev
```

Endpoint health check:

```bash
curl http://localhost:3000/
curl http://localhost:3005/
curl http://localhost:3010/
curl http://localhost:3015/
```

Endpoint metrics Prometheus:

```bash
curl http://localhost:3000/metrics
curl http://localhost:3005/metrics
curl http://localhost:3010/metrics
curl http://localhost:3015/metrics
```

---

## 🐳 Menjalankan dengan Docker Compose

### Baseline

```bash
cd fastify-express-skripsi
docker compose up -d --build
```

Service baseline:

| Service | Port |
|---|---:|
| Express baseline | 3000 |
| Fastify baseline | 3005 |
| Node Exporter | 9100 |

### Optimasi

```bash
cd fastify-express-opti
docker compose up -d --build
```

Service optimasi:

| Service | Port |
|---|---:|
| Express optimasi | 3010 |
| Fastify optimasi | 3015 |
| Checkout worker | background worker |

Catatan: konfigurasi Docker Compose saat ini menggunakan `network_mode: "host"`, sehingga paling cocok dijalankan pada Linux. Jika dijalankan pada Windows/Mac, konfigurasi network mungkin perlu disesuaikan.

---

## 📊 Menjalankan Monitoring Grafana + Prometheus

Masuk ke folder monitoring:

```bash
cd grafana-monitoring
```

Jalankan service:

```bash
docker compose up -d
```

Akses dashboard:

| Tool | URL |
|---|---|
| Prometheus | http://localhost:9090 |
| Grafana | http://localhost:3000 |

Login default Grafana:

```text
username: admin
password: admin
```

Jika Grafana meminta mengganti password, sesuaikan dengan kebutuhan.

Prometheus akan membaca target dari:

```text
grafana-monitoring/prometheus/prometheus.yml
```

Sesuaikan IP target jika aplikasi dijalankan di server berbeda.

---

## 🧪 Menjalankan Load Test dengan k6

Semua script pengujian berada pada folder:

```text
load-test-script/
```

Format umum:

```bash
HOST=<host-api> PORT=<port-api> k6 run load-test-script/<nama-script>.js
```

Contoh menjalankan test ke Express baseline:

```bash
HOST=127.0.0.1 PORT=3000 k6 run load-test-script/browsing-test.js
```

Contoh menjalankan test ke Fastify baseline:

```bash
HOST=127.0.0.1 PORT=3005 k6 run load-test-script/browsing-test.js
```

Contoh menjalankan test ke Express optimasi:

```bash
HOST=127.0.0.1 PORT=3010 k6 run load-test-script/mass-checkout-test.js
```

Contoh menjalankan test ke Fastify optimasi:

```bash
HOST=127.0.0.1 PORT=3015 k6 run load-test-script/mass-checkout-test.js
```

---

## 🧩 Skenario Pengujian

| Script | Skenario | Beban | Tujuan |
|---|---|---:|---|
| `auth-spike-test.js` | Register dan login user | 20 → 40 → 80 req/s | Menguji endpoint autentikasi saat spike |
| `browsing-test.js` | Get products, detail product, categories | 100 → 300 → 600 req/s | Menguji pembacaan data produk/kategori |
| `cart-activit-test.js` | Create cart, add item, update item, get cart | 80 → 100 → 120 req/s | Menguji aktivitas keranjang |
| `mass-checkout-test.js` | Create cart, add item, checkout | 100 → 300 → 600 req/s | Menguji proses checkout massal |

Threshold default pada script:

```text
http_req_failed < 2%
```

---

## 📈 Template Hasil Performa

Bagian ini disiapkan sebagai template awal. Isi nilai aktual setelah menjalankan k6, Prometheus, dan Grafana.

### A. Ringkasan Lingkungan Pengujian

| Item | Nilai |
|---|---|
| Tanggal pengujian | `DD/MM/YYYY` |
| Lokasi server | `Local / VPS / Cloud Provider` |
| Spesifikasi CPU | `... core / ... vCPU` |
| RAM | `... GB` |
| OS | `...` |
| Node.js | `v...` |
| PostgreSQL | `v...` |
| Redis | `v...` |
| Durasi tiap skenario | `... menit` |
| Tool load test | `k6 v...` |

### B. Hasil Express vs Fastify Baseline

| Skenario | Framework | Port | Request Rate | Avg Latency | P95 Latency | P99 Latency | Throughput | Error Rate | Status |
|---|---|---:|---:|---:|---:|---:|---:|---:|---|
| Auth Spike | Express | 3000 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Auth Spike | Fastify | 3005 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Browsing Load | Express | 3000 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Browsing Load | Fastify | 3005 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Cart Activity | Express | 3000 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Cart Activity | Fastify | 3005 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Mass Checkout | Express | 3000 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Mass Checkout | Fastify | 3005 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |

### C. Hasil Express vs Fastify Optimasi

| Skenario | Framework | Port | Request Rate | Avg Latency | P95 Latency | P99 Latency | Throughput | Error Rate | Status |
|---|---|---:|---:|---:|---:|---:|---:|---:|---|
| Auth Spike | Express Opti | 3010 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Auth Spike | Fastify Opti | 3015 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Browsing Load | Express Opti | 3010 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Browsing Load | Fastify Opti | 3015 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Cart Activity | Express Opti | 3010 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Cart Activity | Fastify Opti | 3015 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Mass Checkout | Express Opti | 3010 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |
| Mass Checkout | Fastify Opti | 3015 | `... req/s` | `... ms` | `... ms` | `... ms` | `... req/s` | `... %` | `PASS/FAIL` |

### D. Perbandingan Optimasi

| Skenario | Framework | Baseline Avg Latency | Optimized Avg Latency | Perubahan Latency | Baseline Error Rate | Optimized Error Rate | Kesimpulan |
|---|---|---:|---:|---:|---:|---:|---|
| Auth Spike | Express | `... ms` | `... ms` | `... %` | `... %` | `... %` | `...` |
| Auth Spike | Fastify | `... ms` | `... ms` | `... %` | `... %` | `... %` | `...` |
| Browsing Load | Express | `... ms` | `... ms` | `... %` | `... %` | `... %` | `...` |
| Browsing Load | Fastify | `... ms` | `... ms` | `... %` | `... %` | `... %` | `...` |
| Cart Activity | Express | `... ms` | `... ms` | `... %` | `... %` | `... %` | `...` |
| Cart Activity | Fastify | `... ms` | `... ms` | `... %` | `... %` | `... %` | `...` |
| Mass Checkout | Express | `... ms` | `... ms` | `... %` | `... %` | `... %` | `...` |
| Mass Checkout | Fastify | `... ms` | `... ms` | `... %` | `... %` | `... %` | `...` |

### E. Catatan Observasi

Tuliskan temuan selama pengujian:

```text
1. Pada skenario ..., framework ... menunjukkan latency lebih rendah.
2. Pada beban ..., error rate mulai meningkat pada service ...
3. Worker/queue berdampak pada ...
4. Bottleneck utama terlihat pada ...
5. Rekomendasi optimasi lanjutan: ...
```

---

## 🔎 Query Prometheus yang Berguna

Average latency:

```promql
rate(http_request_duration_seconds_sum[1m])
/
rate(http_request_duration_seconds_count[1m])
```

P95 latency:

```promql
histogram_quantile(
  0.95,
  sum(rate(http_request_duration_seconds_bucket[1m])) by (le, job)
)
```

P99 latency:

```promql
histogram_quantile(
  0.99,
  sum(rate(http_request_duration_seconds_bucket[1m])) by (le, job)
)
```

Request rate:

```promql
sum(rate(http_request_duration_seconds_count[1m])) by (job)
```

Error rate berdasarkan status code 5xx:

```promql
sum(rate(http_request_duration_seconds_count{status_code=~"5.."}[1m])) by (job)
/
sum(rate(http_request_duration_seconds_count[1m])) by (job)
```

CPU usage node exporter:

```promql
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[1m])) * 100)
```

Memory usage node exporter:

```promql
(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes)
/
node_memory_MemTotal_bytes * 100
```

---

## ✅ Checklist Pengujian

- [ ] Database PostgreSQL berjalan
- [ ] Redis berjalan untuk versi optimasi
- [ ] Express baseline aktif di port 3000
- [ ] Fastify baseline aktif di port 3005
- [ ] Express optimasi aktif di port 3010
- [ ] Fastify optimasi aktif di port 3015
- [ ] Endpoint `/metrics` dapat diakses
- [ ] Prometheus berhasil scrape semua target
- [ ] Grafana menampilkan dashboard
- [ ] Script k6 berhasil dijalankan
- [ ] Hasil pengujian dicatat pada template performa

---

## 🧠 Format Kesimpulan Penelitian

Gunakan format berikut setelah semua data performa terisi:

```text
Berdasarkan hasil pengujian menggunakan k6 pada skenario Auth Spike, Browsing Load, Cart Activity, dan Mass Checkout, framework ... menunjukkan performa terbaik pada metrik ... dengan rata-rata latency ... ms dan error rate ...%.

Pada versi optimasi, penggunaan queue/worker memberikan dampak ... terhadap endpoint checkout/cart karena proses berat dipindahkan ke background worker.

Secara keseluruhan, ... lebih sesuai untuk kebutuhan ... sedangkan ... memiliki keunggulan pada ...
```

---

## 📝 Catatan

- File hasil performa pada README ini masih berupa template, sesuai kebutuhan awal.
- Nilai aktual dapat diisi setelah load test selesai dijalankan.
- Jika pengujian dilakukan pada server/cloud, pastikan IP target di Prometheus dan variabel `HOST` pada k6 sudah disesuaikan.
- Untuk hasil yang adil, jalankan setiap framework pada spesifikasi server, data awal, durasi, dan skenario load yang sama.

---

<div align="center">

### Dibuat untuk dokumentasi benchmark performa Express.js vs Fastify

</div>
