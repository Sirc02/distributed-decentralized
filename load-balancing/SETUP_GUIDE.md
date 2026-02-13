# Panduan Menjalankan Load Balancing

## 📚 Pembahasan Load Balancing

### Apa itu Load Balancing?
**Load Balancing** adalah teknik distribusi beban kerja ke multiple server untuk:
- ✅ Meningkatkan performa aplikasi
- ✅ Mencegah satu server overload
- ✅ Menjamin high availability (uptime maksimal)
- ✅ Menangani traffic tinggi secara efisien

Analogi: Jika ada 1 kasir di toko dan ratusan pelanggan mengantri, toko akan lambat. Dengan 2+ kasir, pelayanan lebih cepat.

### Arsitektur Load Balancing Anda

```
┌─────────────┐
│   Browser   │
│  (Client)   │
└──────┬──────┘
       │ Request ke port 80
       ▼
┌─────────────────┐
│     Nginx       │ ← Load Balancer (Round-Robin)
│   (port 80)     │   Membagi request ke 2 instance
└──────┬──────────┘
       │
     ┌─┴────────┬──────────┐
     │           │          │
     ▼           ▼          ▼
┌─────────┐ ┌─────────┐ 
│ App 1   │ │ App 2   │  ← BlackSheep Instance
│ Port 80 │ │ Port 80 │   (Masing² di container)
└────┬────┘ └────┬────┘
     │           │
     └─────┬─────┘
           │ Response HTML
           ▼
     ┌─────────────┐
     │   Browser   │
     │ Tampil page │
     └─────────────┘
```

### Komponen Utama

#### 1. **Nginx (Load Balancer)**
- **Fungsi:** Menerima semua request client dan membaginya ke app instances
- **Strategi:** Round-Robin (request bergantian: 1→App1, 2→App2, 3→App1, dst)
- **Port:** 80 (satu-satunya yang exposed ke client)
- **Konfigurasi:** File `nginx.conf`

```nginx
upstream backend {
  server bs_app:80;  # Docker DNS resolving
}

server {
  listen 80;
  location / {
    proxy_pass http://backend;  # Forward ke backend
  }
}
```

**Keuntungan:**
- Request tidak langsung ke app (lebih aman)
- Distribusi beban otomatis
- Single entry point untuk client

#### 2. **BlackSheep (Web App Framework)**
- **Fungsi:** Framework Python untuk handle HTTP request
- **Instances:** 2 container (dipilih saat `--scale bs_app=2`)
- **Port:** Port 80 (internal, tidak exposed)
- **Routing:** Controller-based (home.py, examples.py)

**Struktur Request:**
```
HTTP Request → Controller (home.py)
    ↓
business logic (services.py)
    ↓
render template (views/home/*.jinja)
    ↓
HTML Response
```

**Endpoint yang tersedia:**
- `GET /` → `Home.index()` → views/home/index.jinja
- `GET /example` → `Home.example()` → views/home/example.jinja

#### 3. **Docker & Docker Compose**
- **Fungsi:** Container orchestration & isolation
- **Benefit:** Setiap app instance berjalan isolated, reproducible

**docker-compose.yml:**
```yaml
services:
  bs_app:
    build: ./blacksheep_lb  # Build dari Dockerfile
    # Akan di-scale jadi 2 instance saat runtime
  
  nginx:
    build: ./nginx
    ports:
      - 80:80               # Port 80 host → port 80 container
    depends_on:
      - bs_app              # Nginx tunggu app ready
```

---

## Alur Request-Response Lengkap

**Scenario User buka http://localhost dan refresh 2x:**

### Request 1 (Client buka homepage)
```
1. Browser → Nginx (http://localhost)
   Client mengirim: GET /
   
2. Nginx menerima request
   Nginx check: Request #1 → forward ke App 1
   
3. App 1 (BlackSheep Instance 1) process request
   Controller (Home.index) dijalankan
   Services.py prepare data untuk template
   
4. Template Engine (Jinja2) render
   Template: views/home/index.jinja
   + Data dari service
   = HTML final
   
5. Response kembali ke client
   Nginx → Browser
   Browser render & display page
```

### Request 2 (Client refresh page)
```
1. Browser → Nginx (http://localhost)
   Client mengirim: GET /
   
2. Nginx menerima request
   Nginx check: Request #2 → forward ke App 2 (round-robin)
   
3. App 2 (BlackSheep Instance 2) process request
   (Proses sama seperti App 1)
   
4. Response dikirim ke client
```

