# TP : Mise en réseau Docker


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