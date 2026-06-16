# Git Worktree

<a name="table-des-matieres"></a>

## Table des matières

1. [Qu'est-ce que Worktree ?](#definition)
2. [Pourquoi Worktree ?](#pourquoi)
3. [Utilisation Pratique](#pratique)
4. [Commandes Essentielles](#commandes)

<a name="definition"></a>
## 1. Qu'est-ce que Worktree ?

**Git worktree** = Avoir plusieurs dossiers de travail pour le MÊME repo.

**Concept :**
- 1 repo Git = normalement 1 dossier  
- Avec worktree = 1 repo, plusieurs dossiers
- Chaque dossier peut être sur une branche différente

**Analogie :** C'est comme avoir plusieurs bureaux pour le même projet.

#### [Retour à la table des matières](#table-des-matieres)

<a name="pourquoi"></a>
## 2. Pourquoi Worktree ?

**Problème classique :**
```bash
# Tu travailles sur feature-A
git checkout feature-A
# Code, code, code... (travail non-fini)

# URGENCE : bug sur main !
git stash                # Sauvegarder work in progress
git checkout main        # Switch vers main  
# Fix le bug...

git checkout feature-A   # Revenir à ton travail
git stash pop           # Récupérer ton work in progress
```

**Solution worktree :**
```bash
# Tu travailles sur feature-A dans dossier principal
# Urgence ? Créer un nouveau dossier pour main !
git worktree add ../hotfix main

# Maintenant :
# - ./           → feature-A (ton travail continue)
# - ../hotfix    → main (pour le fix urgent)
```

**Avantage :** Pas de stash/switch. Juste 2 dossiers, 2 tâches parallèles !

#### [Retour à la table des matières](#table-des-matieres)

<a name="pratique"></a>
## 3. Utilisation Pratique

### **Setup de base :**
```bash
mkdir demo-worktree && cd demo-worktree
git init

echo "print('main v1')" > app.py
git add . && git commit -m "Main version"

# Créer branche feature
git checkout -b feature-new
echo "print('feature work')" > feature.py
git add . && git commit -m "Feature work"

git checkout main
```

### **Créer worktrees :**
```bash
# Créer worktree pour feature-new
git worktree add ../feature-work feature-new

# Créer worktree pour nouvelle branche
git worktree add ../hotfix-branch -b hotfix

# Voir tous les worktrees
git worktree list
```

### **Résultat :**
```
demo-worktree/       → main branch
../feature-work/     → feature-new branch  
../hotfix-branch/    → hotfix branch (nouvelle)
```

### **Travailler en parallèle :**
```bash
# Terminal 1 : Dossier principal (main)
cd demo-worktree
echo "print('main updated')" >> app.py
git add . && git commit -m "Update main"

# Terminal 2 : Feature work
cd ../feature-work  
echo "print('feature complete')" >> feature.py
git add . && git commit -m "Complete feature"

# Terminal 3 : Hotfix
cd ../hotfix-branch
echo "print('urgent fix')" > fix.py
git add . && git commit -m "Critical fix"
```

**Magie :** 3 branches, 3 dossiers, 0 context switching !

#### [Retour à la table des matières](#table-des-matieres)

<a name="commandes"></a>
## 4. Commandes Essentielles

| Commande | Action |
|----------|--------|
| `git worktree add <path> <branch>` | Créer worktree sur branche existante |
| `git worktree add <path> -b <new-branch>` | Créer worktree + nouvelle branche |
| `git worktree list` | Voir tous les worktrees |
| `git worktree remove <path>` | Supprimer worktree |
| `git worktree prune` | Nettoyer worktrees supprimés |

### **Cas d'usage typiques :**

**1. Hotfix urgent :**
```bash
git worktree add ../hotfix main
cd ../hotfix && echo "fix" > fix.py
git add . && git commit -m "Hotfix"
git push origin main
```

**2. Review de PR :**
```bash
git worktree add ../pr-review pr-branch
cd ../pr-review  # Tester la PR sans perturber ton travail
```

**3. Comparaison de branches :**
```bash
git worktree add ../version-a branch-a
git worktree add ../version-b branch-b
# Compare fichiers entre ../version-a et ../version-b
```

### **Nettoyage :**
```bash
git worktree remove ../feature-work
git worktree remove ../hotfix-branch  
git worktree prune                    # Nettoyer les références
```

**Worktree = Multitâche Git sans compromis !**

#### [Retour à la table des matières](#table-des-matieres)