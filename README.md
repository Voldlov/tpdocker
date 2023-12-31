# TP : Mise en réseau Docker

Le "TP : Mise en réseau Docker" est un travail noté en **groupe de deux**. Les membres du groupes sont :
- SUPREME Chelson ;
- GAIO Lucas. 

Ce travail a pour objectif de faire **communiquer les deux conteneurs**. 


## Sommaire
- [Fichiers](##Fichiers)
- [Utilisation](##Utilisation)
- [Partie 1](##Partie-1)
- [Partie 2](##Partie-2)
- [Architecture](##Architecture)
- [Futur](##Futur)


## Fichiers

Ce dossier contient plusieurs documents comme :
- Archi.png : image de l'architecture à faire ;
- README.md : document de procédure et d'explication du projet ;
- ConsignesEN.md : Document réunissant les consignes en anglais ;
- ConsignesFR.md : Document réunissant les consignes en français ;
- Rendu.pdf : Document de rendu. 

## Utilisation

- Lancer les conteneurs : `docker start ynov-frontend ynov-backend router` ;
- Arrêter les conteneurs : `docker stop ynov-frontend ynov-backend router` ;

## Partie 1

Connecter les conteneurs entre eux sur un même réseau.

1. Créer les deux conteneurs : `docker run --name ynov-frontend -d -p 8080:80 -p 443:443 -e ALLOW_EMPTY_PASSWORD=yes prestashop/prestashop` et `docker run -d -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 3306:3306 mariadb` ;
2. Créer le réseau : `docker network create ynov-network` ;
3. Connecter les conteneurs au réseau : `docker network connect ynov-network ynov-frontend` puis `docker network connect ynov-network ynov-backend` ;
4. Installations, sur le conteneur : `apt update`, `apt install -y iproute2` et `apt update && apt install -y iputils-ping` ;
5. communiquer avec les noms, sur le conteneur : `ping ynov-backend` ou `ping ynov-frontend`.

- Supprimer les conteneurs : `docker rm -f ynov-frontend ynov-backend` ;
- Supprimer les réseaux : `docker network rm ynov-network`.

## Partie 2

1. Création du réseau : `docker network create --subnet=10.0.0.0/24 ynov-frontend-network`, `docker network create --subnet=10.0.1.0/24 ynov-backend-network` ;
2. Créer les conteneurs :
    - Frontend : `docker run --privileged -it --network ynov-frontend-network --name ynov-frontend --ip 10.0.0.10 -d -p 8080:80 -p 443:443 -e ALLOW_EMPTY_PASSWORD=yes prestashop/prestashop` ;
    - Backend : `docker run --privileged -it --network ynov-backend-network --name ynov-backend --ip 10.0.1.10 -d -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 3306:3306 mariadb` ;
    - Router : `docker run -d --privileged --name router --network ynov-frontend-network ubuntu sleep infinity` puis faire `docker network connect ynov-backend-network router`
3. Vérifier le bon fonctionnement : `docker network inspect ynov-frontend-network`, `docker network inspect ynov-backend-network` ;
4. Configurer le routeur : 
    - Interface conteneur : `docker exec -it router bash` ;
    - Installations : `apt update`, `apt install -y iproute2` et `apt update && apt install -y iputils-ping` ;
    - Créer le répertoire : `ip route add 10.0.1.10 via 10.0.1.2` et `ip route add 10.0.0.10 via 10.0.0.2` ;
    - Vérifier : `ip route`.
5. Ping entre les conteneurs (ici, version depuis le frontend, inverser pour l'autre) :
    - Aller dans le conteneur : `docker exec -it ynov-frontend /bin/bash` et `docker exec -it ynov-backend /bin/bash` ;
    - Télécharger les dépendances : `apt update`, `apt install -y iproute2` et `apt update && apt install -y iputils-ping` ;
    - Référencer l'autre conteneur : `ip route add 10.0.1.0/24 via 10.0.0.2` et `ip route add 10.0.0.0/24 via 10.0.1.2`.
    - Sortir et ping : `docker exec -it ynov-frontend ping 10.0.1.10` et `docker exec -it ynov-backend ping 10.0.0.10`

- Supprimer les conteneurs : `docker rm -f ynov-frontend ynov-backend router`
- Supprimer les réseaux : `docker network rm ynov-frontend-network ynov-backend-network`

## Architecture

![Schéma d'architecture](archi.png)

## Futur

Je vais convertir tout les lignes ci-dessus en un fichier docker compose, pour automatiser le tout.