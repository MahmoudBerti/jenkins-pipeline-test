# Git Tags

<a name="table-des-matieres"></a>

## Table des matières

1. [Qu'est-ce qu'un Tag ?](#definition)
2. [Types de Tags](#types)
3. [Utilisation Pratique](#pratique)
4. [Gestion des Versions](#versions)

<a name="definition"></a>
## 1. Qu'est-ce qu'un Tag ?

**Git tag** = Étiquette permanente sur un commit spécifique.

**Usage principal :** Marquer les **versions** (v1.0, v2.0, etc.)

**Différence avec branches :**
- **Branche** = Mobile, évolue avec nouveaux commits
- **Tag** = Fixe, pointe toujours vers le même commit

**Analogie :** Tag = autocollant "Version 1.0" sur une page de cahier.

#### [Retour à la table des matières](#table-des-matieres)

<a name="types"></a>
## 2. Types de Tags

### **Lightweight Tag (simple)**
```bash
git tag v1.0                # Tag simple
```
- Juste un nom
- Pointeur vers un commit
- Rapide à créer

### **Annotated Tag (complet)**  
```bash
git tag -a v1.0 -m "Version 1.0 - First stable release"
```
- Contient un message
- Date et auteur
- Peut être signé (GPG)
- Recommandé pour releases officielles

#### [Retour à la table des matières](#table-des-matieres)

<a name="pratique"></a>
## 3. Utilisation Pratique

### **Setup projet :**
```bash
mkdir demo-tags && cd demo-tags
git init

echo "print('app v0.1')" > app.py
git add . && git commit -m "Initial version"

echo "print('app v0.2')" > app.py  
git add . && git commit -m "Bug fixes"

echo "print('app v1.0')" > app.py
git add . && git commit -m "First stable version"
```

### **Créer tags :**
```bash
# Tag simple
git tag v1.0

# Tag avec message  
git tag -a v1.0-stable -m "Version 1.0 - Production ready"

# Tag sur commit précédent
git log --oneline           # Voir les hashs
git tag v0.2-beta <hash>    # Tag sur commit spécifique
```

### **Lister tags :**
```bash
git tag                     # Tous les tags
git tag -l "v1.*"          # Tags matching pattern
git show v1.0              # Détails du tag
```

### **Naviguer avec tags :**
```bash
git checkout v1.0          # Aller au commit taggé
git checkout main          # Revenir à main
```

### **Push tags :**
```bash
git push origin v1.0       # Push un tag spécifique
git push origin --tags     # Push tous les tags
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="versions"></a>
## 4. Gestion des Versions

### **Convention Semantic Versioning :**
```
vMAJOR.MINOR.PATCH
- v1.0.0 → First stable release
- v1.0.1 → Bug fix
- v1.1.0 → New feature (backward compatible)  
- v2.0.0 → Breaking changes
```

### **Workflow release typique :**
```bash
# Développement terminé
git checkout main
git pull origin main

# Tag de release
git tag -a v1.2.0 -m "Release v1.2.0 - Add user authentication"

# Push code + tags
git push origin main
git push origin v1.2.0

# Ou push tout d'un coup :
git push origin main --tags
```

### **Gestion des tags :**
```bash
# Supprimer tag local
git tag -d v1.0

# Supprimer tag distant
git push origin --delete v1.0

# Modifier un tag (recréer)
git tag -d v1.0                           # Supprimer local
git push origin --delete v1.0             # Supprimer distant  
git tag -a v1.0 -m "Corrected message"    # Recréer
git push origin v1.0                      # Re-push
```

### **Cas d'usage :**

**1. Release en production :**
```bash
git tag -a v2.1.0 -m "Production release v2.1.0"
git push origin v2.1.0
# Deploy version v2.1.0 en production
```

**2. Retour à version précédente :**
```bash
git checkout v2.0.0        # Revenir à version stable
git checkout -b hotfix-v2.0.0    # Créer branche hotfix
```

**3. Comparer versions :**
```bash
git diff v1.0.0 v2.0.0     # Voir tous les changements
```

**Tags = Jalons permanents de ton projet. Utilise-les pour toutes tes releases !**

#### [Retour à la table des matières](#table-des-matieres)