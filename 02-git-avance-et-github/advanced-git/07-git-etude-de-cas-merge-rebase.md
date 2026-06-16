# Merge vs Rebase - Étude de Cas

<a name="table-des-matieres"></a>

## Table des matières

1. [Le Problème](#probleme)
2. [Solution 1: Merge](#merge)
3. [Solution 2: Rebase](#rebase)
4. [Comparaison Pratique](#comparaison)
5. [Quelle Approche Choisir ?](#choix)

<a name="probleme"></a>
## 1. Le Problème

**Situation typique :**
- Tu développes sur `feature-branch`  
- Pendant ce temps, `main` avance (collègues pushent)
- Tu veux intégrer ton travail dans `main`

**2 stratégies possibles :**
1. **MERGE** : Fusionner les branches  
2. **REBASE** : Réécrire l'historique

#### [Retour à la table des matières](#table-des-matieres)

<a name="merge"></a>
## 2. Solution 1: Merge

### **Setup du scénario :**

```bash
mkdir demo-merge && cd demo-merge && git init

# Base commune
echo "print('app v1')" > app.py
git add . && git commit -m "Version initiale"

# Branche feature
git checkout -b feature-login  
echo "print('login system')" > login.py
git add . && git commit -m "Add login"
echo "print('login v2')" > login.py  
git add . && git commit -m "Improve login"

# Main avance pendant ce temps
git checkout main
echo "print('app v1.1')" > app.py
git add . && git commit -m "Update main app"
```

### **Merge :**
```bash
git merge feature-login
```

### **Résultat Merge :**
```
*   Merge branch 'feature-login'
|\  
| * Improve login
| * Add login  
* | Update main app
|/  
* Version initiale
```

**Historique :** Montre clairement qu'il y a eu 2 lignes de développement parallèles.

#### [Retour à la table des matières](#table-des-matieres)

<a name="rebase"></a>
## 3. Solution 2: Rebase

### **Setup identique, stratégie différente :**

```bash
mkdir demo-rebase && cd demo-rebase && git init

# Même setup que avant...
echo "print('app v1')" > app.py
git add . && git commit -m "Version initiale"

git checkout -b feature-login  
echo "print('login system')" > login.py
git add . && git commit -m "Add login"
echo "print('login v2')" > login.py  
git add . && git commit -m "Improve login"

git checkout main
echo "print('app v1.1')" > app.py
git add . && git commit -m "Update main app"
```

### **Rebase :**
```bash
git checkout feature-login
git rebase main              # "Déplace" tes commits après main
git checkout main  
git merge feature-login      # Fast-forward merge
```

### **Résultat Rebase :**
```
* Improve login
* Add login  
* Update main app
* Version initiale
```

**Historique :** Linéaire, comme si tu avais développé après les updates de main.

#### [Retour à la table des matières](#table-des-matieres)

<a name="comparaison"></a>
## 4. Comparaison Pratique

| Aspect | **MERGE** | **REBASE** |
|--------|-----------|------------|
| **Historique** | Montre branches parallèles | Linéaire, propre |
| **Commit de merge** | Oui, créé automatiquement | Non, fast-forward |
| **Complexité** | Simple | Plus complexe |
| **Conflits** | Résoudre 1 fois | Peut-être plusieurs fois |
| **Traçabilité** | Voit quand feature créée/mergée | Comme si développé séquentiellement |

### **Visualisation côte à côte :**

**MERGE:**
```
    A---B---C main
   /         \
  D---E---F---G feature (merge commit G)
```

**REBASE:**
```
A---B---C---D'---E'---F' main (commits D,E,F "déplacés")
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="choix"></a>
## 5. Quelle Approche Choisir ?

### **Utilise MERGE quand :**
- Tu débutes avec Git
- Tu veux garder l'historique "vrai"
- Équipe travaille sur branches longue durée
- Sécurité avant tout

**Commande :**
```bash
git checkout main
git merge feature-branch
```

### **Utilise REBASE quand :**
- Tu veux historique propre et linéaire
- Feature branch courte et personnelle  
- Équipe préfère historique "story-like"
- Tu maîtrises bien Git

**Commande :**
```bash
git checkout feature-branch
git rebase main
git checkout main
git merge feature-branch    # Fast-forward
```

### **RÈGLE D'OR :**
**JAMAIS rebase des commits déjà partagés/pushés !**

### **Recommandation simple :**
- **Débutant** → Toujours MERGE
- **Confirmé** → REBASE sur branches perso, MERGE pour partager

**Les deux fonctionnent. L'important = cohérence dans l'équipe !**

#### [Retour à la table des matières](#table-des-matieres)