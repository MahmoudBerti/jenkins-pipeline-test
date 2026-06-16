# Git Ignore

<a name="table-des-matieres"></a>

## Table des matières

1. [Pourquoi .gitignore ?](#pourquoi)
2. [Création Rapide](#creation)
3. [Règles Communes](#regles)
4. [Test et Vérification](#test)

<a name="pourquoi"></a>
## 1. Pourquoi .gitignore ?

**Problème :** Git veut tracker TOUS tes fichiers, même ceux inutiles :
- Fichiers temporaires (`.tmp`)
- Dossiers de build (`node_modules/`)
- Fichiers sensibles (`.env`)
- Cache système (`.DS_Store`)

**Solution : `.gitignore`**
- Liste des fichiers/dossiers à ignorer
- Git fait comme s'ils n'existaient pas

#### [Retour à la table des matières](#table-des-matieres)

<a name="creation"></a>
## 2. Création Rapide

   ```bash
mkdir demo-gitignore && cd demo-gitignore
git init

# Créer des fichiers test
echo "print('app')" > app.py
echo "SECRET_KEY=123456" > .env
mkdir temp && echo "cache" > temp/cache.tmp
mkdir logs && echo "error logs" > logs/app.log

# Sans .gitignore, Git veut tout tracker
git status      # Voir tous les fichiers

# Créer .gitignore
cat > .gitignore << EOF
# Fichiers sensibles
.env
*.secret

# Dossiers temporaires  
temp/
logs/
build/

# Fichiers système
.DS_Store
*.tmp
EOF

# Maintenant Git ignore ces fichiers
git status      # Plus propre !
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="regles"></a>
## 3. Règles Communes

**Patterns utiles :**

| Pattern | Ignore |
|---------|--------|
| `*.log` | Tous les fichiers `.log` |
| `temp/` | Le dossier `temp` entier |
| `.env*` | Tous fichiers commençant par `.env` |
| `!important.log` | Exception : garder ce fichier |
| `build/**` | Tout dans `build` récursivement |

**Templates prêts à l'emploi :**
- **Python :** `__pycache__/`, `*.pyc`, `.env`
- **Node.js :** `node_modules/`, `npm-debug.log`
- **Java :** `*.class`, `target/`

#### [Retour à la table des matières](#table-des-matieres)

<a name="test"></a>
## 4. Test et Vérification

   ```bash
# Vérifier si un fichier est ignoré
git check-ignore -v .env            # Si ignoré : affiche la règle
git check-ignore logs/app.log       # Si ignoré : rien affiché

# Forcer l'ajout d'un fichier ignoré (si vraiment nécessaire)
git add -f fichier-ignore.tmp

# Voir tous les fichiers non ignorés
git ls-files
```

**Règle d'or : Crée ton `.gitignore` au DÉBUT du projet !**

#### [Retour à la table des matières](#table-des-matieres)