# DOMjudge Setup with Docker Compose

This README provides instructions on how to set up a DOMjudge environment for Competitive Programming using Docker Compose. This setup includes services for the database (MariaDB), DOMjudge server (domserver), Judgehost, and Adminer for database management. this readme also availabe in [Bahasa](https://github.com)

## Prerequisites

- Docker and Docker Compose installed on your machine.
- A `.env` file configured with the following variables (copy and configure from `.env.example` file):

  ```env
  DOMJUDGE_VERSION="latest"
  MARIADB_USER="user"
  MARIADB_PASSWORD="password"
  MARIADB_DATABASE="domjudge"
  MARIADB_ROOT_PASSWORD="password"
  JUDGEDAEMON_USERNAME="admin"
  JUDGEDAEMON_PASSWORD="some_random_password"
  ```
## Services Overview

- db: MariaDB database service.
- adminer: Adminer service for database management.
- domserver: DOMjudge server service.
- judgehost: DOMjudge judgehost service.

# Setup Instructions

1. #### Clone the Repository

   ```sh
   git clone https://github.com/FOSTI-UMS/domjudge-fostifest-24.git
   cd https://github.com/FOSTI-UMS/domjudge-fostifest-24.git
   ```

2. #### Create .env File
   Create a .env file in the root of your project directory and fill in the necessary environment variables as shown in the Prerequisites section.
3. #### Start Services
   ```
   docker compose up -d
   ```
4. #### Access DOMjudge and Adminer

   - DOMjudge Web Interface: Open your browser and go to http://localhost.
   - Adminer: Open your browser and go to http://localhost:8080.

5. #### Stop Services

   ```
   docker compose down
   ```

   delete with volume

   ```
   docker compose down -v
   ```

# Configuration Details

1.  #### MariaDB (db)
    - Image: mariadb
    - Ports: Exposes port 3306 for MySQL connections.
    - Volumes: mariadb_data to persist database data.
2.  #### Adminer
    - Image: adminer
    - Ports: Exposes port 8080 for the Adminer web interface.
3.  #### DOMjudge Server (domserver)
    - Image: domjudge/domserver:${DOMJUDGE_VERSION}
    - Ports: Exposes port 80 for the DOMjudge web interface.
    - Volumes: domserver_data to persist DOMjudge configuration.
4.  #### Judgehost
    - Image: domjudge/judgehost:${DOMJUDGE_VERSION}
    - Privileged Mode: Enabled to allow necessary permissions.
    - Volumes:
      - /sys/fs/cgroup for system control groups.
      - judgehost_data to persist Judgehost data.

# Troubleshooting

```sh
docker compose logs <service-name>
```
change <service-name> with service db, domjudge, judgehost, or adminer.
