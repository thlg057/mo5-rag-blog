# 🌐 Thomson MO5 - Blog & Front-end (Hugo)

Ce dépôt contient l'interface utilisateur et le moteur de rendu statique du site **retrocomputing-ai.cloud**.

---

## 🏗️ Rôle du Dépôt

Ce projet est la coquille d'affichage du système.  
Il utilise le moteur de site statique **Hugo** pour transformer de la documentation technique en un site web moderne et accessible.

---

## 🧱 Stack Technique

- **Générateur** : Hugo  
- **Langage de balisage** : Markdown (`.md`)  
- **Déploiement** : Orchestré par un **Makefile externe** qui assure la liaison avec le serveur RAG

---

## 📂 Structure des fichiers

- `themes/`  
  Contient le design et les layouts du site.

- `static/`  
  Assets (images, schémas, téléchargements).

- `config.toml` (ou `.yaml`)  
  Configuration des menus, du SEO et des paramètres du site.

- `content/`  
  Répertoire destiné à recevoir les fichiers Markdown lors du build final.

---

## 🔄 Flux de données

Ce dépôt est conçu pour être **alimenté lors du déploiement** :

1. Les sources du thème sont récupérées via Git  
   *(méthode similaire à l'installation du SDK MO5)*

2. Les fichiers de connaissances techniques (Knowledge Base) sont injectés dans le dossier `content/`.

3. La commande `hugo` compile le tout pour générer le site final.

---

## 🛠️ Développement Local

```bash
# 1. Cloner le projet avec ses thèmes
git clone --recursive https://github.com/thlg057/mo5-rag-blog.git

# 2. Lancer le serveur de prévisualisation
hugo server -D
```