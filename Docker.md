# Note de cours de Docker

## 🧱 1. Introduction à Docker
🔹 Qu’est-ce que Docker ?
Docker est un outil qui permet de créer, déployer et exécuter des applications dans des conteneurs.
Un conteneur = une unité logicielle légère, portable et auto-suffisante.

🔹 Pourquoi Docker ?
   1. Évite le "ça marche sur ma machine".
   2. Léger comparé aux machines virtuelles.
   3. Simplifie les déploiements, CI/CD, tests, ML, etc.

--

🔹 Concepts clés 

| Terme        | Description |
|--------------|-------------|
| **Image**    | Blueprint d’un conteneur, contenant tout le nécessaire pour exécuter une application |
| **Conteneur**| Instance en cours d’exécution d’une image |
| **Dockerfile** | Script utilisé pour construire une image Docker |
| **Docker Hub** | Registre public (ou privé) d’images Docker |
| **Volume**     | Espace persistant pour les données, indépendant du cycle de vie du conteneur |
| **Network**    | Réseau virtuel permettant la communication entre conteneurs |s


---
🔹 Commandes de base 

```
docker pull hello-world          # Récupérer une image
docker run hello-world           # Exécuter un conteneur
docker ps                        # Voir les conteneurs actifs
docker ps -a                     # Voir tous les conteneurs
docker images                    # Voir les images
docker stop <conteneur>   		 # Arrêter un conteneur
docker start <conteneur>  		 # Redémarrer un conteneur arrêté
docker restart <conteneur> 		 # Redémarrer un conteneur
docker rm <id>                   # Supprimer un conteneur
docker rmi <image_id>            # Supprimer une image
docker build -t nom:tag . 		 # Construire une image depuis un Dockerfile
docker push nom:tag       		 # Pousser une image vers un registry
```
---

## Interaction avec les conteneurs
```
docker exec -it <conteneur> /bin/bash  # Ouvrir un shell dans un conteneur en cours d'exécution
docker logs <conteneur>                # Afficher les logs d'un conteneur
docker logs -f <conteneur>             # Suivre les logs en temps réel
docker cp <conteneur>:chemin local     # Copier des fichiers depuis/dans un conteneur
docker inspect <conteneur>      
```
---

##  Nettoyage

```
docker system prune       # Nettoyer les conteneurs, réseaux et images inutilisés
docker volume prune       # Supprimer les volumes inutilisés
docker network prune      # Supprimer les réseaux inutilisés
```
---
##  Réseau et volumes

```
docker network ls         # Lister les réseaux
docker volume ls         # Lister les volumes
docker stats             # Afficher l'utilisation des ressources
```
---

## Exemples pratiques

```
docker run -d -p 8080:80 --name mon_nginx nginx 				# Lancer un serveur Nginx
docker run -it ubuntu /bin/bash						#	Lancer un conteneur interactif 
docker build -t mon_app .  							#  Construire et lancer une app personnalisée :
docker run -d -p 5000:5000 mon_app					#  Suite de Construire
docker system prune -a --volumes					# Nettoyer Docker 
docker run -d -v $(pwd)/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=secret mysql   # Monter un volume pour persister les données Stocke les données MySQL dans ./data sur l'hôte
docker logs -f mon_conteneur						# Inspecter les logs d'un conteneur et  -f : Follow (logs en temps réel)
docker cp mon_fich.txt mon_conteneur:/chemin/destination 				#  Copier un fichier vers/depuis un conteneur
docker cp mon_conteneur:/chemin/fichier.txt ./
docker run -d -e "NODE_ENV=production" -e "API_KEY=12345" node-app						#  Lancer un conteneur avec des variables d'environnement
docker build -t mon_app:1.0 -f Dockerfile.prod .								# -t : Tag (nom:version), -f : Fichier Dockerfile spécifique
docker system prune -a --volumes						# Nettoyer Docker (espace disque), Supprime conteneurs/images/volumes inutilisés
docker run -d -p 8080:80 -p 3000:3000 mon_app 					# Mapper plusieurs ports
```
--- 

###   Lancer PostgreSQL avec persistance
```
docker run -d -p 5432:5432 \
  -v pg_data:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=secret \
  postgres:13

```
--- 
## Connecter des conteneurs via un réseau

```
docker network create mon_reseau
docker run -d --net mon_reseau --name db redis
docker run -d --net mon_reseau -e "DB_HOST=db" mon_app
```
--- 
## Lancer un conteneur avec une limite de mémoire

```
docker run -d -m 512m --memory-swap 1g mon_app
```
--- 

## Sauvegarder/Charger une image

```
docker save -o mon_image.tar mon_app:1.0
docker load -i mon_image.tar
```
## Débugger un conteneur qui crash

---
```
docker run -it --entrypoint /bin/sh mon_image
```
Options courantes :
- -d : Détacher (lancer en arrière-plan)
- -p 8080:80 : Porter forwarding (hôte:conteneur)
- --name mon_conteneur : Nommer le conteneur
- -v /chemin/local:/chemin/conteneur : Monter un volume
- -e VAR=value : Définir une variable d'environnement
- --rm : Supprimer automatiquement après arrêt


##  Configuration optimale pour Python en conteneur
```
ENV PYTHONDONTWRITEBYTECODE 1  # Pas de .pyc
ENV PYTHONUNBUFFERED 1         # Logs immédiats

```


## CMD vs ENTRYPOINT - Commande par défaut
CMD : Commande par défaut qui peut être écrasée
ENTRYPOINT : Commande de base qui n'est pas écrasée

Bonnes pratiques :

   - Préférer les images officielles
   - Utiliser des tags spécifiques (pas latest)
   - Choisir des images légères (-alpine, -slim)