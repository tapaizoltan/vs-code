# Laravel

# Elengedhetetlen követelmények:
================================
# (3/1 -> php -> ellenőrzés php --version)
# (3/2 -> composer -> ellenőrzés composer --version)
# (3/2 -> composer -> ellenőrzés composer --version)

# Laravel telepítése:
=====================
A telepítendő könyvtárban kiadni -> composer create-project laravel/laravel .

# Első beállítás:
=================
Szerkesztőben megnyitni a ".env" fájlt -> ez tartalmazza a laravel beállításait, itt kell beállítani a MySQL kapcsolatot

# Laravel futtatása:
====================
php artisan serve --host:0.0.0.0 (alapesetben a szerver a 8000-as porton fut, vagy adott porton: --port=80 kapcsoló használatával)

# start.sh
==========
#! /bin/bash
clear
echo "Laravel szerver indul..."
cd /var/www/html/laravel/
php artisan serve --host=0.0.0.0

# Laravel debuger:
==================
composer require barryvdh/laravel-debugbar --dev

(!) Beírni az ".env" fájlban: DEBUGBAR_ENABLED=true

# Alap adattáblák készítése:
============================
php artisan migrate