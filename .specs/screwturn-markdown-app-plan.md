# Plan — App .NET Core de conversion ScrewTurn Wiki vers Markdown

## 1) Contexte et domaine
- Ce dépôt contient un export brut du wiki pathfinder-fr.org.
- Les pages sont majoritairement des fichiers `.txt` avec :
  - un en-tête délimité par `---`,
  - un contenu en syntaxe ScrewTurn Wiki (proche MediaWiki).
- Des snippets partagés sont appelés via `{s:...}` et référencés dans `_Snippet`.
- Objectif produit : générer des fichiers Markdown exploitables à partir de cet export.

## 2) Contraintes de cadrage
- Travailler sur une branche dédiée.
- Héberger l’application dans le dossier **`.app/`**.
- Construire une application console .NET Core.
- Utiliser `Spectre.Console` et `Spectre.Console.Cli` pour l’interface CLI.
- Conserver les fichiers `.txt` du dépôt comme source de référence.

## 3) Livrables testables (grandes étapes)

### Livrable 1 — Squelette d’application CLI opérationnel
**But**
- Disposer d’une base exécutable prête à accueillir la conversion.

**Contenu**
- Projet console .NET Core dans `.app/`.
- Wiring `Spectre.Console.Cli` avec au moins une commande racine.
- Paramètres CLI minimum (source, destination, dry-run, verbosité).

**Critères de validation**
- La commande d’aide s’affiche correctement.
- La commande accepte les arguments attendus.
- Un mode dry-run s’exécute sans écrire de fichiers.

### Livrable 2 — Lecture des sources et séparation en-tête/corps
**But**
- Charger les fichiers wiki et structurer les données d’entrée.

**Contenu**
- Parcours des fichiers ciblés (fichier unique, dossier, global).
- Extraction robuste en-tête / contenu.
- Modèle interne minimal représentant une page source.

**Critères de validation**
- Un lot d’échantillons est lu sans erreur bloquante.
- Les métadonnées d’en-tête sont récupérées correctement.
- Le contenu brut ScrewTurn est conservé sans altération.

### Livrable 3 — Conversion Markdown de base
**But**
- Produire un premier rendu Markdown utile sur les cas fréquents.

**Contenu**
- Règles de mapping initiales (titres, paragraphes, listes, emphases, liens).
- Stratégie explicite pour les éléments non supportés.

**Critères de validation**
- Les pages simples produisent un Markdown lisible.
- Les liens internes/externes restent exploitables.
- Les éléments non convertis sont signalés clairement.

### Livrable 4 — Gestion des snippets ScrewTurn
**But**
- Traiter les `{s:...}` de manière contrôlée.

**Contenu**
- Résolution des snippets supportés.
- Marquage des snippets inconnus pour enrichissements ultérieurs.
- Rapport de couverture des snippets rencontrés.

**Critères de validation**
- Les snippets supportés sont convertis de manière reproductible.
- Les snippets non supportés sont listés et traçables.

### Livrable 5 — Génération des sorties et reporting
**But**
- Écrire les fichiers Markdown et fournir un bilan d’exécution.

**Contenu**
- Écriture vers une arborescence cible stable.
- Nommage cohérent et reproductible.
- Résumé de conversion (succès, avertissements, erreurs).

**Critères de validation**
- Les fichiers sont générés aux emplacements attendus.
- Deux exécutions identiques produisent le même résultat.
- Le rapport final permet d’identifier les points à corriger.

### Livrable 6 — Qualité et validation continue
**But**
- Sécuriser les évolutions par une validation testable.

**Contenu**
- Jeu d’échantillons représentatifs (simples + riches en snippets).
- Tests unitaires sur les règles de conversion critiques.
- Revue manuelle ciblée des pages complexes.

**Critères de validation**
- Les tests passent sur les cas nominaux ciblés.
- Les régressions de mapping sont détectables rapidement.
- Les limites connues sont documentées et priorisées.

## 4) Backlog d’enrichissement
- Prioriser les syntaxes ScrewTurn à supporter en premier.
- Fixer la convention de sortie Markdown (style, front matter, chemins).
- Définir la politique de traitement des snippets métier Pathfinder-fr.
- Formaliser les critères d’acceptation de “conversion suffisante”.
