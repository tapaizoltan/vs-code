# Composer

# Composer telepítése:
======================
# (2/1)
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'edb40769019ccf227279e3bdd1f5b2e9950eb000c3233ee85148944e555d97be3ea4f40c3c2fe73b22f875385f6a5155') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"

# (2/2)
sudo mv composer.phar /usr/local/bin/composer

# Composer ellenőrzése:
=======================
composer --version

Composer használata:

(5/1)
Nyitni egy mappát a webszerveren és belelépni

(5/2)
composer create-project laravel/laravel .

(5/3)
Telepítés sikerességének ellenőrzése és a laravel szerver futtatása:
php artisan serve --port=8000

(5/4)
Laravel konfiguráció:
szerkesszük a gyökérben lévő ".env" fájlt, itt is a leginkább a "DB_CONNECTION" részt.

(5/5)
Létrehozunk egy adatbázist a phpmyadmin-al, lehetőleg azt amit beállítottunk a ".env" fájlban.
Majd kiadjuk az alábbi parancsot:
php artisan migrate