# Spottt — projet dev (AI-assisted)

## Consignes

- Utilise le langage de ton choix pour la logique métier. Dis-nous dans le README pourquoi celui-là plutôt qu'un autre.
- **Rendu obligatoire en 3D avec WebGL** (Three.js, Babylon.js, WebGL pur — à toi de voir). Pas besoin d'UI sophistiquée, mais on veut voir les rovers se déplacer dans une scène 3D, pas juste du texte en console.
- **Un serveur MCP qui pilote le rover** (voir plus bas) : on veut pouvoir conduire les rovers depuis un agent, pas seulement depuis un fichier d'entrée.
- **Test à réaliser en AI-assisted** (Cursor, Claude Code, Copilot, ChatGPT, Windsurf — peu importe l'outil). On veut autant voir la qualité du code final que la manière dont tu pilotes tes agents. À joindre au rendu :
    - Un court README qui décrit ton workflow : quels outils IA, comment tu as découpé tes prompts, ce qui a marché et ce qui a mal tourné, ce que tu as dû corriger à la main.
    - **Les règles que tu as données à ton agent** (CLAUDE.md, AGENTS.md, règles Cursor ou équivalent), avec au moins un exemple : une erreur que l'agent répétait, et la règle que tu as écrite pour qu'il arrête.
    - L'historique de tes prompts si tu peux l'exporter (Cursor, Claude Code, chat logs) — sinon des commits bien descriptifs suffisent.
- Ça ne devrait pas te prendre plus de **2 à 3 heures** au total. **Le test est calibré pour que tu ne puisses pas tout faire** — c'est volontaire (voir section arbitrage).
- Ne t'inquiète pas si tu ne finis pas à temps, on sait que ce n'est pas forcément évident de trouver du temps libre ! Envoie-nous simplement ce que tu as fait et quelques phrases sur ce que tu aurais ajouté ensuite.
- Le rendu se fait par pull request sur le dépôt que nous avons créé pour toi (voir « Rendu » en bas de page).

---

## Le cœur du test — Rover martien

Écris un programme qui prend des commandes et déplace un ou plusieurs robots sur Mars.

- Le monde est modélisé comme une grille de taille m x n.
- Ton programme doit lire l'entrée, mettre à jour les robots, et **afficher en 3D (WebGL) la scène martienne et les déplacements des rovers**. Les états finaux doivent aussi être lisibles (dans la scène, un panneau, ou en console).
- Chaque robot a une position (x, y) et une orientation (N, E, S, W).
- Chaque robot peut avancer d'une case (F), tourner à gauche de 90° (L) ou tourner à droite de 90° (R).
- Si un robot sort de la grille, il est marqué comme "perdu" et sa dernière position et orientation valides sont enregistrées.
- Aller de x → x+1 se fait vers l'est, et y → y+1 se fait vers le nord. (0, 0) représente donc le coin sud-ouest de la grille.

### Entrée

```
4 8
(2, 3, E) LFRFF
(0, 2, N) FFLFRFF
```

La première ligne spécifie la taille de la grille. Les lignes suivantes représentent chacune l'état initial et les commandes pour un seul robot.

### Sortie attendue

```
(4, 4, E)
(0, 4, W) LOST
```

Autre exemple :

```
4 8
(2, 3, N) FLLFR
(1, 0, S) FFRLF
```

→

```
(2, 3, W)
(1, 0, S) LOST
```

### Le serveur MCP

Expose ton rover à un agent IA par un serveur MCP (Model Context Protocol).

- Depuis un client MCP (Claude Code, Claude Desktop, Cursor…), on doit pouvoir créer une grille, poser un ou plusieurs rovers, les déplacer et connaître leur état — y compris quand un rover est perdu.
- Le serveur s'appuie sur la même logique métier que le reste du programme : pas de règles réécrites à côté.
- Les outils que tu exposes, leur découpage et ce qu'ils renvoient sont à toi de décider. Explique ton choix dans le README : pourquoi ces outils-là, à ce niveau de détail.
- Donne-nous de quoi le brancher en deux minutes : la configuration du client et un exemple de demande en langage naturel qui fonctionne (par exemple « amène le rover au coin nord-est sans le perdre »).
- Que la scène 3D suive en direct ce que fait l'agent est un plus, pas une obligation. Si tu ne le fais pas, dis-nous comment tu t'y prendrais.

C'est **le minimum à livrer**. Tout ce qui suit relève de ton arbitrage.

---

## L'arbitrage produit

Tu as 2-3h. Tu ne peux pas tout faire. **On veut voir ce que tu choisis de faire, et surtout ce que tu choisis de ne pas faire, et pourquoi.**

Voici une palette d'axes d'approfondissement possibles. **Choisis-en 1 ou 2** (ou propose les tiens — tant mieux si tu sors de la liste), et **justifie tes choix dans le README** en une dizaine de lignes max : pourquoi ceux-là, qu'est-ce que tu as volontairement écarté, qu'est-ce qui aurait changé ton arbitrage si on t'avait dit "c'est pour mettre en prod demain" vs "c'est pour une démo investisseur".

**Axes possibles :**

- **Planète sphérique** — Mars devient une sphère, le rover ne sort plus jamais des limites, il tourne autour du globe. Enjeu : modélisation géométrique (lat/long ? icosaèdre ? cube-sphère ?), cohérence des commandes F/L/R sur surface courbe, gestion des pôles.
- **Rendu réaliste** — textures Mars, éclairage directionnel, skybox, caméra orbitale interactive. Enjeu : qualité visuelle et maîtrise WebGL.
- **Animation et temporalité** — déplacements animés fluides (pas téléportés), replay, pause, contrôle de vitesse. Enjeu : découpler logique métier et rendu temporel.
- **UI interactive** — saisir les commandes en live, ajouter/supprimer des rovers à la volée, reset, import/export de scénarios. Enjeu : ergonomie produit.
- **Multi-rovers temps réel** — plusieurs rovers exécutent leurs commandes en parallèle, gestion des collisions entre eux. Enjeu : modèle de concurrence.
- **Qualité et robustesse** — tests unitaires sur la logique métier, gestion propre des inputs malformés, CI qui tourne sur le repo. Enjeu : code prod-ready.
- **Terrain non trivial** — obstacles, cratères, reliefs qui bloquent ou redirigent les rovers. Enjeu : extension du modèle de jeu.

Tu peux évidemment combiner (ex : sphérique + animé), mais attention à livrer quelque chose de **fini** plutôt que trois chantiers à moitié ouverts.

---

## Ce qu'on évalue

- **Ton arbitrage** : est-ce que tes choix sont lucides et défendus — langage compris ? Est-ce que tu livres un truc cohérent et fini, ou tu t'es éparpillé ?
- **La qualité du code** : structure, séparation logique métier / rendu / serveur MCP, lisibilité. Un code propre qui fait moins vaut mieux qu'un code sale qui fait plus.
- **Ton autonomie sur les outils IA** : savoir déléguer, savoir reprendre la main, savoir vérifier, savoir cadrer un agent par des règles. On repère vite les candidats qui ont poussé du code généré sans le comprendre.
- **La conception de ton serveur MCP** : des outils qu'un agent utilise correctement du premier coup, des retours qui lui permettent de se corriger.
- **La pertinence de ta modélisation** sur les axes techniques que tu choisis (surtout si tu prends le sphérique ou le multi-rovers).

Ce qu'on **ne regarde pas** : le nombre de lignes, la beauté des textures, le nombre de features livrées. Mieux vaut livrer 1 axe bien fait que 3 axes bancals.

---

## Rendu

Le lien reçu dans notre e-mail pointe vers **un dépôt créé pour toi**, sous un nom anonyme.

1. Forke ce dépôt sur ton compte GitHub (bouton « Fork »).
2. Travaille dans ton fork, avec des commits au fil de l'eau.
3. Avant l'échéance indiquée dans notre e-mail, ouvre une **pull request de ton fork vers ce dépôt** : c'est ton rendu.

À réception, nous repassons le dépôt en privé. Pense alors à supprimer ton fork ou à le passer en privé.

Lors de l'entretien technique, tu nous présenteras tes choix, puis nous te demanderons une évolution à traiter en direct
avec ton agent, écran partagé. Garde donc ton environnement prêt.
