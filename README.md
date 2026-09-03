<div align="center">

## ANALISIS KOMPARATIF DAN OPTIMASI KINERJA FRAMEWORK EXPRESS.JS DAN FASTIFY DALAM MENANGANI KONDISI HIGH CONCURRENT REQUEST MENGGUNAKAN METODE LOAD TESTING

Telkom University — S1 Rekayasa Perangkat Lunak, 2026

[🔗 Lihat Karya Ilmiah di Telkom University Open Library](https://openlibrary.telkomuniversity.ac.id/home/catalog/id/248710/slug/analisis-komparatif-dan-optimasi-kinerja-framework-express-js-dan-fastify-dalam-menangani-kondisi-high-concurrent-request-menggunakan-metode-load-testing-dalam-bentuk-buku-karya-ilmiah.html)

### ⚡ Benchmark Express.js vs Fastify untuk REST API E-Commerce

![Node.js](https://img.shields.io/badge/Node.js-24-5FA04E?style=for-the-badge&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express.js-5.2.1-000000?style=for-the-badge&logo=express&logoColor=white)
![Fastify](https://img.shields.io/badge/Fastify-5.7.2-202020?style=for-the-badge&logo=fastify&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-DBMS-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-Queue-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![BullMQ](https://img.shields.io/badge/BullMQ-Job_Queue-202020?style=for-the-badge)
![k6](https://img.shields.io/badge/k6-Load_Testing-7D64FF?style=for-the-badge&logo=k6&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-Dashboard-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-Metrics-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)

</div>

---

## 📌 Ringkasan Project

Repository ini dibuat untuk kebutuhan penelitian/skripsi mengenai perbandingan performa backend REST API berbasis Express.js dan Fastify. Sistem yang diuji mensimulasikan alur e-commerce sederhana, seperti autentikasi user, browsing produk, aktivitas create cart, checkout, dan monitoring performa server.

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

## 🧱 Arsitektur Infrastruktur AWS

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                       AWS CLOUD                                        │
│                                                                                        │
│  ┌──────────────────────┐    ┌──────────────────────┐    ┌──────────────────────────┐  │
│  │  Availability Zone   │    │  Availability Zone   │    │    Availability Zone     │  │
│  │   ap-southeast-1a    │    │   ap-southeast-1b    │    │     ap-southeast-1c      │  │
│  │                      │    │                      │    │                          │  │
│  │  ┌────────────────┐  │    │  ┌────────────────┐  │    │  ┌────────────────────┐  │  │
│  │  │ Public Subnet  │  │    │  │ Public Subnet  │  │    │  │   Public Subnet    │  │  │
│  │  │                │  │    │  │                │  │    │  │                    │  │  │
│  │  │  ┌──────────┐  │  │    │  │  ┌──────────┐  │  │    │  │  ┌──────────────┐  │  │  │
│  │  │  │    k6    │  │  │    │  │  │ REST API │  │  │    │  │  │  Prometheus  │  │  │  │
│  │  │  │          │  │  │    │  │  │          │  │  │    │  │  │      ↓       │  │  │  │
│  │  │  │t3.medium │  │  │    │  │  │c5.xlarge │  │  │    │  │  │   Grafana    │  │  │  │
│  │  │  └────┬─────┘  │  │    │  │  └────▲───┬─┘  │  │    │  │  │  c5a.xlarge  │  │  │  │
│  │  └───────┼────────┘  │    │  └───────┼───┼────┘  │    │  └──┴──────▲───────┴──┘  │  │
│  └──────────┼───────────┘    └──────────┼───┼───────┘    └────────────┼─────────────┘  │
│             │                           │   │                           │              │
│             └─────── HTTP Request ──────┘   └──── Metrics Scraping ─────┘              │
│                                                                                        │
└────────────────────────────────────────────────────────────────────────────────────────┘

```
---

## 📁 Struktur Folder

```text
.
├── fastify-express-baseline/
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
| `cart-activit-test.js` | Create cart, add item, update item, get cart | 80 → 100 → 120 req/s | Menguji aktivitas create cart |
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
| Lingkungan Pengujian | `AWS EC2` |
| Spesifikasi | `4 vCPU` |
| RAM | `8 GB` |
| OS | `ubuntu 24.04` |
| Node.js | `v 24.16` |
| PostgreSQL | `v 18.6` |


### B. Hasil Express vs Fastify Baseline

| Skenario      | Framework | Port | P95 Latency |    Throughput |
| ------------- | --------- | ---: | ----------: | ------------: |
| Auth Spike    | Express   | 3000 |   `2620 ms` |  `7242 req/s` |
| Auth Spike    | Fastify   | 3005 |   `2740 ms` |  `7236 req/s` |
| Browsing Load | Express   | 3000 | `0.4205 ms` | `90447 req/s` |
| Browsing Load | Fastify   | 3005 | `0.4675 ms` | `90447 req/s` |
| Cart Activity | Express   | 3000 |   `2.14 ms` | `36596 req/s` |
| Cart Activity | Fastify   | 3005 |   `2.39 ms` | `36596 req/s` |
| Mass Checkout | Express   | 3000 |  `15.25 ms` | `81819 req/s` |
| Mass Checkout | Fastify   | 3005 |   `37.5 ms` | `81543 req/s` |

### C. Hasil Express vs Fastify Optimasi

| Skenario      | Framework        | Port | P95 Latency |    Throughput |
| ------------- | ---------------- | ---: | ----------: | ------------: |
| Mass Checkout | Express Optimasi | 3010 |   `5.05 ms` | `86249 req/s` |
| Mass Checkout | Fastify Optimasi | 3015 |   `5.67 ms` | `84699 req/s` |

### D. Perbandingan Optimasi dengan Baseline

| Skenario      | Framework | Baseline P95 | Optimasi P95 | Perubahan P95 |
| ------------- | --------- | -----------: | ------------: | ------------: |
| Mass Checkout | Express   |   `15.25 ms` |     `5.05 ms` |  **↓ 66.89%** |
| Mass Checkout | Fastify   |    `37.5 ms` |     `5.67 ms` |  **↓ 84.88%** |


### E. Catatan Observasi

Tuliskan temuan selama pengujian:

```text
1. Pada skenario Auth Spike, Express menunjukkan P95 latency
   yang lebih rendah dibandingkan Fastify.

2. Pada skenario Browsing Load, Express menunjukkan P95 latency
   yang lebih rendah dibandingkan Fastify.

3. Pada skenario Cart Activity, Express memiliki P95 latency
   yang lebih rendah dibandingkan Fastify.

4. Pada skenario Mass Checkout baseline, Express menunjukkan
   P95 latency yang lebih rendah dibandingkan Fastify.

5. Setelah optimasi menggunakan Redis dan BullMQ, kedua framework
   mengalami penurunan P95 latency pada skenario Mass Checkout.

6. Express Optimasi memperoleh P95 latency lebih rendah
   dibandingkan Fastify Optimasi.

7. Throughput Express Optimasi lebih tinggi dibandingkan
   Fastify Optimasi, yaitu 86249 req/s dan 84699 req/s.

8. Optimasi Redis dan BullMQ memberikan peningkatan performa
   pada skenario Mass Checkout.
```

---

## 🔎 Query Prometheus

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


## 🧠 Format Kesimpulan Penelitian

Gunakan format berikut setelah semua data performa terisi:

```text
Berdasarkan hasil pengujian throughput, percentile latency, dan kenaikan RPS
per interval, dapat disimpulkan bahwa performa Express.js dan Fastify
menunjukkan karakteristik yang berbeda pada setiap skenario pengujian.
Pada pengujian throughput, Express.js secara konsisten memperoleh nilai
total request dan RPS yang sedikit lebih tinggi dibandingkan Fastify,
terutama pada skenario Mass Checkout dan Mass Checkout setelah optimasi.
Perbedaan performa tersebut dipengaruhi oleh karakteristik arsitektur
masing-masing framework.

Hasil metrik juga dipengaruhi oleh spesifikasi lingkungan pengujian.
Kapasitas CPU, RAM, bandwidth jaringan, serta performa I/O
pada VPS berpengaruh terhadap kemampuan server dalam menangani
beban concurrent request. Ketika jumlah request meningkat, penggunaan CPU
menjadi faktor utama yang memengaruhi throughput dan latency.
Keterbatasan resource server dapat menyebabkan bottleneck sehingga nilai
RPS menurun pada interval akhir pengujian meskipun penggunaan resource
masih tinggi. Dalam hal ini, penambahan spesifikasi server perlu dilakukan
berdasarkan kebutuhan sistem dan estimasi jumlah concurrent user
agar performa aplikasi tetap stabil, mampu mempertahankan throughput
yang tinggi, serta meminimalkan peningkatan latency pada kondisi beban tinggi.

Penerapan optimasi menggunakan Redis dan BullMQ berhasil
meningkatkan throughput dan menurunkan latency pada kedua framework.
Redis membantu mempercepat akses data melalui mekanisme in-memory datastore,
sedangkan BullMQ memindahkan proses berat ke background worker
sehingga request utama dapat diproses lebih cepat. Namun, optimasi tersebut
juga meningkatkan penggunaan resource karena adanya proses queue
dan worker tambahan. Express.js setelah optimasi menunjukkan performa
yang lebih tinggi dengan penggunaan resource yang lebih efisien.

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
