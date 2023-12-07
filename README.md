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



# Tâche 2

- Créez maintenant deux réseaux avec le cidr spécifié dans les schémas. Veuillez utiliser `--subnet` pour ajouter un sous-réseau au réseau lors de sa création. Le nom des réseaux est « ynov-frontend-network » pour le conteneur du site Web frontal et « ynov-backend-network » pour le conteneur de base de données.

Astuce : pour que les deux conteneurs communiquent entre eux,

- utiliser un troisième conteneur que l'on peut appeler « passerelle » ou « routeur » et connecter ce conteneur aux deux réseaux ci-dessus.
- À l'intérieur de la passerelle du routeur, configurez la table de routage en utilisant la commande ip ci-dessous

`ip route ajouter <FROM_CIDR_REPLACE_ME> via <GATEWAY_IP_FROM_EACH_NETWORK_SIDE>`
faites la même commande pour les deux réseaux.

Remarque : L'objectif de la deuxième tâche n'est pas de déployer l'application mais de s'assurer que les conteneurs peuvent communiquer entre eux en utilisant leurs IP.

# Soumission

Vous devez effectuer le travail en équipe de 2 maximum. Aucune soumission unique n'est autorisée.

- Produisez un pdf de votre travail. Déposez toutes les captures d'écran requises qui prouvent votre travail avec des descriptions de la façon dont vous réalisez les choses
- Créez un référentiel github public et dans Moodle, soumettez uniquement l'URL de votre référentiel.
- Créez un fichier Lisez-moi propre (table des matières, sections, écrans, etc. qui montrent votre travail)
- Dans le fichier Lisez-moi, n'oubliez pas de mettre les noms des membres de votre équipe