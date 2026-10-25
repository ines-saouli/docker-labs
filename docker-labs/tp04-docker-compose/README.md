# TP04 - Décrire une application complète avec Docker Compose

## 🎯 Objectifs du TP
* Remplacer une suite de commandes `docker run` par un fichier d'orchestration unique : **`compose.yaml`**[cite: 17].
* Démarrer, observer et arrêter une application multi-conteneurs en une seule commande (`docker compose up -d` / `docker compose down`)[cite: 17].
* Construire des images directement depuis Compose grâce à l'instruction `build:`[cite: 17].
* Externaliser la configuration et les données sensibles dans un fichier `.env` (et l'exclure via `.gitignore` pour des raisons de sécurité)[cite: 17, 23, 31].
* Configurer des **healthchecks** pour s'assurer qu'un service (comme une base de données) est réellement prêt avant de lancer l'application dépendante (`service_healthy`)[cite: 17, 29, 31].

## 🛠️ Notions clés et Équivalences

* **Correspondances `docker run` vs `compose.yaml` :**
  * `--name db` $\rightarrow$ Nom du service sous `services:`[cite: 17]
  * `image` $\rightarrow$ `image: postgres:16-alpine`[cite: 17]
  * `-e CLE=valeur` $\rightarrow$ Section `environment:` (ou liaison avec un fichier `.env` via `${CLE}`)[cite: 17, 23]
  * `-p 8081:8080` $\rightarrow$ Section `ports:`[cite: 17]
  * `-v pgdata:/var/lib/...` $\rightarrow$ Section `volumes:` (déclaré dans le service et en bas du fichier)[cite: 17]
  * `--network labnet` $\rightarrow$ Section `networks:` (Compose crée automatiquement un réseau par défaut pour le projet)[cite: 17, 27]

* **Commandes indispensables du quotidien :**
  * Valider la syntaxe du fichier Compose : `docker compose config`[cite: 20]
  * Démarrer la pile en arrière-plan (avec reconstruction si besoin) : `docker compose up -d --build`[cite: 26]
  * Suivre les logs en direct : `docker compose logs -f`[cite: 27]
  * Arrêter la pile : `docker compose stop` ou `docker compose down`[cite: 27, 31]
  * **Attention en production :** L'option `docker compose down -v` supprime également les volumes de données. À manipuler avec précaution pour éviter d'effacer des bases de données de production[cite: 24, 31].

* **Sécurité & Bonnes pratiques :**
  * Ne **jamais** pousser de fichier `.env` contenant des mots de passe en clair sur GitHub ou GitLab (`.gitignore` obligatoire)[cite: 24, 31].
  * Utiliser les *healthchecks* couplés à `condition: service_healthy` pour éviter qu'une application ne plante au démarrage en essayant de joindre une base de données qui n'est pas encore prête à accepter des connexions[cite: 29, 31].
