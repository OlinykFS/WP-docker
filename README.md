# WordPress Development with Docker

This repository provides a simple and quick way to set up a WordPress development environment using Docker. It includes WordPress, MySQL, and phpMyAdmin, allowing you to develop locally with ease.

## Prerequisites

Ensure you have the following installed:
- [Docker](https://www.docker.com/get-started)

## Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/OlinykFS/WP-docker
cd wordpress-docker
```

### 2. Set up environment variables

Create a `.env` file by copying the example file:

```env
# WordPress environment
WORDPRESS_DB_HOST=db:3306
WORDPRESS_DB_NAME=wordpress
WORDPRESS_DB_USER=user
WORDPRESS_DB_PASSWORD=password

# MySQL environment
MYSQL_DATABASE=wordpress
MYSQL_USER=user
MYSQL_PASSWORD=password
MYSQL_ROOT_PASSWORD=root_password

# Ports
WORDPRESS_PORT=8000
PHPMYADMIN_PORT=8080
```

Variables explained:
- `WORDPRESS_DB_HOST`: Host of the database service.
- `WORDPRESS_DB_NAME`: Name of the WordPress database.
- `WORDPRESS_DB_USER`: MySQL user for WordPress.
- `WORDPRESS_DB_PASSWORD`: Password for the MySQL user.
- `MYSQL_ROOT_PASSWORD`: Root password for MySQL.
- `WORDPRESS_PORT`: Port to access WordPress (default: 8000).
- `PHPMYADMIN_PORT`: Port to access phpMyAdmin (default: 8080).

### 3. Start the containers

```bash
docker-compose up -d
```

### 4. Access the services

- **WordPress**: Go to `http://localhost:8000` (or the port you set) to access WordPress.
- **phpMyAdmin**: Go to `http://localhost:8080` to access phpMyAdmin. Use the MySQL credentials from your `.env` file.

### 5. Stop the containers

To stop and remove the containers:

```bash
docker-compose down
```

## Customization

- All WordPress files are mapped to the `./wordpress` folder, allowing you to edit themes and plugins locally.
- phpMyAdmin provides an easy interface to manage the database.

## Docker Compose Configuration

Here's the `docker-compose.yml` file used for this setup:

```yaml
version: '3'

services:
  wordpress:
    image: wordpress:latest
    container_name: wordpress
    ports:
      - "${WORDPRESS_PORT}:80"
    volumes:
      - ./wordpress:/var/www/html
    environment:
      WORDPRESS_DB_HOST: ${WORDPRESS_DB_HOST}
      WORDPRESS_DB_NAME: ${WORDPRESS_DB_NAME}
      WORDPRESS_DB_USER: ${WORDPRESS_DB_USER}
      WORDPRESS_DB_PASSWORD: ${WORDPRESS_DB_PASSWORD}
    depends_on:
      - db

  db:
    image: mysql:8.0
    container_name: mysql
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    environment:
      PMA_HOST: ${WORDPRESS_DB_HOST}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    ports:
      - "${PHPMYADMIN_PORT}:80"

volumes:
  db_data:
```

This configuration sets up three services: WordPress, MySQL, and phpMyAdmin, with appropriate volumes and environment variables.
