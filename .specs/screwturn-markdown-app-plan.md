# Plan — App .NET Core de conversion ScrewTurn Wiki vers Markdown

## 1) Contexte et domaine
- Ce dépôt contient un export brut du wiki pathfinder-fr.org.
- Les pages sont majoritairement des fichiers `.txt` avec :
  - un en-tête délimité par `---`,
  - un contenu en syntaxe ScrewTurn Wiki (proche MediaWiki).
- Des snippets partagés sont appelés via `{s:...}` et référencés dans `_Snippet`.
- Objectif produit : générer des fichiers Markdown exploitables par **Markdig** pour alimenter la génération d’un site statique.

## 2) Contraintes de cadrage
- Travailler sur une branche dédiée.
- Héberger l’application dans le dossier **`.app/`**.
- Construire une application console .NET Core.
- Utiliser `Spectre.Console` et `Spectre.Console.Cli` pour l’interface CLI.
- Conserver les fichiers `.txt` du dépôt comme source de référence.
- Générer la sortie dans un dossier **`.out/`** conçu pour pouvoir devenir un repository Git versionnable et pushable.

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
- Produire un premier rendu Markdown compatible Markdig sur les cas fréquents.

**Contenu**
- Règles de mapping initiales (titres, paragraphes, listes, emphases, liens) en privilégiant le Markdown le plus simple à rendre ensuite.
- Analyse et heuristiques de conversion pour choisir la forme Markdown la plus stable selon les cas ScrewTurn rencontrés.
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
- Possibilité d’introduire une balise/extension Markdown dédiée et un plugin Markdig pour conserver le comportement des snippets quand la conversion directe n’est pas suffisante.
- Marquage des snippets inconnus pour enrichissements ultérieurs.
- Rapport de couverture des snippets rencontrés.

**Critères de validation**
- Les snippets supportés sont convertis de manière reproductible.
- Les snippets non supportés sont listés et traçables.

### Livrable 5 — Génération des sorties et reporting
**But**
- Écrire les fichiers Markdown et fournir un bilan d’exécution.

**Contenu**
- Écriture vers le dossier `.out/` avec une arborescence cible stable.
- Nommage cohérent et reproductible.
- Résumé de conversion (succès, avertissements, erreurs).
- Préparation du répertoire `.out/` pour un usage Git (structure propre, fichiers versionnables, régénération déterministe).

**Critères de validation**
- Les fichiers sont générés aux emplacements attendus.
- Deux exécutions identiques produisent le même résultat.
- Le rapport final permet d’identifier les points à corriger.

### Livrable 6 — Validation finale orientée site statique Markdig (dernière étape)
**But**
- Valider que la sortie Markdown `.out/` est directement exploitable pour une génération de site statique basée sur Markdig.

**Contenu**
- Jeu d’échantillons représentatifs (simples + riches en snippets).
- Tests unitaires sur les règles de conversion critiques.
- Revue manuelle ciblée des pages complexes.
- Vérification de rendu via Markdig (extensions/plugins retenus, y compris plugin snippet si implémenté).

**Critères de validation**
- Les tests passent sur les cas nominaux ciblés.
- Les régressions de mapping sont détectables rapidement.
- Le rendu Markdig des pages testées est exploitable pour le site statique cible.
- Le dossier `.out/` peut être versionné et poussé comme repository dédié.
- Les limites connues sont documentées et priorisées.

## 4) Backlog d’enrichissement
- Prioriser les syntaxes ScrewTurn à supporter en premier.
- Fixer la convention de sortie Markdown (style, front matter, chemins).
- Définir la politique de traitement des snippets métier Pathfinder-fr.
- Formaliser les critères d’acceptation de “conversion suffisante”.
