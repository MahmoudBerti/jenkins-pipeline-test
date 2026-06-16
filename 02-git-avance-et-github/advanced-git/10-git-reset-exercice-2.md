# Git Reset - Exercice Pratique

<a name="table-des-matieres"></a>

## Table des matières

1. [Setup de l'Exercice](#setup)
2. [Exercice 1: Reset Soft](#exercice1)
3. [Exercice 2: Reset Mixed](#exercice2) 
4. [Exercice 3: Reset Hard](#exercice3)
5. [Vérification](#verification)

<a name="setup"></a>
## 1. Setup de l'Exercice

```bash
mkdir exercice-reset && cd exercice-reset
git init

# Créer plusieurs commits pour tester
echo "print('v1')" > app.py && git add . && git commit -m "Version 1"
echo "print('v2')" > app.py && git add . && git commit -m "Version 2"
echo "print('v3')" > app.py && git add . && git commit -m "Version 3"
echo "print('v4')" > app.py && git add . && git commit -m "Version 4"

git log --oneline    # Noter les hashs
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="exercice1"></a>
## 2. Exercice 1: Reset Soft

**Objectif :** Revenir 2 commits en arrière, garder les changements stagés.

```bash
git reset --soft HEAD~2

# Vérifications
git status           # Changements en staging area ?
cat app.py          # Quel contenu ?
git log --oneline   # Combien de commits ?
```

**Résultat attendu :**
- Fichier : contient `v4`
- Status : modifs stagées pour commit
- Log : 2 commits (v1, v2)

#### [Retour à la table des matières](#table-des-matieres)

<a name="exercice2"></a>
## 3. Exercice 2: Reset Mixed  

**Remise à zéro puis test mixed :**

```bash
git commit -m "Re-commit v4"    # Remettre v4
git reset --mixed HEAD~1        # Revenir 1 commit (mode par défaut)

# Vérifications
git status           # Changements non-stagés ?
cat app.py          # Quel contenu ?
git log --oneline   # Combien de commits ?
```

**Résultat attendu :**
- Fichier : contient `v4`
- Status : modifs non-stagées
- Log : 3 commits (v1, v2, v3)

#### [Retour à la table des matières](#table-des-matieres)

<a name="exercice3"></a>
## 4. Exercice 3: Reset Hard

**ATTENTION : Reset hard = destructif !**

```bash
git add .                    # Stager les changements
git commit -m "Re-commit v4" # Remettre v4
git reset --hard HEAD~2      # TOUT supprimer, revenir 2 commits

# Vérifications
git status           # Working directory propre ?
cat app.py          # Quel contenu ?
git log --oneline   # Combien de commits ?
```

**Résultat attendu :**
- Fichier : contient `v2`
- Status : nothing to commit
- Log : 2 commits (v1, v2)

#### [Retour à la table des matières](#table-des-matieres)

<a name="verification"></a>
## 5. Vérification

**Récap des 3 modes :**

| Mode | Commits | Staging | Working Directory |
|------|---------|---------|------------------|
| `--soft` | Recule | Garde | Garde |
| `--mixed` | Recule | Supprime | Garde |
| `--hard` | Recule | Supprime | Supprime |

**Tu as compris les 3 modes ? Parfait ! Reset n'a plus de secrets.**

#### [Retour à la table des matières](#table-des-matieres)