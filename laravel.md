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

## Produkce - přesun z DEV
```shell
# `.env` ...
APP_ENV=production
APP_DEBUG=false

# Optimalizace autoloader
composer dump-autoload -o
```
