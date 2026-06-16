# Git Branches

<a name="table-des-matieres"></a>

## Table des matières

1. [Pourquoi les Branches ?](#pourquoi)
2. [Branches en Action](#action)
3. [Merge vs Rebase](#merge-rebase)
4. [Commandes Essentielles](#commandes)
5. [Workflow Recommandé](#workflow)

<a name="pourquoi"></a>
## 1. Pourquoi les Branches ?

**Problème sans branches :**
- Tout le monde code sur `main`
- Chaos total, conflits permanents
- Code cassé en permanence

**Solution : Branches !**
- `main` = toujours stable
- `feature-login` = tu développes la connexion
- `feature-api` = collègue développe l'API
- `hotfix-bug` = correction urgente

**Chacun dans son coin, merge quand c'est prêt !**

#### [Retour à la table des matières](#table-des-matieres)

<a name="action"></a>
## 2. Branches en Action

```bash
mkdir demo-branches && cd demo-branches
git init

# Base de départ
echo "print('App v1.0')" > app.py
git add . && git commit -m "Version initiale"

# Créer et aller sur nouvelle branche
git checkout -b feature-login
echo "print('Login system')" > login.py
git add . && git commit -m "Add login"

# Revenir sur main
git checkout main
echo "print('App v1.1')" > app.py  
git add . && git commit -m "Update main"

# Voir toutes les branches
git branch --all

# Merger la feature
git merge feature-login
```

**Résultat : Les 2 développements sont combinés !**

#### [Retour à la table des matières](#table-des-matieres)

<a name="merge-rebase"></a>
## 3. Merge vs Rebase

**MERGE :**
```bash
git merge feature-branch    # Crée un commit de merge
```
- Historique : montre que 2 branches ont été combinées
- Sûr et simple

**REBASE :**
```bash
git rebase main             # "Déplace" tes commits après main
```
- Historique : linéaire, propre
- Plus complexe

**Recommandation : Commence par MERGE !**

#### [Retour à la table des matières](#table-des-matieres)

<a name="commandes"></a>
## 4. Commandes Essentielles

| Commande | Action |
|----------|--------|
| `git branch` | Lister les branches |
| `git checkout -b <nom>` | Créer + aller sur branche |
| `git checkout <nom>` | Changer de branche |
| `git merge <branche>` | Merger une branche |
| `git branch -d <nom>` | Supprimer branche |
| `git push origin <nom>` | Pousser branche |

#### [Retour à la table des matières](#table-des-matieres)

<a name="workflow"></a>
## 5. Workflow Recommandé

**Workflow simple et efficace :**

1. **Nouvelle feature :**
   ```bash
   git checkout main
   git pull
   git checkout -b feature-nouvelle-fonctionnalite
   ```

2. **Développer :**
   ```bash
   # Code, code, code...
   git add .
   git commit -m "Feature terminée"
   ```

3. **Merger :**
   ```bash
   git checkout main
   git merge feature-nouvelle-fonctionnalite
   git branch -d feature-nouvelle-fonctionnalite  # Nettoyer
   ```

**C'est tout ! Branches = Organisation et Propreté.**

#### [Retour à la table des matières](#table-des-matieres)