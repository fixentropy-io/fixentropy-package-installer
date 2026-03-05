# @fixentropy-io/package-installer

📦 Gestionnaire de téléchargement, extraction et installation de packages pour les projets fixentropy

## 🎯 Description

`@fixentropy-io/package-installer` est une bibliothèque TypeScript qui facilite la découverte, le téléchargement et l'installation de packages depuis un registre npm. Elle permet de :

- 🔍 Rechercher des projets localement dans un répertoire de cache
- 📥 Télécharger automatiquement les packages depuis un registre npm
- 📦 Extraire les archives `.tar.gz` téléchargées
- ✅ Vérifier l'intégrité des packages via checksum
- 🔌 Installer automatiquement les dépendances
- 🚀 Charger dynamiquement les modules téléchargés

## 📦 Installation

```bash
bun add @fixentropy-io/package-installer
```

```bash
npm install @fixentropy-io/package-installer
```

```bash
pnpm add @fixentropy-io/package-installer
```

```bash
yarn add @fixentropy-io/package-installer
```

## 🚀 Utilisation

### Exemple de base

```typescript
import { lookupForProjects } from '@fixentropy-io/package-installer';

const projectsRegistryUrl = 'https://registry.npmjs.org/@mon-organisation';
const localRegistryPath = './cache/packages';
const projectNames = ['projet-a', 'projet-b', 'projet-c'];

// Recherche et installation automatique des projets
const projects = await lookupForProjects<MonType>(
    projectsRegistryUrl,
    localRegistryPath,
    projectNames
);

// Les projets sont maintenant disponibles
console.log(`${projects.length} projets chargés`);
```

### Recherche locale uniquement

```typescript
import { findProjectLocally } from '@fixentropy-io/package-installer';

const project = await findProjectLocally<MonType>(
    './cache/packages',
    'mon-projet'
);

if (project.isSome()) {
    console.log('Projet trouvé localement !');
    const projectData = project.unwrap();
}
```

### Installation manuelle d'un projet

```typescript
import { installFor } from '@fixentropy-io/package-installer';

const project = await installFor<MonType>(
    'https://registry.npmjs.org/@mon-organisation',
    './cache/packages',
    'mon-projet'
);

if (project) {
    console.log('Projet installé avec succès !');
}
```

## 📚 API

### `lookupForProjects<T>(projectsRegistryUrl, localRegistryPath, projectNames)`

Fonction principale qui recherche des projets localement, et les télécharge/installe si nécessaire.

**Paramètres:**
- `projectsRegistryUrl` (string): URL du registre npm (ex: `https://registry.npmjs.org/@organisation`)
- `localRegistryPath` (string): Chemin local où les packages sont stockés
- `projectNames` (string[]): Liste des noms de projets à charger

**Retour:** `Promise<T[]>` - Liste des projets chargés

### `findProjectLocally<T>(localRegistryPath, projectName)`

Recherche un projet dans le cache local.

**Paramètres:**
- `localRegistryPath` (string): Chemin du répertoire de cache
- `projectName` (string): Nom du projet à rechercher

**Retour:** `Promise<Maybe<T>>` - Instance du projet si trouvé

### `installFor<T>(projectsRegistryUrl, localRegistryPath, projectName)`

Télécharge, extrait et installe un projet depuis le registre npm.

**Paramètres:**
- `projectsRegistryUrl` (string): URL du registre npm
- `localRegistryPath` (string): Chemin local de destination
- `projectName` (string): Nom du projet à installer

**Retour:** `Promise<Nullable<T>>` - Instance du projet installé ou `null` en cas d'erreur

### `findProjectIndex(localRegistryPath, projectName)`

Recherche le fichier `index.ts` d'un projet dans le répertoire local.

**Paramètres:**
- `localRegistryPath` (string): Chemin du répertoire de cache
- `projectName` (string): Nom du projet

**Retour:** `Promise<string | null>` - Chemin absolu vers le fichier index.ts ou `null`

## 🔧 Fonctionnalités

### Vérification d'intégrité

Le package vérifie automatiquement l'intégrité des fichiers téléchargés en comparant le checksum SHA avec celui fourni par le registre npm :

```typescript
// La vérification est automatique lors de l'installation
// Une erreur sera levée si l'intégrité ne correspond pas
```

### Gestion du cache

Les packages téléchargés sont conservés dans un répertoire local pour éviter les téléchargements inutiles :

```
localRegistryPath/
  ├── projet-a/
  │   ├── index.ts
  │   ├── package.json
  │   └── node_modules/
  ├── projet-b/
  │   ├── index.ts
  │   ├── package.json
  │   └── node_modules/
  └── projet-c/
      ├── index.ts
      ├── package.json
      └── node_modules/
```

### Installation automatique des dépendances

Lors de l'installation d'un projet, les dépendances sont automatiquement installées via `bun install` si le dossier `node_modules` n'existe pas.

## 🛠️ Développement

### Scripts disponibles

```bash
# Formatage du code
bun run format

# Linting
bun run lint

# Vérification complète (format + lint)
bun run check

# Exécution des tests
bun run test

# Tests en mode watch
bun run test:watch
```

### Structure du projet

```
src/
  ├── install-namespace-project.ts   # Logique d'installation et extraction
  ├── project-lookup.ts               # Recherche et chargement des projets
  └── services/
      └── project.service.ts          # Téléchargement et vérification d'intégrité
```

## 📋 Prérequis

- Node.js >= 18
- Bun (pour l'exécution des tests et l'installation des dépendances)
- TypeScript >= 5.5.0

## 🔗 Dépendances

- [`@fixentropy-io/type`](https://github.com/fixentropy-io/type) - Types et utilitaires pour Result/Maybe
- [`axios`](https://axios-http.com/) - Client HTTP pour le téléchargement des packages
- [`tar`](https://www.npmjs.com/package/tar) - Extraction des archives tar.gz

## 📄 Licence

Consultez le fichier LICENSE pour plus de détails.

## 🤝 Contribution

Les contributions sont les bienvenues ! N'hésitez pas à ouvrir une issue ou une pull request.

## 🔗 Liens utiles

- [Projet parent: dragee-cli](https://github.com/fixentropy-io/dragee-cli)
- [Organisation fixentropy](https://github.com/fixentropy-io)