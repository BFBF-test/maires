# 📰 Archives Maire-Info

Outil d'archivage et de consultation des articles du quotidien [Maire-Info](https://maire-info.com), édité par l'Association des Maires de France (AMF).

**Application autonome** (fichier HTML unique) mise à jour automatiquement chaque jour ouvré via GitHub Actions.

## Fonctionnalités

- **Archivage automatique** : récupération quotidienne des articles via le flux RSS
- **Recherche plein texte** : filtrage instantané par mots-clés dans les titres et descriptions
- **Filtrage par rubrique** : Polices municipales, Élections, Commerce, Agriculture, etc.
- **Filtrage par période** : 7 jours, 30 jours, 3 mois
- **Sélection individuelle** : cocher les articles à exporter
- **7 formats d'export** : JSON, CSV, Markdown, OPML, Bookmarks HTML, BibTeX, XML/RSS
- **Interface responsive** : fonctionne sur ordinateur, tablette et mobile
- **Impression** : mise en page optimisée pour l'impression

## Formats d'export

| Format | Extension | Usage |
|--------|-----------|-------|
| **JSON** | `.json` | Intégrations, développement |
| **CSV** | `.csv` | Tableurs (Excel, Google Sheets) — séparateur `;` |
| **Markdown** | `.md` | Documentation, notes |
| **OPML** | `.opml` | Lecteurs RSS (Feedly, Inoreader) |
| **Bookmarks** | `.html` | Import navigateurs (Chrome, Firefox) |
| **BibTeX** | `.bib` | Références bibliographiques (Zotero, Mendeley) |
| **XML/RSS** | `.xml` | Format structuré, syndication |

## Mise à jour automatique

Le dépôt utilise **GitHub Actions** pour se synchroniser automatiquement :

- **Chaque jour ouvré à 7h30** (heure de Paris) : récupération du flux RSS
- **Déclenchement manuel** : possible via l'onglet "Actions" → "Run workflow"

Le flux de travail :

1. Récupère le flux RSS de `http://www.maire-info.com/rss/`
2. Parse les articles (titre, description, URL, date)
3. Récupère la rubrique de chaque article depuis sa page
4. Fusionne avec les articles déjà archivés (dédoublonnage par ID)
5. Met à jour le fichier `index.html` avec les nouvelles données
6. Commit et publie automatiquement

## Structure du projet

```
archives-maire-info/
├── index.html                    # Application complète (données embarquées)
├── README.md                     # Documentation
└── .github/
    └── workflows/
        └── update-data.yml       # Workflow de mise à jour automatique
```

## Installation

### Prérequis

- Un compte GitHub

### Étapes

1. **Créer un nouveau dépôt GitHub** (ou forker celui-ci)

2. **Déposer les fichiers** dans le dépôt :
   - `index.html` à la racine
   - `update-data.yml` dans `.github/workflows/`

3. **Activer GitHub Pages** :
   - Settings → Pages
   - Source : **Deploy from a branch**
   - Branch : `main` / `root`

4. **Configurer les permissions Actions** :
   - Settings → Actions → General
   - Workflow permissions : **Read and write permissions**

5. **Lancer une première mise à jour** :
   - Onglet Actions → "Mise à jour quotidienne des articles Maire-Info"
   - Cliquer sur "Run workflow"

6. L'application est accessible à : `https://VOTRE-USERNAME.github.io/NOM-DU-REPO/`

### Intégration dans un autre site (optionnel)

```html
<iframe 
  src="https://VOTRE-USERNAME.github.io/archives-maire-info/" 
  width="100%" 
  height="800" 
  frameborder="0" 
  style="border-radius:12px; box-shadow:0 4px 20px rgba(0,0,0,0.1);">
</iframe>
```

## Configuration

### Modifier la fréquence de mise à jour

Dans `.github/workflows/update-data.yml`, modifier le `cron` :

```yaml
schedule:
  - cron: '30 6 * * 1-5'  # Lundi-vendredi à 7h30 (Paris)
```

### Modifier le nombre maximum d'articles archivés

Dans le workflow, modifier `MAX_ARTICLES` :

```javascript
const MAX_ARTICLES = 500;  // Par défaut : 500 articles
```

### Récupération historique

Pour remplir les archives avec des articles plus anciens, vous pouvez modifier temporairement le workflow pour scanner les IDs d'articles séquentiellement (les IDs sont numériques et incrémentaux : `article.asp?param=30468`, etc.).

## Fonctionnement technique

L'application repose sur le même principe que l'outil [export-lectures-partagees](https://github.com/uneIAparjour/export-lectures-partagees/) :

- **Pas de serveur** : tout est dans un seul fichier HTML
- **Pas de dépendance** : JavaScript vanilla, CSS natif, aucune librairie externe
- **Pas de CORS** : la récupération des données se fait côté serveur (GitHub Actions), pas côté navigateur
- **Données embarquées** : le tableau `ARTICLES` dans le HTML contient toutes les données
- **Git comme base de données** : chaque mise à jour est un commit, l'historique est traçable

## Licence

Ce projet est partagé sous licence [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.fr).

Les contenus des articles restent la propriété de l'AMF / Maire-Info.

## Assistance

Outil développé avec l'assistance de Claude (Anthropic).
