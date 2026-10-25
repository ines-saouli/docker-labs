# TP01 - Mes premiers conteneurs

## 🎯 Objectifs du TP
* Télécharger des images Docker et lancer des conteneurs (en mode interactif ou en arrière-plan)[cite: 64].
* Publier des ports pour accéder à un serveur web (Nginx) depuis la machine hôte[cite: 64].
* Gérer le cycle de vie des conteneurs (logs, exécution de commandes avec `exec`, arrêt et suppression)[cite: 64].
* Comprendre le caractère **éphémère** des conteneurs : les modifications faites à l'intérieur d'un conteneur disparaissent lorsqu'on le supprime[cite: 64].

## 🛠️ Commandes et Manipulations clés
* **Lancer un conteneur interactif (Bash) :** `docker run -it --name mon-ubuntu ubuntu:24.04 bash`[cite: 65]
* **Lancer un serveur web en arrière-plan :** `docker run -d --name web -p 8080:80 nginx:1.30-alpine`[cite: 68]
* **Consulter les logs :** `docker logs web`[cite: 69]
* **Entrer dans un conteneur en cours d'exécution :** `docker exec -it web sh`[cite: 71]
* **Différence entre `stop`, `rm` et persistance :** Un conteneur arrêté (`stop`) conserve ses données et peut être relancé, mais un conteneur supprimé (`rm`) efface sa couche inscriptible[cite: 72, 73, 74].
