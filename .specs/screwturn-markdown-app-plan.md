# Plan — App .NET Core de conversion ScrewTurn Wiki vers Markdown

## 1) Cadre de travail
- Créer une branche dédiée pour ce chantier.
- Créer un **second dossier applicatif** dans le dépôt (ex. `tools/screwturn-markdown-exporter/`).
- Initialiser une application console .NET Core dans ce dossier.
- Conserver le dépôt actuel comme source de données d’entrée (fichiers `.txt` exportés).

## 2) Entrées / sorties
- **Entrée** : fichiers texte du wiki contenant :
  - un en-tête délimité par `---`,
  - un corps en syntaxe ScrewTurn Wiki (proche MediaWiki).
- **Sortie** : fichiers Markdown dans une arborescence miroir (ou configurable), avec nommage stable.
- Définir un mode de traitement :
  - fichier unique,
  - dossier,
  - traitement global.

## 3) Pipeline de conversion
- Lire et séparer en-tête / contenu.
- Parser les constructions ScrewTurn prioritaires :
  - titres, listes, emphases, liens internes/externes,
  - snippets `{s:...}`,
  - éléments non convertibles directement.
- Appliquer des règles de transformation vers Markdown.
- Produire un rapport des éléments partiellement convertis (pour itérations futures).

## 4) Règles de mapping (version initiale)
- Définir une table de correspondance ScrewTurn -> Markdown.
- Implémenter d’abord les structures à fort volume (titres, listes, liens, paragraphes).
- Ajouter une stratégie explicite pour les snippets :
  - substitution quand une règle existe,
  - marquage clair quand la conversion est inconnue.
- Prévoir un mécanisme de règles extensibles (fichier de configuration ou couche dédiée).

## 5) Architecture de l’app
- Organiser le code en composants séparés :
  - lecture des sources,
  - parsing,
  - conversion,
  - écriture des sorties,
  - reporting.
- Prévoir des options CLI minimales :
  - chemin source,
  - chemin destination,
  - mode dry-run,
  - niveau de verbosité.

## 6) Validation
- Constituer un jeu d’échantillons représentatifs (pages simples + pages riches en snippets).
- Vérifier la stabilité du rendu Markdown sur ces échantillons.
- Ajouter des tests unitaires sur les règles de transformation critiques.
- Prévoir une vérification manuelle ciblée des pages complexes.

## 7) Évolution progressive
- Livrer une première version utilisable avec un sous-ensemble de la syntaxe.
- Itérer ensuite par enrichissement des règles (snippets, cas limites, liens spécifiques).
- Documenter les limitations connues et la roadmap de conversion.

## 8) Points à enrichir avec tes prochaines consignes
- Priorisation des syntaxes ScrewTurn à supporter en premier.
- Convention de sortie Markdown attendue (style, front matter, chemins).
- Politique de gestion des snippets métier spécifiques au wiki Pathfinder-fr.
- Critères d’acceptation pour déclarer la conversion “suffisante”.
