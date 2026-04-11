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
- Thomson
- Motorola
- 6809
- C
- wibe coding
title: "Bienvenue sur retrocomputing-ai.cloud"
translationKey: "index-1"
---

# Redonner vie à la documentation vintage

Quand on parle de préservation du patrimoine informatique, on pense souvent au matériel, aux cartes mères qu'on ressoude, aux cassettes qu'on dumpe et qu'on sauvegarde pour la postérité.

Mais il manque souvent quelque chose d'essentiel : **la connaissance**.

Comment retrouver, sans fouiller pendant 30 minutes, la syntaxe exacte d'une routine assembleur 6809 ?  
Ou encore cette astuce obscure pour compiler un programme C sur MO5 ?

C'est exactement pour ça que j'ai lancé **retrocomputing-ai.cloud** : un serveur RAG (*Retrieval-Augmented Generation*) pensé pour aider à faire revivre les machines… en les rendant **de nouveau programmables**.

---

## Ma démarche : une aide au développement très ciblée

Pour l'instant, je me concentre volontairement sur un seul terrain de jeu : 👉 **le Thomson MO5**

Le BASIC est déjà très bien couvert partout.  
En revanche, dès qu'on touche :

- au **C**
- à l'**assembleur**
- aux détails bas niveau du 6809

…les informations deviennent éparpillées, parfois contradictoires, et souvent difficiles à retrouver.

Et puis soyons honnêtes : j'ai un MO5 sous la main… ça aide 😄

L'idée est simple :

> poser une question en langage naturel,  
> et laisser le RAG retrouver la bonne page, le bon extrait, la bonne 
> explication.

### Ce que contient la base de connaissances

La base documentaire couvre plusieurs couches, bien séparées pour éviter de tout mélanger :

**📄 Documentation officielle MO5**  
Les PDFs de référence sur le hardware, le système, le BASIC — les sources primaires, numérisées et indexées.

**⚙️ CMOC**  
Une documentation dédiée au compilateur C pour 6809, que j'ai synthétisée dans un fichier Markdown pour la rendre plus exploitable par les IA.

**🛠️ SDK MO5**  
Tous les fichiers `.h` du SDK sont documentés en Markdown (prototypes, descriptions, exemples). Ils sont injectés dans le RAG dans une catégorie séparée, et installés automatiquement si vous partez du template (`make install`).

**📚 How-to et patterns**  
Des guides que j'ai rédigés sur les techniques spécifiques au jeu vidéo sur MO5 : VBL, compression RLE, détection de collisions, optimisations mémoire… Le genre de trucs qu'on ne trouve nulle part, ou qu'on redécouvre à la dure après trois soirées perdues 😄

À terme, j'aimerais étendre la plateforme à d'autres machines mythiques : Commodore 64, Apple II, Amstrad CPC…

---

## Une toolchain complète pour développer sur MO5 en 2025

Le serveur RAG ne vit pas seul. Il fait partie d'un écosystème que j'ai construit brique par brique :

