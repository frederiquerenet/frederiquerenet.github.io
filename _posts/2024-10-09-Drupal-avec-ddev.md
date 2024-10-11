---
layout: post
title:  "Installer Drupal dans un environnement local avec DDEV"
author: "Frédérique"
---

J'ai décidé de suivre les préconisations de l'équipe Drupal pour l'installation en environnement local, aec la solution DDEV.

Je travaille avec Windows 11 édition Pro.

J'avais commencé avec la doc de DDEV mais j'ai malheureusement rapidement été bloquée dans la configuration de mon projet.

Pour les webmasters débutants dans une contexte similaire, voici les étapes que j'ai suivi pour installer un site Drupal opérationnel en local avec DDEV :

## Installer WSL2 et Linux

**Mes ressources**

[installer WSL 2 | Microsoft Learn](https://learn.microsoft.com/fr-fr/windows/wsl/install)
[DDEV installation Windows](https://ddev.readthedocs.io/en/stable/users/install/ddev-installation/#windows)

Je vous recommande d'ouvrir PowerShell en mode administrateur (Windows PowerShell/Exécuter ISE en tant qu'administrateur) pour cette étape.

La commande pour installer wsl avec la distri Ubuntu par défaut : wsl --install

Le script pour installer DDEV sous Ubuntu : Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072;
iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/ddev/ddev/master/scripts/install_ddev_wsl2_docker_inside.ps1'))

N.B. Le script ne fonctionne que pour Ubuntu.

## Créer le projet

Lancer une session Ubuntu (cliquer sur l'icône Ubuntu). Cela ouvre un terminal pour la suite.

**Les commandes**

mkdir nom-projet
cd nom-projet

ddev config --project-type=drupal --docroot=web --create-docroot --php-version=8.3
Il faut absolument rajouter la v.8.3 pour php si vous comptez installer Drupal 11.
Sinon c'est la version 8.2 de php qui est installée. Et ça met le bazar pour l'installation de Drush ensuite.

ddev start
Les containers démarrent

ddev composer create drupal/recommended-project
pour installer la dernière version (ici 11)

ddev composer require drush/drush
pour installer l'outil Drush

ddev drush site:install --site-name=MonSite --account-name=admin --account-pass=admin -y
Drush s'occupe de procéder à l'installation du site

ddev launch
ouvre le site créé dans le navigateur par défaut.

ddev stop
pour arrêter les containers.


Mon avis : une fois qu'on a les bonnes commandes, ça va très vite. Par contre, ce n'a pas aussi simple que la documentation de Drupal le laisse entendre pour le commun des mortels.
Mais c'est vrai que cela présente de vrais avantages pour avoir un environnement de développement opérationnel qui réagit bien comme un serveur de prod.

Pour rouvrir par la suite le site, il suffit de rouvrir une session Ubuntu, charger le répertoire du site et saisir les commandes ddev start, puis ddev launch.

Ne pas oubliez ddev stop une fois que vous avez terminé de travailler sur le site, pour libérer les ressources.
