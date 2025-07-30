✅ 1. Cek Loki sudah Ready
Di terminal, jalankan:

bash
Copy
Edit
docker exec -it grafana wget -qO- http://loki:3100/ready
Kalau hasilnya:

arduino
Copy
Edit
ready
Berarti Loki sehat dan siap dipakai.

✅ 2. Tambahkan Loki ke Grafana
Buka browser: http://localhost:4000

Login dengan:

Username: admin

Password: admin (default)

Klik ikon ⚙️ (Configuration) → Data Sources

Klik Add data source

Pilih Loki

Isi URL: http://loki:3100

Klik Save & Test

Kalau sukses, akan muncul tulisan “Data source is working”.

✅ 3. Test Query Log
Di Grafana, pergi ke Explore

Pilih data source: Loki

Masukkan query seperti:

arduino
Copy
Edit
{job="docker"}
atau jika tidak ada, coba:

arduino
Copy
Edit
{container="grafana"}
Klik Run query untuk melihat log.

Kalau belum muncul log:

Pastikan promtail sudah jalan dan membaca log

Cek docker logs promtail

Atau kirimkan file promtail-config.yaml ke saya agar saya bantu periksa

Silakan lanjutkan dengan langkah-langkah ini dan beri tahu jika ada error tertentu.








You said:
buatkan saya readme tentang ini


ChatGPT said:
Berikut adalah isi README.md untuk setup Grafana, Loki, dan Promtail menggunakan Docker Compose:

markdown
Copy
Edit
# 🖥️ Monitoring Log Container dengan Grafana + Loki + Promtail

Setup ini digunakan untuk menampilkan log dari container Docker ke Grafana menggunakan Loki sebagai backend dan Promtail sebagai agent pengumpul log.

---

## 📁 Struktur Folder

pintro/
├── docker-compose.yaml
├── loki-config.yaml
├── promtail-config.yaml
├── loki_data/ # data volume untuk Loki (otomatis dibuat)
└── grafana_data/ # data volume untuk Grafana (otomatis dibuat)

yaml
Copy
Edit

---

## 🚀 Cara Menjalankan

### 1. Jalankan Docker Compose

```bash
docker compose up -d
2. Cek Service
bash
Copy
Edit
docker ps
Pastikan service berikut berjalan:

grafana

loki

promtail

3. Akses Grafana
Buka browser ke: http://localhost:4000
Login default:

Username: admin

Password: admin

⚙️ Setup Loki di Grafana
Klik ⚙️ (Configuration) → Data Sources

Klik Add data source

Pilih Loki

Isi URL:

arduino
Copy
Edit
http://loki:3100
Klik Save & Test

Jika berhasil: ✅ “Data source is working”

🔍 Menampilkan Log
Pergi ke menu Explore

Pilih data source: Loki

Jalankan query log, contoh:

arduino
Copy
Edit
{container="grafana"}
atau

arduino
Copy
Edit
{job="docker"}
🛠️ Konfigurasi File
loki-config.yaml
yaml
Copy
Edit
auth_enabled: false

server:
  http_listen_port: 3100

ingester:
  lifecycler:
    ring:
      kvstore:
        store: inmemory
      replication_factor: 1
  final_sleep: 0s
  chunk_idle_period: 5m
  max_chunk_age: 1h

schema_config:
  configs:
    - from: 2023-01-01
      store: boltdb-shipper
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        period: 24h

storage_config:
  boltdb_shipper:
    active_index_directory: /loki/index
    cache_location: /loki/boltdb-cache
  filesystem:
    directory: /loki/chunks

limits_config:
  enforce_metric_name: false
  reject_old_samples: true
  reject_old_samples_max_age: 168h

chunk_store_config:
  max_look_back_period: 0s

table_manager:
  retention_deletes_enabled: true
  retention_period: 168h
promtail-config.yaml
yaml
Copy
Edit
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://loki:3100/loki/api/v1/push

scrape_configs:
  - job_name: docker
    docker_sd_configs:
      - host: unix:///var/run/docker.sock
        refresh_interval: 5s
    relabel_configs:
      - source_labels: [__meta_docker_container_name]
        target_label: container
🧹 Hentikan & Bersihkan
Untuk menghentikan:

bash
Copy
Edit
docker compose down
Untuk menghapus volume juga:

bash
Copy
Edit
docker compose down -v
📝 Catatan Tambahan
Gunakan docker logs promtail dan docker logs loki untuk debugging jika log tidak muncul.

Pastikan Docker daemon berjalan, dan file docker.sock bisa diakses oleh Promtail.

Gunakan wget http://loki:3100/ready untuk healthcheck service Loki.

📌 Referensi
Grafana Loki Docs

Promtail Configuration

Docker Compose

go
Copy
Edit
