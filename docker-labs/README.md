# 🐳 Docker Security & Lab - Bachelor 3 Cybersécurité

Ce dépôt regroupe mes Travaux Pratiques (TP) réalisés dans le cadre du module **Docker et Sécurité des Conteneurs** à La Plateforme. 

L'objectif est d'adopter une démarche **"Security-by-Design"** : comprendre le fonctionnement des conteneurs, auditer leurs failles, et appliquer les bonnes pratiques de durcissement (*hardening*).

## 📂 Structure du Projet

* **TP01** : Premiers pas, cycle de vie, gestion des ports et conteneurs éphémères.
* **TP02** : Construction d'images (Dockerfiles), cache de build, `.dockerignore` et builds *multi-stage*.
* **TP03** : Persistance des données (volumes nommés, bind mounts) et cloisonnement réseau.
* **TP04** : Orchestration avec Docker Compose, externalisation des secrets (`.env`), healthchecks.
* **TP05** : Sécurité, audit statique (Hadolint), scan de vulnérabilités (Trivy), SBOM et durcissement avancé.
