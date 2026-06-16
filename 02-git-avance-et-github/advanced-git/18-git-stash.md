# Git Stash

<a name="table-des-matieres"></a>

## Table des matières

1. [Qu'est-ce que Stash ?](#definition)
2. [Pratique Immédiate](#pratique)
3. [Commandes Essentielles](#commandes)
4. [Cas d'Usage Typique](#cas-usage)

<a name="definition"></a>
## 1. Qu'est-ce que Stash ?

**Git stash** = Sauvegarder temporairement tes modifications NON commitées.

**Analogie :** C'est comme mettre tes affaires dans un tiroir secret avant de faire autre chose.

**Pourquoi ?**
- Tu codes sur feature-A
- URGENCE : bug critique sur main  
- Tu ne veux pas commit du code à moitié fait
- **Solution :** `git stash` → switch → fix → revenir → `git stash pop`

#### [Retour à la table des matières](#table-des-matieres)

<a name="pratique"></a>
## 2. Pratique Immédiate

   ```bash
mkdir demo-stash && cd demo-stash  
git init

echo "print('v1')" > app.py
git add . && git commit -m "Version initiale"

# Tu commences à coder
echo "print('work in progress')" >> app.py

# URGENCE ! Tu dois switcher de branche
git stash                    # Sauvegarder le WIP
git status                   # Working directory propre !

# Faire autre chose...
git checkout -b hotfix
echo "print('CRITICAL FIX')" > fix.py
git add . && git commit -m "Urgent fix"

# Revenir au travail original  
git checkout main
git stash pop               # Récupérer le work in progress
```

**Magie ! Tes modifications sont revenues.**

#### [Retour à la table des matières](#table-des-matieres)

<a name="commandes"></a>
## 3. Commandes Essentielles

| Commande | Action |
|----------|--------|
| `git stash` | Sauvegarder les modifs |
| `git stash pop` | Récupérer + supprimer du stash |
| `git stash list` | Voir tous les stashs |
| `git stash apply` | Récupérer SANS supprimer |
| `git stash drop` | Supprimer un stash |
| `git stash clear` | Vider tous les stashs |

#### [Retour à la table des matières](#table-des-matieres)

<a name="cas-usage"></a>
## 4. Cas d'Usage Typique

**Situation classique :**
1. Tu codes tranquillement
2. Collègue : "Y'a un bug en prod !!!"  
3. Toi : `git stash`
4. Fix rapide et déploiement
5. Toi : `git stash pop`
6. Tu continues comme si de rien n'était

**Stash = Ton meilleur ami pour les interruptions !**

#### [Retour à la table des matières](#table-des-matieres)