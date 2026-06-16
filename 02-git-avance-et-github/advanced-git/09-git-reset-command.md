# Git Reset

<a name="table-des-matieres"></a>

## Table des matières

1. [Qu'est-ce que Reset ?](#definition)
2. [Les 3 Modes de Reset](#modes)
3. [Reset Soft](#reset-soft)
4. [Reset Mixed](#reset-mixed) 
5. [Reset Hard](#reset-hard)
6. [Danger et Précautions](#danger)
7. [Commandes Pratiques](#commandes)

<a name="definition"></a>
## 1. Qu'est-ce que Reset ?

**Git reset** = Remonter le temps en annulant des commits.

**Concept :** 
- Déplacer la branche vers un commit antérieur
- 3 modes selon ce qu'on garde ou supprime
- DESTRUCTIF si mal utilisé

**Analogie :** C'est comme effacer des pages d'un cahier.

#### [Retour à la table des matières](#table-des-matieres)

<a name="modes"></a>
## 2. Les 3 Modes de Reset

| Mode | Commits | Index/Staging | Working Directory |
|------|---------|---------------|------------------|
| **--soft** | Annule | Conserve | Conserve |
| **--mixed** | Annule | Efface | Conserve |
| **--hard** | Annule | Efface | Efface |

**Mémo simple :**
- **Soft** = juste les commits
- **Mixed** = commits + staging  
- **Hard** = TOUT (dangereux !)

#### [Retour à la table des matières](#table-des-matieres)

<a name="reset-soft"></a>
## 3. Reset Soft

**Usage :** Refaire le dernier commit avec plus de changements.

```bash
mkdir demo-reset && cd demo-reset
git init

echo "print('v1')" > app.py && git add . && git commit -m "Version 1"
echo "print('v2')" > app.py && git add . && git commit -m "Version 2"

# Reset soft : annule commit, garde tout en staging
git reset --soft HEAD~1

git status                    # Changements stagés prêts
git log --oneline            # Un commit en moins
cat app.py                   # Contenu v2 toujours là

# Recommit avec message amélioré
git commit -m "Version 2 - Avec nouvelles features"
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="reset-mixed"></a>
## 4. Reset Mixed

**Usage :** Annuler commits ET déstagifier (mode par défaut).

```bash
echo "print('v3')" > app.py && git add . && git commit -m "Version 3"

# Reset mixed (ou juste git reset)
git reset HEAD~1

git status                   # Changements NON-stagés
git log --oneline           # Commits annulés
cat app.py                  # Contenu v3 toujours là, mais pas stagé

# Faut re-add avant commit
git add . && git commit -m "Version 3 - Refait proprement"
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="reset-hard"></a>
## 5. Reset Hard

**DANGER** : Supprime TOUT, même tes modifications non-commitées !

```bash
echo "print('v4')" > app.py && git add . && git commit -m "Version 4"

# Modifications en cours
echo "print('work in progress')" >> app.py

# Reset hard : TOUT DISPARAÎT
git reset --hard HEAD~1

git status                  # Working directory clean
cat app.py                 # Revenu à v2, work in progress PERDU !
```

**Usage :** Quand tu veux VRAIMENT tout jeter et revenir en arrière.

#### [Retour à la table des matières](#table-des-matieres)

<a name="danger"></a>
## 6. Danger et Précautions

**Reset est DESTRUCTEUR avec commits partagés !**

**JAMAIS faire ça :**
```bash
git push origin main           # Commits partagés avec l'équipe  
git reset --hard HEAD~3        # Supprime 3 commits partagés
git push --force              # Force la suppression chez tout le monde
# = CHAOS dans l'équipe !
```

**Règle de sécurité :**
- Reset OK sur commits locaux (non-pushés)
- Reset INTERDIT sur commits déjà partagés
- Alternative : `git revert` (plus sûr)

#### [Retour à la table des matières](#table-des-matieres)

<a name="commandes"></a>
## 7. Commandes Pratiques

| Commande | Action |
|----------|--------|
| `git reset HEAD~1` | Annule 1 commit (mixed) |
| `git reset --soft HEAD~2` | Annule 2 commits, garde staging |
| `git reset --hard HEAD~1` | Annule 1 commit, supprime TOUT |
| `git reset <hash>` | Revenir à un commit spécifique |
| `git reset --hard origin/main` | Reset sur version distante |

**Reset = Puissant mais dangereux. Utilise avec précaution !**

#### [Retour à la table des matières](#table-des-matieres)