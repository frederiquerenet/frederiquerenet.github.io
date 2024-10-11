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

Copier le script pour installer DDEV sous Ubuntu dans la documentation DDEV. Attention, ce script ne fonctionne que pour une distri Ubuntu.

## Créer le projet

Lancer une session Ubuntu (cliquer sur l'icône Ubuntu). Cela ouvre un terminal pour la suite.

**Les commandes**

$ mkdir nom-projet

$ cd nom-projet

$ ddev config --project-type=drupal --docroot=web --create-docroot --php-version=8.3

Il faut absolument rajouter la v.8.3 pour php si vous comptez installer Drupal 11. Sinon c'est la version 8.2 de php qui est installée. Et ça met le bazar pour l'installation de Drush ensuite.

$ ddev start

$ ddev composer create drupal/recommended-project

Rappel : cette commande installe la dernière version (ici 11)

$ ddev composer require drush/drush

$ ddev drush site:install --site-name=MonSite --account-name=admin --account-pass=admin -y

Drush s'occupe de procéder à l'installation du site avec la création du compte super-utilisateur. J'ai mis admin pour exemple. Même mes machines locales ont des noms d'utilisateur et mots de passe sécurisés. C'est une saine habitude à avoir.

$ ddev launch

Pour ouvrir le site créé dans le navigateur par défaut.

$ ddev stop

Pour arrêter les containers, lorsqu'on n'a plus besoin de travailler sur le site.


Mon avis : une fois qu'on a les bonnes commandes, ça va très vite. Par contre, ce n'a pas aussi simple que la documentation de Drupal le laisse entendre pour le commun des mortels.
Mais c'est vrai que cela présente de vrais avantages pour avoir un environnement de développement opérationnel qui réagit bien comme un serveur de prod.

Pour rouvrir par la suite le site, il suffit de rouvrir une session Ubuntu, charger le répertoire du site et saisir les commandes ddev start, puis ddev launch.

Ne pas oubliez ddev stop une fois que vous avez terminé de travailler sur le site, pour libérer les ressources.
