# TP : Mise en réseau Docker

Le "TP : Mise en réseau Docker" est un travail noté en **groupe de deux**. Les membres du groupes sont :
- SUPREME Chelson ;
- GAIO Lucas. 

Ce travail a pour objectif de faire **communiquer les deux conteneurs**. 


## Sommaire
- [Fonctionement](##Fonctionement)
  - [Fichiers](###Fichiers)
- [Consignes](##Consignes)
  - [Etape I](###Etape-I)
  - [Etape II](###Etape-II)
  - [Etape III](###Etape-III)
  - [Etape IV : rendu](###Etape-IV-:-rendu)

## Fonctionement

### Fichiers

### Création 

#### Partie 1

Connecter les conteneurs entre eux sur un même réseau.

1. Créer les deux conteneurs : `docker run --name ynov-frontend -d -p 80:80 nginx` et `docker run -d --name ynov-backend -p 8080:80 -p 443:443 -e ALLOW_EMPTY_PASSWORD=yes bitnami/prestashop` ;
2. Créer le réseau : `docker network create ynov-network` ;
3. Connecter les conteneurs au réseau : `docker network connect ynov-network ynov-frontend` puis `docker network connect ynov-network ynov-backend` ;
4. Installations, sur le conteneur : `apt update`, `apt install -y iproute2` et `apt update && apt install -y iputils-ping` ;
5. communiquer avec les noms, sur le conteneur : `ping ynov-backend` ou `ping ynov-frontend`.

#### Partie 2

1. Création du réseau : `docker network create --subnet=10.0.0.0/24 ynov-frontend-network`, `docker network create --subnet=10.0.1.0/24 ynov-backend-network` ;
2. Créer les conteneurs : 
  - Conteneur front : `docker run -it --network ynov-frontend-network --name ynov-frontend --ip 10.0.0.10 -d -p 80:80 nginx` ;
  - Conteneur back [A tester] : `docker run -e PRESTASHOP_DATABASE_PASSWORD=PASSWORD -p 8080:80 bitnami/prestashop` ;
  - Conteneur passerelle : `docker run -d --privileged --name router --network ynov-frontend-network ubuntu sleep infinity` ;
  - Vérifier que tout fonctionne correctement : `docker network inspect ynov-frontend-network`, `docker network inspect ynov-backend-network` ;
3. Faire communiquer :
4. Configurer le routeur :
  - Connecter au second réseau : `docker network connect ynov-backend-network router`
  - Aller sur le conteneur : `docker exec -it router bash` ;
  - Installations : `apt update`, `apt install -y iproute2` et `apt update && apt install -y iputils-ping` ;
  - Configurer les interfaces : `ip addr add 10.0.0.254/24 dev eth0` et `ip addr add 10.0.1.254/24 dev eth1`
  - `echo 1 > /proc/sys/net/ipv4/ip_forward`
  - Créer : `ip route add 10.0.1.10 via 10.0.1.2` et `ip route add 10.0.0.10 via 10.0.0.2` ;
  - Vérifier : `ip route` ou `route -n`.
9. Tout supprimer : 
  - Supprimer les conteneurs : `docker rm -f ynov-frontend ynov-backend router`
  - Supprimer les réseaux : `docker network rm ynov-frontend-network ynov-backend-network`


### Utilisation

## Consignes 

Déployer une application de e-commerce. Voir l'architecture ci-dessous :
![Schéma d'architecture](archi.png)

### Etape I

1. Utiliser l'[image Prestashop, disponible ici](https://hub.docker.com/r/bitnami/prestashop).
2. Création d'un fronten et d'une base de données pour stocker des données persistantes.

### Etape II

Déployer dans le même réseau.

### Etape III

Déployer dans deux réseaux différents et s'assurer qu'ils peuvent communiquer ensemble, en utilisant leur nom.

1. Créer le réseau front avec l'IP `10.0.0.0/24` du nom "ynov-frontend-network" ;
2. Créer le réseau back avec l'IP `10.0.1.0/24` du nom "ynov-backend-network" ;
3. Utiliser `--subnet`, pour ajouter un sous-réseau au réseau lors de sa création ;
4. Faire communiquer les deux conteneurs, avec un troisième conteneur "passerelle" ou "routeur" pour les connecter ;
5. Configurer la table de routage dans la passerelle en utilisant la commande `ip route ajouter <FROM_CIDR_REPLACE_ME> via <GATEWAY_IP_FROM_EACH_NETWORK_SIDE>`. Faire la même commande pour les deux réseaux.

L'objectif est de faire communiquer les deux routeurs par leurs IP, pas spécialement de faire fonctionner le site.

### Etape IV : rendu

- Groupe de 2 ;
- PDF contenant les captures d'écran des étapes et descriptions ;
- Github public, mettre le lien du gitbuh sur moodle ;
- Readme.md : table des matières, sections, écrans, etc. qui montrent votre travail ;