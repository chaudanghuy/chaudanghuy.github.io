## How To Install and Set Up Laravel with Docker Compose

In this guide, I'll use Docker Compose to containerize a Laravel application for development. 

When you're finished, you'll have a demo Laravel application running on three separate service containers:

1. An app service running PHP 8.1
2. An db service running MYSQL 5.7

Software:
[Docker Desktop](https://docs.docker.com/docker-for-windows/install/)

---

### Steps:

#### Install Laravel Project

```command
composer create-project laravel/laravel laravel
```

#### Create Dockerfile

```command
FROM php:8.1.0-fpm

RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /app

COPY . .

RUN composer install

CMD php artisan serve --host=0.0.0.0
EXPOSE 8000
```

#### Create docker-compose.yml

**docker-compose.yml**

```yml
version: '3.8'
services:
    app
        build:
            context: .
            dockerfile: Dockerfile
        ports:
            -   8000:8000
        depends_on:
            -   db

    db
        image: mysql:5.7.22
        environment:
            MYSQL_DATABASE: laravel
            MYSQL_USER: admin
            MYSQL_PASSWORD: admin
            MYSQL_ROOT_PASSWORD: root
        volumes:
            -  ./storage/dbdata:/var/lib/mysql
        ports:
            -   33060:3306            
```


#### Update .env

```php
Change DB_HOST to db
```

#### Run Docker Compose

```command
docker-compose up -d --build
```
