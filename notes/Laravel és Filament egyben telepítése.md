# Laravel és Filament egyben telepítése

cd /var/www/html

mkdir <projektneve>

cd <projektmeve>

composer create-project laravel/laravel .

(!) Kitölteni az ".env" fájlban:
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=<adatbázisneve>
DB_USERNAME=root
DB_PASSWORD=root

nano ./start.sh
#! /bin/bash
clear
echo "Laravel szerver indul... c(_)"
php artisan serve --host=0.0.0.0

chmod 7777 start.sh

composer require barryvdh/laravel-debugbar --dev

(!) Beírni az ".env" fájlban: DEBUGBAR_ENABLED=true

./start.sh

php artisan migrate

composer require filament/filament:"^3.2" -W
php artisan filament:install --panels
php artisan make:filament-user

(!) Átirni az ".env" fájlban:
APP_LOCALE=en -> hu
APP_FALLBACK_LOCALE=en -> hu
APP_FAKER_LOCALE=en_US -> hu_HU

(!) Beleirni az app/Providers/FIlament/AdminPanelProvider.php 
fájlba a:

->authMiddleware([
    Authenticate::class,
]);

rész után ezt:

->maxContentWidth(\Filament\Support\Enums\MaxWidth::Full);
FONTOS, hogy a pontosvessző átkerül az új sor végére!

(!) Beleirni az app/Providers/AppServiceProvider.php 

fájlba:

use Illuminate\Database\Eloquent\Model;

public function boot(): void
{
    Model::unguard();
}

# Áttelepítés

composer install
nano .env
php artisan migrate