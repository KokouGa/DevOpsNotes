# Note de cours de Docker

## 🧱 1. Introduction à Docker
🔹 Qu’est-ce que Docker ?
Docker est un outil qui permet de créer, déployer et exécuter des applications dans des conteneurs.
Un conteneur = une unité logicielle légère, portable et auto-suffisante.

🔹 Pourquoi Docker ?
   1. Évite le "ça marche sur ma machine".
   2. Léger comparé aux machines virtuelles.
   3. Simplifie les déploiements, CI/CD, tests, ML, etc.

🔹 Concepts clés 

| Terme        | Description |
|--------------|-------------|
| **Image**    | Blueprint d’un conteneur, contenant tout le nécessaire pour exécuter une application |
| **Conteneur**| Instance en cours d’exécution d’une image |
| **Dockerfile** | Script utilisé pour construire une image Docker |
| **Docker Hub** | Registre public (ou privé) d’images Docker |
| **Volume**     | Espace persistant pour les données, indépendant du cycle de vie du conteneur |
| **Network**    | Réseau virtuel permettant la communication entre conteneurs |