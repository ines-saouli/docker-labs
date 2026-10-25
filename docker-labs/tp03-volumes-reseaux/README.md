# TP03 - Garder ses données et faire communiquer ses conteneurs

## 🎯 Objectifs du TP
* Conserver les données des conteneurs même après leur suppression grâce aux **volumes nommés**[cite: 33].
* Utiliser les **bind mounts** pour lier un dossier de la machine hôte à un conteneur (très utile en développement pour voir les modifications de code en direct)[cite: 33, 35, 44].
* Déployer une base de données PostgreSQL dont les données persistent à travers les redémarrages et suppressions de conteneurs[cite: 33, 37].
* Créer un **réseau Docker personnalisé** (`labnet`) pour permettre aux conteneurs de communiquer entre eux par leur nom (grâce au DNS interne)[cite: 33, 40, 44].

## 🛠️ Notions clés et Comparatifs

* **Volume nommé vs Bind mount :**
  * **Volume nommé (`-v mon-volume:/data`)** : Géré entièrement par Docker, idéal pour stocker les données d'une application ou d'une base de données en production[cite: 33, 44].
  * **Bind mount (`-v $(pwd)/site:/usr/share/nginx/html:ro`)** : Pointe vers un dossier spécifique de votre machine locale. Idéal pour le développement. L'ajout de `:ro` (*read-only*) garantit que le conteneur ne peut pas modifier les fichiers source, renforçant la sécurité[cite: 33, 35, 37, 44].

* **Réseau par défaut vs Réseau personnalisé :**
  * Le réseau Docker par défaut ne fournit **pas** de DNS interne, ce qui empêche les conteneurs de se joindre par leur nom[cite: 40, 44].
  * Un réseau créé par l'utilisateur (ex: `docker network create labnet`) active un DNS interne automatique, permettant à des conteneurs (comme une application et une base de données) de se connecter proprement par leur nom d'hôte (ex: `db`)[cite: 40, 44].

* **Commandes principales manipulées :**
  * Créer un volume : `docker volume create mesdonnees`[cite: 34]
  * Créer un réseau : `docker network create labnet`[cite: 40]
  * Lancer un conteneur avec un bind mount en lecture seule : `docker run -d -p 8080:80 -v "$(pwd)/site:/usr/share/nginx/html:ro" nginx:1.30-alpine`[cite: 35]
  * Inspecter un réseau ou un volume : `docker network inspect labnet` / `docker volume inspect mesdonnees`[cite: 35, 43]
