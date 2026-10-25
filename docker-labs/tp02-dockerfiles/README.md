# TP02 - Construire ses propres images avec un Dockerfile

## 🎯 Objectifs du TP
* Écrire un `Dockerfile` pour automatiser la création d'images personnalisées[cite: 46].
* Comprendre le fonctionnement des couches et optimiser le **cache de build** en ordonnant bien les instructions[cite: 46, 62].
* Utiliser un fichier `.dockerignore` pour exclure les fichiers inutiles et sécuriser les builds[cite: 55, 62].
* Maîtriser la différence entre `CMD` et `ENTRYPOINT`[cite: 46, 57].
* Réduire drastiquement la taille d'une image grâce au **build multi-stage** (passage de ~480 Mo à ~13 Mo)[cite: 46, 58, 60].
* Pousser et tester une image sur un registry local[cite: 46, 61].

## 🛠️ Notions clés et Bonnes pratiques

* **Les instructions essentielles d'un Dockerfile :**
  * `FROM` : Définit l'image de départ (toujours en premier)[cite: 46].
  * `WORKDIR` : Spécifie le dossier de travail à l'intérieur du conteneur[cite: 46].
  * `COPY` : Copie des fichiers de la machine hôte vers l'image[cite: 46].
  * `RUN` : Exécute des commandes *pendant* la construction de l'image (ex: installation de dépendances)[cite: 46, 62].
  * `ENV` / `EXPOSE` : Définissent des variables d'environnement et documentent les ports écoutés[cite: 46].
  * `CMD` vs `ENTRYPOINT` : `ENTRYPOINT` fixe la commande principale de base, tandis que `CMD` fournit des arguments par défaut qui peuvent facilement être remplacés en ligne de commande[cite: 46, 57].

* **Optimisation du cache de build :**
  Les instructions qui changent le moins souvent (comme l'installation des dépendances via `requirements.txt`) doivent être placées *en haut* du Dockerfile, et le code source qui change souvent *en bas*. Cela évite de réinstaller toutes les dépendances à chaque modification mineure du code[cite: 62].

* **Build Multi-Stage (Exemple avec Go) :**
  Permet d'utiliser une image lourde contenant le compilateur dans une première étape (build), puis de copier uniquement l'exécutable final dans une image de base vide (`FROM scratch`) pour l'exécution. Résultat : une image minuscule, ultra-rapide et beaucoup plus sûre (zéro outil superflu, surface d'attaque minimale)[cite: 58, 60, 62].