### 🛠️ SDK MO5
Une bibliothèque C optimisée pour [CMOC](http://perso.b2b2c.ca/~sarrazip/dev/cmoc.html), avec des abstractions spécifiques au MO5 : gestion des sprites, du mode graphique, des entrées, etc.  
👉 [github.com/thlg057/sdk_mo5](https://github.com/thlg057/sdk_mo5)

### 📦 MO5 Project Template
Un template de projet prêt à l'emploi avec un Makefile automatisé.  
3 commandes dans un GitHub Codespace, et vous compilez votre premier programme C pour MO5 — sans rien installer localement.  
👉 [github.com/thlg057/mo5_template](https://github.com/thlg057/mo5_template)

### 🤖 Serveur MCP (ce site)
Le pont entre votre coding agent et la base de connaissances MO5.  
Branchez-le à Claude Desktop, Cursor ou Augment, et votre IA devient soudain **spécialiste du MO5**.  
👉 [npmjs.com/@thlg057/mo5-rag-mcp](https://www.npmjs.com/package/@thlg057/mo5-rag-mcp)

---

## Un mot sur la partie technique (sans trop rentrer dans les câbles)

Le serveur repose sur un pipeline RAG assez classique, mais pensé pour rester **autonome** et **peu coûteux**.

Aujourd'hui :

- j'utilise un **modèle d'embeddings neuronal local**
- les documents sont transformés en vecteurs
- une base vectorielle fait la recherche rapide
- une API .NET orchestre l'ensemble et renvoie les passages pertinents

Pourquoi ce choix ?

- **coût quasi nul**,  
- indépendance,  
- et la possibilité d'itérer rapidement.

Le modèle a ses limites, il lui manque parfois un peu de sens et de compréhension sémantique, et les résultats ne sont pas toujours optimaux. Mais il fait le job, et surtout : pas de frais supplémentaires. Juste mon hébergement VPS, point.

Quel que soit le moteur, l'objectif reste le même : fournir une réponse utile *et sourcée*, plutôt qu'une hallucination convaincante.

---

## Utiliser l'index MO5 dans votre IDE en 2 minutes

Grâce au protocole **MCP**, votre IA peut se brancher sur ma base de connaissances **sans installation complexe**.

Le serveur MCP est publié sur npm — pas besoin de cloner quoi que ce soit. Ajoutez simplement ceci dans votre fichier de configuration MCP (Claude Desktop, Cursor, Augment…) :

```json
{
  "mcpServers": {
    "mo5-rag": {
      "command": "npx",
      "args": ["-y", "@thlg057/mo5-rag-mcp"],
      "env": {
        "RAG_BASE_URL": "https://retrocomputing-ai.cloud"
      }
    }
  }
}
```

C'est tout. Pas de `git clone`, pas de `npm install`, pas de chemin à configurer. `npx` télécharge et lance le serveur à la volée. 🎉

Le serveur MCP est également référencé dans le **registre officiel MCP** (`registry.modelcontextprotocol.io`) — il est donc découvrable directement depuis GitHub Copilot et les autres clients compatibles.

---

## Et la suite ?

Le projet avance plutôt **par expérimentations** que par grandes annonces planifiées.

Je continue à compléter la documentation au fil de mes découvertes, parfois en corrigeant de vieux malentendus, parfois en ajoutant des exemples plus clairs. Côté développement, le SDK gère maintenant les sprites et le mode graphique, j'ai même réussi à faire tirer une fusée en vibe coding 🚀

Il reste quelques briques à poser pour finaliser le Space Invaders :la gestion du joystick, le son, et les inévitables optimisations qui viennent quand quelqu'un d'autre que soi essaie le truc 😄

Mais dans l'ensemble, la toolchain tient la route. Ce qui était encore un chantier il y a quelques mois est devenu quelque chose d'utilisable, et même d'amusant.

Je prendrai donc le temps, régulièrement, de mettre à jour la documentation.  
Tous mes fichiers **Markdown** sont disponibles avec les sources du serveur RAG, rien n'est caché, et tout peut être relu, amélioré, complété.

Et si vous avez envie de contribuer, d'apporter des documents, des correctifs, des idées : **faites-moi signe.** Tout seul, je ne pourrai pas faire vivre durablement ce service, et j'aimerais vraiment qu'il devienne un outil utile pour tous les passionnés de retro-computing.

Enjoy 😄

---

## Pour aller plus loin

- 📖 Blog (FR) : <https://thlg057.github.io/mo5-blog/>
- 📖 Blog (EN) : <https://thlg057.github.io/mo5-blog/en/>
- 💾 Sources du RAG server : <https://github.com/thlg057/mo5-rag-server>
- 🎬 Vidéo — développer sur MO5 en 3 commandes : <https://youtu.be/R9TeEZwZZpI>
