# Pengaturan DOMjudge dengan Docker Compose

README ini memberikan instruksi tentang cara mengatur lingkungan DOMjudge menggunakan Docker Compose. Setup ini mencakup layanan untuk database (MariaDB), server DOMjudge (domserver), Judgehost, dan Adminer untuk manajemen database.

## Prasyarat

- Docker dan Docker Compose terinstal di komputer Anda.
- File `.env` yang dikonfigurasi dengan variabel berikut (salin dan konfigurasi dari file `.env.example`):

  ```env
  DOMJUDGE_VERSION="latest"
  MARIADB_USER="user"
  MARIADB_PASSWORD="password"
  MARIADB_DATABASE="domjudge"
  MARIADB_ROOT_PASSWORD="password"
  JUDGEDAEMON_USERNAME="admin"
  JUDGEDAEMON_PASSWORD="some_random_password"
  ```

## Ikhtisar Layanan

- db: Layanan database MariaDB.
- adminer: Layanan Adminer untuk manajemen database.
- domserver: Layanan server DOMjudge.
- judgehost: Layanan Judgehost DOMjudge.

# Instruksi Setup

1. #### Clone the Repository

   ```sh
   git clone https://github.com/FOSTI-UMS/domjudge-fostifest-24.git
   cd https://github.com/FOSTI-UMS/domjudge-fostifest-24.git
   ```
2. #### Buat File .env
    Buat file .env di root direktori proyek Anda dan isi variabel lingkungan yang diperlukan seperti yang ditunjukkan di bagian Prasyarat.
3. #### Mulai Layanan
   ```
   docker compose up -d
   ```
4. #### Akses DOMjudge dan Adminer
    - Antarmuka Web DOMjudge: Buka browser Anda dan pergi ke http://localhost.
    - localhost:8080.
5. #### Hentikan Layanan
   ```
   docker compose down
   ```

   atau hapus volume

   ```
   docker compose down -v
   ```

# Detail Konfigurasi

1.  #### MariaDB (db)
    - Image: mariadb
    - Ports: Mengekspos port 3306 untuk koneksi MySQL.
    - Volumes: mariadb_data untuk menyimpan data database.
2.  #### Adminer
    - Image: adminer
    - Ports: Mengekspos port 8080 untuk antarmuka web Adminer.interface.
3.  #### DOMjudge Server (domserver)
    - Image: domjudge/domserver:${DOMJUDGE_VERSION}
    - Ports: Mengekspos port 80 untuk antarmuka web DOMjudge.
    - Volumes: domserver_data untuk menyimpan konfigurasi DOMjudge.configuration.
4.  #### Judgehost
    - Image: domjudge/judgehost:${DOMJUDGE_VERSION}
    - Mode Privileged: Diaktifkan untuk memberikan izin yang diperlukan.
    - Volumes:
        - /sys/fs/cgroup untuk sistem kontrol grup.
        - judgehost_data untuk menyimpan data Judgehost.

# Pemecahan Masalah

```sh
docker compose logs <service-name>
```
Ganti <service-name> dengan db, domjudge, judgehost, atau adminer.


