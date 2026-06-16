# Exercices Git - Test Pratique

<a name="table-des-matieres"></a>

## Table des matières

1. [Exercice 1: Basics](#exercice1)
2. [Exercice 2: Branches](#exercice2)
3. [Exercice 3: Merge vs Rebase](#exercice3)
4. [Exercice 4: Stash & Cherry-pick](#exercice4)
5. [Exercice 5: Urgence & Reset](#exercice5)

<a name="exercice1"></a>
## Exercice 1: Basics

**Mission :** Créer un projet, faire 3 commits, corriger le dernier.

```bash
mkdir test-git && cd test-git && git init

# 3 commits
echo "print('v1')" > app.py && git add . && git commit -m "Version 1"
echo "print('v2')" > app.py && git add . && git commit -m "Version 2"  
echo "print('v3 with bug')" > app.py && git add . && git commit -m "Version 3"

# Corriger le dernier commit
echo "print('v3 fixed')" > app.py && git add . && git commit --amend -m "Version 3 - Fixed"
```

**Vérification :** `git log --oneline` → 3 commits, le dernier dit "Fixed"

#### [Retour à la table des matières](#table-des-matieres)

<a name="exercice2"></a>
## Exercice 2: Branches

**Mission :** 2 features en parallèle, merger proprement.

```bash
# Base
echo "print('main')" > main.py && git add . && git commit -m "Main app"

# Feature 1  
git checkout -b feature-login
echo "print('login')" > login.py && git add . && git commit -m "Add login"

# Feature 2
git checkout main && git checkout -b feature-api
echo "print('api')" > api.py && git add . && git commit -m "Add API"

# Merger tout dans main
git checkout main
git merge feature-login
git merge feature-api

# Nettoyer
git branch -d feature-login feature-api
```

**Vérification :** `git log --graph --oneline` → voir les merges

#### [Retour à la table des matières](#table-des-matieres)

<a name="exercice3"></a>
## Exercice 3: Merge vs Rebase

**Mission :** Même scenario, 2 stratégies différentes.

### **Stratégie A : Merge**
```bash
mkdir test-merge && cd test-merge && git init
echo "print('base')" > app.py && git add . && git commit -m "Base"

# Feature branch
git checkout -b feature
echo "print('feature')" > feature.py && git add . && git commit -m "Add feature"

# Main avance
git checkout main
echo "print('base v2')" > app.py && git add . && git commit -m "Update base"

# Merge
git merge feature
git log --graph --oneline    # Voir l'historique avec merge
```

### **Stratégie B : Rebase**  
```bash
mkdir test-rebase && cd test-rebase && git init
echo "print('base')" > app.py && git add . && git commit -m "Base"

git checkout -b feature  
echo "print('feature')" > feature.py && git add . && git commit -m "Add feature"

git checkout main
echo "print('base v2')" > app.py && git add . && git commit -m "Update base"

# Rebase
git checkout feature && git rebase main
git checkout main && git merge feature
git log --oneline           # Historique linéaire
```

**Question :** Quelle différence vois-tu dans `git log` ?

#### [Retour à la table des matières](#table-des-matieres)

<a name="exercice4"></a>
## Exercice 4: Stash & Cherry-pick

**Mission :** Jongler avec plusieurs tâches.

```bash
mkdir test-stash && cd test-stash && git init
echo "print('v1')" > app.py && git add . && git commit -m "Version 1"

# Tu commences à coder
echo "print('work in progress')" >> app.py

# URGENCE ! Tu dois switcher
git stash                    # Sauvegarder
git checkout -b hotfix
echo "print('URGENT FIX')" > fix.py && git add . && git commit -m "Critical fix"

# Cherry-pick le fix vers main
git checkout main
git cherry-pick <hash-du-fix>

# Revenir à ton travail
git stash pop
echo "print('feature complete')" >> app.py && git add . && git commit -m "Feature done"
```

**Défi :** Le fix urgent est-il dans main ET dans hotfix ?

#### [Retour à la table des matières](#table-des-matieres)

<a name="exercice5"></a>
## Exercice 5: Urgence & Reset

**Mission :** Simuler une urgence avec reset.

```bash
mkdir test-urgence && cd test-urgence && git init

# Plusieurs commits
echo "print('v1')" > app.py && git add . && git commit -m "Version 1"
echo "print('v2')" > app.py && git add . && git commit -m "Version 2"  
echo "print('v3')" > app.py && git add . && git commit -m "Version 3"
echo "print('v4 BROKEN')" > app.py && git add . && git commit -m "Version 4 - Bug!"

# Oh non ! v4 casse tout en production
git log --oneline                    # Noter le hash de v3

# Solution 1 : Reset hard (destructif)
git reset --hard <hash-v3>           # Retour à v3, v4 disparaît
git log --oneline                    # Plus que 3 commits

# Solution 2 : Revert (sûr)  
# Remettre v4 d'abord
git reset --hard <hash-v4>           # Revenir à v4
git revert HEAD                      # Créer commit qui annule v4
git log --oneline                    # 5 commits : v1,v2,v3,v4,revert-v4
```

**Leçon :** Revert = sûr pour production, Reset = OK pour local

#### [Retour à la table des matières](#table-des-matieres)

---

## **CHALLENGE FINAL**

**Peux-tu faire tout ça en 15 minutes ?**
1. Projet avec 3 commits
2. 2 branches avec features
3. 1 merge + 1 rebase
4. 1 stash + 1 cherry-pick  
5. 1 reset ou revert d'urgence

**Si oui → tu maîtrises Git !**

#### [Retour à la table des matières](#table-des-matieres)