### Request 3 (Client refresh lagi)
```
Request #3 → kembali ke App 1
(Cycle berulang)
```

---

## Keuntungan Setup Ini

| Keuntungan | Penjelasan |
|------------|-----------|
| **High Availability** | Jika 1 app crash, masih ada 1 app lain yang melayani |
| **Better Performance** | 2 app = 2x processing power, request tidak antri lama |
| **Scalability** | Bisa easy di-scale: `--scale bs_app=3`, `--scale bs_app=5`, dst |
| **Isolation** | Setiap app di container terpisah, tidak saling menganggu |
| **Simplified Deployment** | Docker = environment sama di dev/prod |
| **No Single Point of Failure** | Load balancer + 2 app = redundancy |

---

## Teknologi yang Digunakan

| Komponen | Fungsi | Benefit |
|----------|--------|---------|
| **Nginx** | Reverse Proxy & Load Balancer | Lightweight, fast, reliable |
| **BlackSheep** | Python Web Framework | Asynchronous, clean syntax |
| **Jinja2** | Template Engine | Powerful data rendering |
| **Docker** | Container Platform | Consistent environments |
| **Round-Robin** | Load Balancing Algorithm | Simple, fair distribution |

---

## Skenario Real-World

**Tanpa Load Balancing (1 Server):**
```
Request 1 ──┐
Request 2 ──┤
Request 3 ──┼─→ Server (process satu per satu) ─→ Lambat
Request 4 ──┤
Request 5 ──┘
(Queue panjang!)
```

**Dengan Load Balancing (2+ Server):**
```
Request 1 ──┐          
Request 2 ──┤─→ Server 1 ┐
Request 3 ──┤           ├─→ Cepat (parallel processing)
Request 4 ──┼─→ Server 2 ┤
Request 5 ──┘           
(Distribusi merata!)
```

---

## Langkah-Langkah Menjalankan

### 1. Buka Terminal & Navigasi ke Folder
```bash
cd c:\Users\user\Documents\STDT\distributed-decentrarized\load-balancing
```

### 2. Build & Jalankan Docker Compose
```bash
docker-compose up --build -d --scale bs_app=2
```
**Penjelasan:**
- `--build` = Build image baru dari Dockerfile
- `-d` = Detached mode (berjalan di background)
- `--scale bs_app=2` = Membuat 2 instance aplikasi BlackSheep

### 3. Verifikasi Container Berjalan
```bash
docker ps
```
**Harapan output:** 3 container aktif
- 1× `nginx` (port 80 → load balancer)
- 2× `blacksheep_lb` (app instance 1 & 2)

### 4. Akses Aplikasi di Browser
Buka URL berikut:
- **Home Page:** http://localhost
- **Example Page:** http://localhost/example

### 5. Lihat Logs (Untuk Debug)
```bash
# Lihat logs Nginx
docker logs nginx

# Lihat logs salah satu app instance
docker logs <CONTAINER_ID>
```

### 6. Stop & Cleanup Semua Container
```bash
docker-compose down
```

---

## Tabel Referensi Cepat

| Aksi | Perintah |
|------|---------|
| **Start** | `docker-compose up --build -d --scale bs_app=2` |
| **Check Status** | `docker ps` |
| **Access App** | http://localhost |
| **View Nginx Logs** | `docker logs nginx` |
| **View App Logs** | `docker logs <CONTAINER_ID>` |
| **Stop All** | `docker-compose down` |

---

## Struktur Folder

```
load-balancing/
├── docker-compose.yml      (Konfigurasi Docker)
├── env.sh                  (Environment variables)
├── blacksheep_lb/          (Aplikasi Python - BlackSheep)
│   ├── app/
│   │   ├── controllers/    (Tempat logic HTTP request)
│   │   ├── views/          (Template HTML - Jinja2)
│   │   └── services.py     (Business logic)
│   ├── Dockerfile
│   └── requirements.txt
└── nginx/                  (Load Balancer)
    ├── Dockerfile
    └── nginx.conf          (Konfigurasi routing)
```

---

## Catatan Penting

- **Port 80** = Nginx (yang user akses)
- **Round-robin** = Request 1 → App1, Request 2 → App2, Request 3 → App1, dst
- **Jinja2** = Template engine untuk rendering HTML
- Aplikasi berjalan dalam **Docker container** (isolated environment)
