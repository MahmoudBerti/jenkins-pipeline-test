# Git Squash

<a name="table-des-matieres"></a>

## Table des matières

1. [Qu'est-ce que Squash ?](#definition)
2. [Pourquoi Squash ?](#pourquoi)
3. [Squash Interactif](#interactif)
4. [Pratique Immédiate](#pratique)
5. [Cas d'Usage](#cas-usage)

<a name="definition"></a>
## 1. Qu'est-ce que Squash ?

**Git squash** = Combiner plusieurs commits en UN SEUL commit.

**Analogie :** Tu as 5 brouillons d'un email → tu les combines en 1 email final propre.

**Exemple :**
```
AVANT squash:
- "fix typo"  
- "add feature"
- "fix bug"
- "another fix"

APRÈS squash:  
- "Add complete login feature"
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="pourquoi"></a>
## 2. Pourquoi Squash ?

**Problème : Historique sale**
```
git log --oneline
abc123 oops fix typo
def456 forgot semicolon  
ghi789 actually implement feature
jkl012 fix indent
mno345 real commit message
```

**Solution : Squash !**
```
git log --oneline  
xyz999 Implement user authentication system
```

**Avantages :**
- Historique propre et lisible
- Commits logiques par fonctionnalité
- Plus facile de revert une feature complète

#### [Retour à la table des matières](#table-des-matieres)

<a name="interactif"></a>
## 3. Squash Interactif

**La commande magique :**
   ```bash
git rebase -i HEAD~4    # Squash les 4 derniers commits
```

**Interface qui s'ouvre :**
```
pick abc123 oops fix typo
pick def456 forgot semicolon
pick ghi789 actually implement feature  
pick jkl012 fix indent
```

**Change en :**
```
pick ghi789 actually implement feature
squash abc123 oops fix typo
squash def456 forgot semicolon  
squash jkl012 fix indent
```

**Résultat :** 4 commits → 1 commit propre !

#### [Retour à la table des matières](#table-des-matieres)

<a name="pratique"></a>
## 4. Pratique Immédiate

   ```bash
mkdir demo-squash && cd demo-squash
git init

# Créer plusieurs commits "sales"
echo "print('v1')" > app.py && git add . && git commit -m "start feature"
echo "print('v1.1')" > app.py && git add . && git commit -m "oops fix"  
echo "print('v1.2')" > app.py && git add . && git commit -m "another fix"
echo "print('v2 done')" > app.py && git add . && git commit -m "feature complete"

# Historique sale
git log --oneline

# Squash interactif
git rebase -i HEAD~4

# Dans l'éditeur : garder le premier "pick", changer les autres en "squash"
# Sauvegarder et fermer

# Écrire le message du commit final
# Résultat : 1 commit propre !
   git log --oneline
   ```

#### [Retour à la table des matières](#table-des-matieres)

<a name="cas-usage"></a>
## 5. Cas d'Usage

**Situations typiques :**

1. **Feature branch avant merge :**
   ```bash
   git checkout feature-login
   # ... 15 commits de dev avec typos/fixes
   git rebase -i HEAD~15    # Squash en 2-3 commits logiques
   git checkout main && git merge feature-login
   ```

2. **Nettoyer avant push :**
   ```bash
   # Développement local brouillon
   git rebase -i HEAD~8     # Nettoyer avant de partager
   git push origin feature-branch
   ```

3. **Corriger l'historique public (attention !) :**
   ```bash
   # Seulement si personne n'a encore récupéré tes commits
   git rebase -i HEAD~5
   git push --force-with-lease    # Plus sûr que --force
   ```

**Squash = Ton aspirateur pour nettoyer l'historique !**

#### [Retour à la table des matières](#table-des-matieres)