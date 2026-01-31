---
date: 2026-01-04
description: Pourquoi j'ai construit un serveur RAG dédié au MO5 et comment l'utiliser via MCP dans Copilot ou Augment.
draft: false
tags:
- Retrocomputing
- AI
- RAG
- MCP
- MO5
- NodeJS
title: "Bienvenue sur retrocomputing-ai.cloud"
translationKey: "index-1"
---

# Redonner vie à la documentation vintage

Quand on parle de préservation du patrimoine informatique, on pense souvent au matériel, aux cartes mères qu'on ressoude, aux cassettes qu'on dumpe et qu'on sauvegarde pour la postérité.

Mais il manque souvent quelque chose d'essentiel : **la connaissance**.

Comment retrouver, sans fouiller pendant 30 minutes, la syntaxe exacte d'une routine assembleur 6809 ?\
Ou encore cette astuce obscure pour compiler un programme C sur MO5 ?

C'est exactement pour ça que j'ai lancé **retrocomputing-ai.cloud** : un serveur RAG (*Retrieval-Augmented Generation*) pensé pour aider à faire revivre les machines... en les rendant **de nouveau programmables**.

## Ma démarche : une aide au développement très ciblée

Pour l'instant, je me concentre volontairement sur un seul terrain de jeu : 👉 **le Thomson MO5**

Le BASIC est déjà très bien couvert partout.\
En revanche, dès qu'on touche :

-   au **C**
-   à l'**assembleur**
-   aux détails bas niveau du 6809

...les informations deviennent éparpillées, parfois contradictoires, et souvent difficiles à retrouver.

Et puis soyons honnêtes : j'ai un MO5 sous la main... ça aide 😄

L'idée est simple :

> poser une question en langage naturel,\
> et laisser le RAG retrouver la bonne page, le bon extrait, la bonne explication.

À terme, j'aimerais étendre la plateforme à d'autres machines mythiques : Commodore 64, Apple II, Amstrad CPC...

## Un mot sur la partie technique (sans trop rentrer dans les câbles)

Le serveur repose sur un pipeline RAG assez classique, mais pensé pour rester **autonome** et **peu coûteux**.

Aujourd'hui :

-   j'utilise un **modèle d'embeddings neuronal local**
-   les documents sont transformés en vecteurs
-   une base vectorielle fait la recherche rapide
-   une API .NET orchestre l'ensemble et renvoie les passages pertinents

Pourquoi ce choix ?

👉 **coût quasi nul**,\
👉 indépendance,\
👉 et la possibilité d'itérer rapidement.

À terme, je basculerai probablement sur des embeddings générés par des modèles IA plus récents (meilleure compréhension, meilleure
robustesse), mais pour l'instant, j'aime bien cette approche « pas de frais surprise ».

Quelque soit le moteur, l'objectif reste le même : fournir une réponse utile *et sourcée*, plutôt qu'une hallucination convaincante.

## Utiliser l'index MO5 directement dans votre IDE (Copilot, Augment, Claude...)

Grâce au protocole **MCP (Model Context Protocol)**, votre IA peut se brancher **comme si de rien n'était** sur ma base de connaissances... et devenir soudain **spécialiste du MO5**.

### 1️⃣ Installation du serveur MCP

1.  **Cloner le dépôt :**

``` bash
git clone https://github.com/thlg057/mo5-mcp-server.git
cd mo5-mcp-server
```

2.  **Préparation**

Assurez-vous d'avoir **Node.js** installé.

Sous Linux/macOS, pensez à rendre `index.js` exécutable :

``` bash
chmod +x index.js
```

### 2️⃣ Configuration du client MCP

Ajoutez ceci dans le fichier JSON de configuration (Copilot, Augment, Claude Desktop, etc.) :

``` json
{
  "mcpServers": {
    "mo5-rag": {
      "command": "node",
      "args": ["C:\\votre\\chemin\\vers\\mo5-mcp-server\\index.js"],
      "env": {
        "RAG_BASE_URL": "https://retrocomputing-ai.cloud"
      }
    }
  }
}
```

L'URL **retrocomputing-ai.cloud** pointe vers mon API .NET : elle gère la recherche vectorielle, assemble la réponse et sert
d'interface entre votre IDE et les documents.

Le serveur MCP, lui, fait le pont "proprement", sans bricolage.

## Et la suite ?

Le projet avance plutôt **par expérimentations** que par grandes annonces planifiées.

Je continue à compléter la documentation au fil de mes découvertes, parfois en corrigeant de vieux malentendus, parfois en ajoutant des exemples plus clairs. Côté développement, je pense avoir bien apprivoisé le mode **texte**, mais le mode **graphique** est une autre histoire : plus subtil, plus exigeant... et je suis encore en train d'apprendre à le dompter.

Je prendrai donc le temps, régulièrement, de mettre à jour la documentation.\
D'ailleurs, tous mes fichiers **Markdown** sont disponibles avec les sources du serveur RAG, rien n'est caché, et tout peut être relu, amélioré, complété.

Et si vous avez envie de contribuer, d'apporter des documents, des correctifs, des idées : **faites-moi signe.** Tout seul, je ne pourrai pas faire vivre durablement ce service et j'aimerais vraiment qu'il devienne un outil utile pour tous les passionnés de retro-computing.

Enjoy 😄

## Pour aller plus loin

Si vous voulez suivre mon aventure autour du retro-computing, je raconte tout sur mon blog :  
Version française : https://thlg057.github.io/mo5-blog/  
Version anglaise : https://thlg057.github.io/mo5-blog/en/  
Sources du RAG server : https://github.com/thlg057/mo5-rag-server
