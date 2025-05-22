# COURS DE GIT

## Configuration initiale

	```
		git config --global user.name "Votre Nom"
		git config --global user.email "votre.email@example.com"
	
	```
## Pour vérifier la configuration :
		`git config --list`


## Les commandes Git essentielles


	```
		git status
		git add fichier.txt
		git commit -m "Message du commit"
		git log     											# Voir l'historique des commits
		git log --oneline  										# vue compacte
		git log --graph    										# avec visualisation des branches
		git checkout -- fichier.txt    							# Annuler les modifications locales (non indexées)
		git clone https://github.com/utilisateur/projet.git
		git push origin main
		git pull origin main
		git checkout -b nouvelle-fonction 
		git remote add origin https://github.com/utilisateur/projet.git	
		git remote -v				
	```

## Git rebase 



	```
		git checkout feature/login 
		git rebase main
		git rebase --abort   #  Si vous voulez annuler le rebase : 
	
	```

main:        A -- B -- C (HEAD)  
feature/login:   A -- D -- E (HEAD)  
👉 Problème : Votre branche feature/login est en retard sur main.
Vous voulez intégrer les changements de main dans feature/login sans créer un commit de merge.

Git va :

	1. Retirer temporairement D et E
	2. Appliquer B et C depuis main
	3. Rejouer D et E par-dessus

main:        A -- B -- C  
feature/login:   A -- B -- C -- D' -- E' (HEAD)  

 Cas Pratique : Résoudre un Conflit Pendant Rebase  Si Git détecte un conflit :

Il s’arrête et affiche le fichier en conflit. Vous devez :

	- Modifier le fichier pour résoudre le conflit
	- Ajouter les changements **git add fichier**
	- Continuer le rebase  **git rebase --continue**

## Git Merge - Explication Complète

git merge est une commande Git qui fusionne les changements d'une branche dans une autre, créant un nouveau commit de fusion (merge commit) qui combine les historiques des deux branches.

🔄 Comment ça marche ?
Vous êtes sur une branche (ex: main)
Vous fusionnez une autre branche (ex: feature/login)
Git combine les modifications et crée un commit spécial


	```
		it checkout main       					# Se placer sur la branche de destination
		git merge feature/login 				# Fusionner la branche feature/login dans main
		git merge --no-ff feature				# Utiliser --no-ff pour forcer un commit de merge explicite:
		git diff main..feature      			# Vérifier les différences avant de merger:    		
	
	```