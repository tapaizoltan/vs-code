# GIT Alapok
==================================
git branch -m master
git init
git config --local user.name "Tapai Zoltan"
git config --local user.email "zoltan@tapai.hu"
git config --list
git config --list --show-origin

git status
git add fájlnév.kiterjesztés vagy -all
git status
git commit -m "megjegyzés a commithoz"


# feltöltés
===========
git remote add origin <github_link>
git push origin master

# letöltés
==========
git pull origin master <- a master a branch
-vagy-
git clone <hithub_link>



git log

git diff fájlnév.kiterjesztés
git diff HEAD
git diff --cached fájlnév.kiterjesztés

Szinkronizálásból kizárt fájlok:
létre kell hozni egy .gitihnore nevű fájlt a munkakönyvtárunk gyökerében.
ebbe a fájlba beleírva azon fájlok nevét amiket nem kell követni


# Ez egy jó kiinduló pont

…or create a new repository on the command line
===============================================
echo "# laravel-gyakorlo" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/tapaizoltan/laravel-gyakorlo.git
git push -u origin main

…or push an existing repository from the command line
=====================================================
git remote add origin https://github.com/tapaizoltan/laravel-gyakorlo.git
git branch -M main
git push -u origin main