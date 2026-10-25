# TP05 - Sécurité des conteneurs : auditer, corriger, durcir

## 🎯 Objectifs du TP
* Identifier les erreurs de sécurité courantes dans un `Dockerfile` (exécution en root, secrets en clair, copie inutile de fichiers sensibles comme `.env`, image de base obsolète ou trop lourde)[cite: 2, 3, 5].
* Utiliser **Hadolint** pour l'analyse statique des bonnes pratiques d'écriture du Dockerfile[cite: 2, 4].
* Utiliser **Trivy** pour scanner les vulnérabilités connues (CVE) et comparer l'impact d'une image complète vs des images minimalistes (`-slim`, `alpine`)[cite: 2, 7].
* Produire un **SBOM** (Software Bill of Materials) au format CycloneDX pour lister et auditer tous les composants d'une image[cite: 2, 12, 13].
* Durcir l'exécution d'un conteneur (**Hardening**) : système de fichiers en lecture seule (`--read-only`), suppression des privilèges Linux (`--cap-drop ALL`), options de sécurité (`no-new-privileges`) et limitation stricte des ressources (RAM, CPU, PIDs)[cite: 2, 10].
* (Bonus) Signer et vérifier une image avec **Cosign** pour garantir son intégrité et son origine[cite: 14].

## 🛠️ Notions clés et Bonnes pratiques de Sécurité

* **1. Audit et Correction d'un Dockerfile :**
  * **Privilèges :** Par défaut, un conteneur tourne souvent en `root`. Un attaquant qui exploite une faille obtient tous les droits dans le conteneur[cite: 3]. Il faut créer un utilisateur dédié non-privilégié (`useradd` + `USER appuser`)[cite: 7, 8].
  * **Secrets :** Ne jamais stocker de mots de passe ou de clés d'API dans des variables d'environnement (`ENV`) ou dans des fichiers copiés (`COPY`), car ils persistent en clair dans l'historique de l'image (`docker history`)[cite: 3, 5]. Les passer à l'exécution ou via des montages sécurisés[cite: 5, 9].
  * **Fichiers ignorés :** Utiliser un fichier `.dockerignore` pour empêcher l'intégration de fichiers sensibles (`.env`, `.git`) dans l'image finale[cite: 7, 8].
  * **Image de base :** Privilégier des images minimalistes (ex: `python:3.12-slim` ou Alpine) pour réduire drastiquement la surface d'attaque et le nombre de CVE[cite: 5, 7].

* **2. Outils d'analyse :**
  * **Hadolint :** Analyse statique du code du Dockerfile (détecte les mauvaises pratiques de syntaxe et de structure)[cite: 4, 5].
  * **Trivy :** Scanner de vulnérabilités (CVE) sur les paquets système et applicatifs d'une image[cite: 5, 6]. *Attention : le montage du socket Docker (`/var/run/docker.sock`) dans Trivy donne l'équivalent d'un accès root sur la machine hôte ; à n'utiliser qu'avec des outils de confiance[cite: 2, 7].*
  * **SBOM (CycloneDX) :** Inventaire complet des composants (bibliothèques Python, paquets Debian) permettant de savoir instantanément si l'application est touchée en cas d'annonce d'une faille critique (comme sur OpenSSL)[cite: 13, 14].

* **3. Durcissement à l'exécution (Hardening avec `docker run`) :**
  * `--read-only` : Monte le système de fichiers racine en lecture seule pour empêcher un attaquant de modifier le code source ou de déposer des malwares (on associe `--tmpfs /tmp` si l'app a besoin d'écrire temporairement)[cite: 10, 11].
  * `--cap-drop ALL` : Supprime toutes les *capabilities* Linux pour brider les pouvoirs spéciaux du conteneur[cite: 10, 12].
  * `--security-opt no-new-privileges:true` : Interdit l'élévation de privilèges à l'intérieur du conteneur[cite: 10].
  * `--memory`, `--cpus`, `--pids-limit` : Plafonnent la consommation de ressources pour contrer les attaques par déni de service (DoS) ou d'épuisement de processus[cite: 10].
