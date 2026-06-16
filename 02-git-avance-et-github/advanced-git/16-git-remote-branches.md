# Git Remote Branches

<a name="table-des-matieres"></a>

## Table des matières

1. [Qu'est-ce qu'une Remote Branch ?](#definition)
2. [Tracking des Branches](#tracking)
3. [Synchronisation](#sync)
4. [Gestion des Branches Distantes](#gestion)
5. [Commandes Essentielles](#commandes)

<a name="definition"></a>
## 1. Qu'est-ce qu'une Remote Branch ?

**Remote branch** = Une branche qui existe sur le serveur distant (GitHub, GitLab, etc.).

**Concept clé :**
- **Local branch :** sur ton PC uniquement
- **Remote branch :** sur le serveur partagé
- **Remote-tracking branch :** copie locale de la branche distante

**Analogie :** C'est comme avoir une copie locale du livre de la bibliothèque.

#### [Retour à la table des matières](#table-des-matieres)

<a name="tracking"></a>
## 2. Tracking des Branches

```bash
mkdir demo-remote && cd demo-remote
git init && echo "print('main')" > app.py
git add . && git commit -m "Initial commit"

# Ajouter remote (simule GitHub)  
git remote add origin https://github.com/user/repo.git

# Créer branche qui track une remote
git checkout -b feature-login
echo "print('login')" > login.py
git add . && git commit -m "Add login"

# Push ET créer le tracking
git push -u origin feature-login

# Vérifier le tracking
git branch -vv    # Voir quelles branches trackent quoi
```

**Résultat :** Ta branche locale `feature-login` suit `origin/feature-login`.

#### [Retour à la table des matières](#table-des-matieres)

<a name="sync"></a>
## 3. Synchronisation

**Récupérer les updates sans merger :**
```bash
git fetch origin                 # Télécharge les changements
git status                       # Voir si ta branche est en retard
git log --oneline origin/main    # Voir les nouveaux commits distants
```

**Récupérer ET merger :**
```bash
git pull origin main             # = git fetch + git merge
# Ou pour ta branche trackée :
git pull                         # Plus simple
```

**Pousser tes changements :**
```bash
git push                         # Si branche trackée
git push origin feature-login    # Explicite
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="gestion"></a>
## 4. Gestion des Branches Distantes

**Voir toutes les branches (locales + distantes) :**
```bash
git branch -a                    # Toutes les branches
git branch -r                    # Seulement les remotes
git remote show origin           # Détails du remote
```

**Supprimer une branche distante :**
```bash
git push origin --delete feature-old    # Supprimer sur serveur
git branch -d feature-old               # Supprimer localement
git fetch --prune                       # Nettoyer les références
```

**Créer branche locale depuis remote :**
```bash
git checkout -b local-name origin/remote-name
# Ou plus simple :
git checkout remote-name         # Crée automatiquement le tracking
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="commandes"></a>
## 5. Commandes Essentielles

| Commande | Action |
|----------|--------|
| `git fetch` | Récupérer updates sans merger |
| `git pull` | Récupérer + merger |
| `git push -u origin <branch>` | Pousser + créer tracking |
| `git branch -vv` | Voir le tracking status |
| `git remote show origin` | Infos détaillées du remote |
| `git push origin --delete <branch>` | Supprimer branche distante |
| `git fetch --prune` | Nettoyer références mortes |

**Remote branches = Coordination en équipe. Maîtrise ça = travail fluide !**

#### [Retour à la table des matières](#table-des-matieres)