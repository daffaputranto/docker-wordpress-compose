# WordPress dengan Docker Compose (WordPress + MySQL + Redis)

## Deskripsi

Project ini merupakan implementasi **WordPress CMS menggunakan Docker Compose** dengan arsitektur multi-container yang terdiri dari:

* **WordPress** sebagai aplikasi web
* **MySQL** sebagai database
* **Redis** sebagai object cache

Dengan Docker Compose, semua service dapat dijalankan secara bersamaan dan saling terhubung dalam satu environment.

---

## Teknologi yang Digunakan

* Docker
* Docker Compose
* WordPress
* MySQL 5.7
* Redis

---

## Struktur Project

```
wordpress-docker
│
├── docker-compose.yml
├── README.md
└── screenshots
    ├── docker-ps.png
    ├── wordpress-install.png
    ├── wordpress-dashboard.png
    └── redis-test.png
```

---

## Cara Menjalankan Project

### 1. Masuk ke folder project

```
cd wordpress-docker
```

### 2. Jalankan Docker Compose

```
docker compose up -d
```

Docker akan menjalankan 3 container:

* wordpress
* mysql
* redis

### 3. Cek container

```
docker ps
```

### 4. Akses WordPress

Buka browser:

```
http://localhost:8000
```

Lalu lakukan instalasi WordPress melalui halaman setup.

---

## Konfigurasi Redis

Tambahkan konfigurasi berikut pada file **wp-config.php**:

```
define('WP_REDIS_HOST', 'redis');
define('WP_REDIS_PORT', 6379);
```

Setelah itu install plugin **Redis Object Cache** dari dashboard WordPress.

---

## Testing Redis

Masuk ke Redis container:

```
docker exec -it wordpress-docker-redis-1 redis-cli
```

Tes koneksi:

```
ping
```

Output yang diharapkan:

```
PONG
```

---

## Jawaban Pertanyaan

**1. Kenapa perlu volume untuk MySQL?**
Volume digunakan agar data database tidak hilang ketika container dimatikan atau dihapus.

**2. Apa fungsi depends_on?**
depends_on memastikan container MySQL dijalankan terlebih dahulu sebelum WordPress.

**3. Bagaimana WordPress connect ke MySQL?**
WordPress menggunakan environment variable `WORDPRESS_DB_HOST=mysql`. Docker Compose membuat network sehingga service bisa saling terhubung menggunakan nama service.

**4. Apa keuntungan menggunakan Redis untuk WordPress?**
Redis digunakan sebagai cache sehingga dapat mempercepat loading website dan mengurangi query ke database.

---

### WordPress Installation
![install](screenshots/wordpress-install.png)

### WordPress Dashboard
![dashboard](screenshots/wordpress-dashboard.png)

### Docker Containers
![docker](screenshots/docker-ps.png)

### Redis Test
![redis](screenshots/redis-test.png)