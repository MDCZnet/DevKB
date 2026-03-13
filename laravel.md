# LARAVEL

## Components
```shell
# create component
php artisan make:component layout --view
```

## Views
```shell
# create view
php artisan make:view zakazky

# `web.php` example...
use Illuminate\Support\Facades\Route;

Route::permanentRedirect('/', '/vykresy');
// Route::redirect('/', '/vykresy');

Route::view('/vykresy', 'vykresy');
Route::view('/zakazky', 'zakazky');
```

## Tailwind
```shell

# Instalace NPM
npm install

# Start webserveru pro sledování změn
npm run dev
```

## Instalace
```shell
composer create-project laravel/laravel .

# `.env` ...
APP_ENV=local
APP_DEBUG=true
SESSION_DRIVER=file

# Clear chache and optimize  
php artisan config:clear
php artisan optimize        
```

## Závislosti
```shell
# Huge Icons
# https://blade-ui-kit.com/blade-icons > Hugeicons Free
composer require afatmustafa/blade-hugeicons

# Laravel Boost
# https://laravel.com/ai/boost
composer require laravel/boost --dev
```

## TEST

### Localhost
- SMAZAT `vendor`
- SMAZAT `node_modules`
- SMAZAT `storage/logs`
- SMAZAT `storage/framework/views`

### Docker
- VYTVOŘIT `Dockerfile`
```shell
FROM php:8.3-fpm

# Instalace systémových závislostí
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Instalace PHP rozšíření (pro Laravel nezbytné)
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Instalace Composeru přímo z oficiálního obrazu
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Nastavení pracovního adresáře
WORKDIR /var/www

# Zkopírování projektu (zatím bez vendor)
COPY . .

# Nastavení práv pro složku storage (klasický problém v Dockeru)
RUN chown -R www-data:www-data /var/www/storage /var/www/bootstrap/cache

EXPOSE 9000
CMD ["php-fpm"]
```

### Docker
- VYTVOŘIT `docker-compose.yml`
```shell
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: laravel-app
    restart: unless-stopped
    volumes:
      - .:/var/www
    networks:
      - laravel-network

  nginx:
    image: nginx:alpine
    container_name: laravel-nginx
    restart: unless-stopped
    ports:
      - "8000:80"
    volumes:
      - .:/var/www
      - ./docker/nginx/conf.d:/etc/nginx/conf.d
    networks:
      - laravel-network

  db:
    image: mysql:8.0
    container_name: laravel-db
    restart: unless-stopped
    environment:
      MYSQL_DATABASE: ${DB_DATABASE}
      MYSQL_ROOT_PASSWORD: ${DB_PASSWORD}
    networks:
      - laravel-network

networks:
  laravel-network:
    driver: bridge
```

## PROD
```shell
# `.env` ...
APP_ENV=production
APP_DEBUG=false

# Optimalizace autoloader
composer dump-autoload -o

# 
```
