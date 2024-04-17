# Docker alapparancsok
======================

# CheatSheet
============

Letöltött Doker image-ek listálya: docker image ls
Docker image keresése: docker search image_név
Docker image telepítése: docker pull image_név
Docker image törlése: docker rmi image_id
Docker file felépítés: docker build . -t adj_egy_nevet (pl.: docker build . -t fullstack/nginx)
Bejelentkezés BASH-el futó konténerbe: docker exec -it CONTAINER_ID bash


# Docker konténer felépítése:
=============================

Kétféle képpen lehet Docker konténert felépíteni.

# (1/1 -> Már kész image használatával (DockerHub))
docker run image_neve:verzio_szama -> Letölti a kívánt image-et. Ha nincs verziószám akkor elég csak az image nevét megadni.

# (1/2 -> Saját image létrhozásával)

# (1/2/1 -> docker fájl létrehozása)
Létre kell hozni egy "DockerFile" nevű fájlt.
Ebbe kell elhelyezni az alábbiakat:
...
Létre kell hozni egy ".dockerignore" nevű fájlt.
Ebbe kell elhelyezni az alábbiakat:
...

# (1/2/2 -> docker image felépítése)
docker build . -t adj_egy_nevet (pl.: docker build . -t fullstack)


# Docker konténer első indítása:
================================

docker run -p saját_host_port:távoli_host_port futtatni_kívánt_image -> docker run -p 80:8000 fullstack

    -vagy-

Először hozz létre egy mappát a "c:/developing_source/htdocs".

docker run -d -v saját_mappa:távoli_mappa -p saját_host_port:távoli_host_port futtatni_kívánt_image -> 
docker run -d -v c:/developing_source/htdocs:/var/www/html -p 80:8000 fullstack

docker stop

# Már futó konténerek listája:
==============================
docker ps

# Minden elérhető konténer listája:
===================================
docker ps -a

# Docker konténer futtatása / leállítása:
=========================================

docker start CONTAINER_ID -> A CONTAINER_id-ból elég az első 3-4 karakter, ami egyedileg már azonosítja a kiválasztott konténert.

# Konténer logok megtekintése:
==============================

docker logs CONTAINER_ID -> Eddig létrejött logok megtekintése
docker logs -f CONTAINER_ID -> Valós idejű log monitorozás

# Konténerek össze-bridge-elése:
================================

docker network create egy_név_a_networknek
pl.: docker network create fullstack-mysql

- ezt lehet ellnőrizni -> docker network ls

!Fontos: Az, hogy melyik konténer melyik konténerhez tartozzon, azt a konténer létrehozásakor kell megadni, szóval konténerek el kell dobni a docker rm CONTAINER_ID CONTANIER_ID

# Volume-ok kezelése
docker volume list
docker volume create <VOLUME_NEVE>
docker volume rm <VOLUME_NEVE>

# Volume-ok backup-olása
docker save --output <FÁJL_NEVE.zip> <VOLUME_NEVE>
pl.: docker save --output fullstack.zip fullstack


# Végső verzió
==============
docker run --name fullstack -d -v c:/developing_source/htdocs:/var/www/html -p 80:8000 --network fullstack-mysql fullstack
docker run --name mysql -d -v c:/developing_source/databases:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 --network fullstack-mysql mysql:8.3

docker run --name fullstack -d -v fullstack:/var/www/html -p 80:8000 --network fullstack-mysql fullstack
docker run --name mysql -d -v databases:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 --network fullstack-mysql mysql:latest