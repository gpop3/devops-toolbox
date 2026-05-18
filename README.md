# 🛠️ DevSecOps Toolbox

Ce dépôt centralise mes **workflows GitHub Actions réutilisables (`workflow_call`)** et ses scripts DevOps. 

L'objectif est d'adopter une approche **DRY (Don't Repeat Yourself)** à l'échelle de l'infrastructure, en évitant la duplication de fichiers YAML.

---

## 🔄 1. Git Mirror

Ce workflow permet de synchroniser de manière totalement automatisée et sécurisée un dépôt source (généralement privé) vers un dépôt de destination (généralement public pour un portfolio ou un miroir de sauvegarde).

### ⚙️ Fonctionnement interne
Le workflow s'exécute dans le contexte du dépôt appelant. Il configure une identité Git temporaire sécurisée, ajoute le dépôt distant ciblé via une authentification par Token (PAT), puis force le déploiement (`git push -f`) de la branche principale.

### 🚀 Utilisation / Intégration

Pour utiliser ce mécanisme dans votre pipeline, ajoutez simplement le bloc suivant dans le fichier de workflow de votre dépôt d'origine :

```yaml
name: Git Mirror

on:
  push:
    branches: [main]

jobs:
  call-shared-mirror:
    uses: [TonPseudoGitHub]/devops-toolbox/.github/workflows/mirror.yml@main
    with:
      target_repo: "[PseudoGitHub]/mon-repo-public-cible"
    secrets:
      api_token: ${{ secrets.PUBLIC_REPO_TOKEN }}
```

### 📥 Entrées & Secrets requis

| Paramètre | Type | Statut | Description |
|---|---|---|---|
| `inputs.target_repo` | `string` | Requis | Le chemin du dépôt de destination, par exemple `[PseudoGitHub]/mon-repo-public-cible`. |
| `secrets.api_token` | `secret` | Requis | Un Personal Access Token, PAT GitHub, ayant les droits d’écriture `repo` sur le dépôt cible. |

### 📂 Structure de la Boîte à Outils

```Plaintext
devops-toolbox/
├── .github/
│   └── workflows/
│       └── mirror.yml      # Workflow réutilisable de synchronisation Git
└── README.md               # Présentation et documentation d'utilisation
```