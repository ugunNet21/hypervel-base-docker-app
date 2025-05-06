# Hyper-App Documentation

This is a Hypervel-based application running on Docker with PHP and Swoole. The project uses Laravel-like artisan commands and is configured to run efficiently in a containerized environment. This README provides instructions for setting up, running, and managing the application.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Setup Instructions](#setup-instructions)
- [Running the Application](#running-the-application)
- [Database Migration](#database-migration)
- [Environment Configuration](#environment-configuration)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites
- **Docker** and **Docker Compose** installed on your system.
- PHP 8.3 or compatible version (handled by the Docker image).
- Composer for dependency management (installed in the container).
- A `.env` file configured with your environment variables (use `.env.example` as a template).
- Optional: MySQL or PostgreSQL if using a database (configured in `docker-compose.yml`).

## Project Structure
```
hyper-app/
├── app/                   # Application logic and models
├── bootstrap/             # Bootstrap and initialization files
├── config/                # Configuration files
├── database/              # Database migrations and seeds
├── public/                # Publicly accessible files
├── resources/             # Views, assets, and other resources
├── routes/                # Route definitions
├── storage/               # Storage for logs, cache, etc.
├── tests/                 # Test suites
├── vendor/                # Composer dependencies
├── .env                   # Environment configuration
├── .env.example           # Example environment file
├── composer.json          # Composer dependencies and scripts
├── docker-compose.yml     # Docker Compose configuration
├── artisan                # Artisan CLI tool
├── README.md              # This file
```

## Setup Instructions
1. **Clone the Repository**:
   ```bash
   git clone <repository-url> hyper-app
   cd hyper-app
   ```

2. **Copy Environment File**:
   ```bash
   cp .env.example .env
   ```
   Edit `.env` to configure your database, application settings, and other environment variables.

3. **Build and Start Docker Containers**:
   ```bash
   docker-compose up -d
   ```
   This starts the Hypervel application on port `9501`. If using a database, uncomment the relevant database service in `docker-compose.yml` and update `.env` accordingly.

4. **Install Dependencies**:
   ```bash
   docker-compose exec app composer install
   ```

- touch file atau buat file database.sqlite di /database/database.sqlite

## Running the Application
- **Development Mode**:
  The `docker-compose.yml` is configured to run `php artisan watch` for hot-reloading:
  ```bash
  docker-compose up -d
  ```
  Access the application at `http://localhost:9501`.

- **Production Mode**:
  Modify the `command` in `docker-compose.yml` to:
  ```yaml
  command: php artisan start
  ```
  Then restart the containers:
  ```bash
  docker-compose down && docker-compose up -d
  ```

## Database Migration
- **Run Migrations**:
  ```bash
  docker-compose exec app php artisan migrate
  ```
  This applies pending migrations. Example output:
  ```
  Migrating: 2023_08_03_000000_create_users_table
  Migrated:  2023_08_03_000000_create_users_table
  ```

- **Fresh Migration** (drops all tables and re-runs migrations):
  ```bash
  docker-compose exec app php artisan migrate:fresh
  ```
  Example output:
  ```
  Dropped all tables successfully.
  [INFO] Migration table created successfully.
  Migrating: 2023_08_03_000000_create_users_table
  Migrated:  2023_08_03_000000_create_users_table
  ```

- **Database Configuration**:
  Ensure your `.env` file includes the correct database settings, e.g.:
  ```
  DB_CONNECTION=mysql
  DB_HOST=db
  DB_PORT=3306
  DB_DATABASE=hypervel
  DB_USERNAME=hypervel
  DB_PASSWORD=secret
  ```
  Update `docker-compose.yml` to include the database service if not already enabled.

## Environment Configuration
- The `.env` file is loaded automatically by Hypervel.
- Key variables to configure:
  - `APP_NAME`: Application name.
  - `APP_ENV`: Environment (`local`, `production`, etc.).
  - `APP_KEY`: Run `docker-compose exec app php artisan key:generate` to set this.
  - Database credentials (see above).
- Avoid hardcoding sensitive values in `docker-compose.yml`. Use `.env` instead.

- `cp .env.docker.dev .env`
  `docker compose up -d`

- `cp .env.docker.prod .env`
  `docker compose up -d --build`

## Troubleshooting
- **Permission Issues**:
  If you encounter volume permission issues (common with SELinux), add `:Z` to the volume mount in `docker-compose.yml`:
  ```yaml
  volumes:
    - .:/data/project:Z
  ```
  Alternatively, run the container as root (not recommended for production):
  ```yaml
  app:
    privileged: true
    user: root
  ```

- **Nothing to Migrate**:
  If `php artisan migrate` shows "Nothing to migrate," ensure migration files exist in `database/migrations/`.

- **Container Logs**:
  Check logs for errors:
  ```bash
  docker-compose logs app
  ```

- **Database Connection Issues**:
  Verify the database service is running and the `.env` credentials match the `docker-compose.yml` configuration.

## Contributing
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/YourFeature`).
3. Commit changes (`git commit -m 'Add YourFeature'`).
4. Push to the branch (`git push origin feature/YourFeature`).
5. Open a pull request.

- sudo chown -R userpc:userpc storage runtime
userpc misal ugun:ugun

## 🐳 Syntax Docker

### 🔍 Perintah Umum

- `docker ps -a`  
  Melihat semua container (baik yang sedang berjalan maupun yang sudah berhenti)

- `docker images`  
  Melihat daftar image yang ada di sistem

- `docker image ls`  
  Alternatif lain untuk melihat image (sama seperti `docker images`)

- `docker system df`  
  Melihat penggunaan disk oleh Docker (image, container, volume)

- `docker image prune`  
  Menghapus image yang tidak digunakan oleh container mana pun (dangling)

- `docker system prune`  
  Menghapus semua data yang tidak digunakan (container stopped, network, image dangling)

- `docker system prune -a --volumes`  
  Menghapus semua image yang tidak digunakan, termasuk volume tidak terpakai
- `docker-compose exec app php artisan migrate:fresh`
  Menjalankan migration dan fresh

---

### ⚙️ Perintah `docker-compose`

| Perintah                 | Fungsi                                                                |
|--------------------------|-----------------------------------------------------------------------|
| `docker-compose build`   | Membangun ulang image berdasarkan Dockerfile                          |
| `docker-compose up`      | Menjalankan container dari image (membuat jika belum ada)             |
| `docker-compose down`    | Menghentikan container & menghapus container serta jaringan terkait   |


## License
This project is licensed under the MIT License. See the `LICENSE` file for details.

> See [this issue](https://github.com/laravel/octane/issues/765) for more discussions.

## Documentation

[https://hypervel.org/docs](https://hypervel.org/docs)