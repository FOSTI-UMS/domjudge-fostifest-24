# DOMjudge Setup with Docker Compose

This README provides instructions on how to set up a DOMjudge environment for Competitive Programming using Docker Compose. This setup includes services for the database (MariaDB), DOMjudge server (domserver), Judgehost, and Adminer for database management. this readme also availabe in [Bahasa](https://github.com)

## Prerequisites

- [Docker]()
  ```sh
  curl -fsSL https://get.docker.com -o get-docker.sh
  sudo sh ./get-docker.sh --dry-run
  ```
- [Docker Compose for current user](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)
  ```sh
  DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
  mkdir -p $DOCKER_CONFIG/cli-plugins
  curl -SL https://github.com/docker/compose/releases/download/v2.29.1/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
  chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
  docker compose version
  ```
- [Docker Compose for all user](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)
  ```bash
  sudo mkdir -p /usr/local/lib/docker/cli-plugins
  sudo curl -SL https://github.com/docker/compose/releases/download/v2.29.1/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose
  sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
  ```
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
   ![docker compose up -d](https://raw.githubusercontent.com/FOSTI-UMS/domjudge-fostifest-24/main/docs/images/0.png?raw=true)
4. ### Check initial domserver password

   run in your terminal (use sudo if your docker running for root user)

   ```sh
   docker compose exec domserver cat /opt/domjudge/domserver/etc/initial_admin_password.secret
   ```

   ![inital password](https://raw.githubusercontent.com/FOSTI-UMS/domjudge-fostifest-24/main/docs/images/s3.png?raw=true)
   use the initial password for login as `admin` username in http://localhost/login
   ![admin login](https://raw.githubusercontent.com/FOSTI-UMS/domjudge-fostifest-24/main/docs/images/6.png?raw=true)

5. ### Change Judgedaemon password on virtual environment `.env`
   ![.env](https://raw.githubusercontent.com/FOSTI-UMS/domjudge-fostifest-24/main/docs/images/2.png?raw=true)
6. ### Check judgehost status
   Visit http://localhost/jury/judgehosts on browser
   ![judgehost](https://raw.githubusercontent.com/FOSTI-UMS/domjudge-fostifest-24/main/docs/images/1.png?raw=true)
7. ### Restart Judgehost service

   If you just see 1 judgehost ("example-judgehost1") maybe you need to restart your service.
   In your terminal run (use sudo if your docker running for root user)

   ```sh
   docker compose down
   docker compose up -d
   ```

   access http://localhost/jury/judgehost, if still inactive run this command

   ```sh
   docker compose restart judgehost
   ```

   in still inactive try troubleshooting

8. ### Check judgehost status
   Visit http://localhost/jury/judgehosts on browser
   ![](https://raw.githubusercontent.com/FOSTI-UMS/domjudge-fostifest-24/main/docs/images/4.png?raw=true)
9. ### Manage database
   Use Adminer by open your browser and go to http://localhost:8080. Login with `MARIADB_USER` and `MARIADB_PASSWORD` on `.env` file
   ![adminer](https://raw.githubusercontent.com/FOSTI-UMS/domjudge-fostifest-24/main/docs/images/5.png?raw=true)
10. ### login as demo user
    from http://localhost/login you can use this credentials to login
    ```
    USERNAME="demo"
    PASSWORD="demo"
    ```
    ![participants user](https://raw.githubusercontent.com/FOSTI-UMS/domjudge-fostifest-24/main/docs/images/7.png?raw=true)
11. #### Stop Services

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
