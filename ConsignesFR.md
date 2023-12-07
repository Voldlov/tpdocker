4. Faire communiquer les deux conteneurs, avec un troisième conteneur "passerelle" ou "routeur" pour les connecter ;
5. Configurer la table de routage dans la passerelle en utilisant la commande `ip route ajouter <FROM_CIDR_REPLACE_ME> via <GATEWAY_IP_FROM_EACH_NETWORK_SIDE>`. Faire la même commande pour les deux réseaux.

L'objectif est de faire communiquer les deux routeurs par leurs IP, pas spécialement de faire fonctionner le site.

# Mise en réseau Docker

L'image prestashop disponible [Image Prestashop disponible ici](https://hub.docker.com/r/bitnami/prestashop) peut être utilisée pour déployer une application e-commerce. Cette application utilise deux composants. un site fronten et une base de données pour stocker des données persistantes.

## Architecture

![Schéma d'architecture](archi.png)

## Tache 1

Déployez cette application dans un réseau. Assurez-vous que les deux conteneurs peuvent communiquer entre eux en utilisant leurs noms.

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