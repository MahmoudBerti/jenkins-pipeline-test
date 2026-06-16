# Git Sync avec GitHub

<a name="table-des-matieres"></a>

## Table des matières

1. [Sync Local ↔ GitHub](#sync)
2. [Setup Rapide](#setup)
3. [Push/Pull Workflow](#workflow)
4. [Gestion des Conflits](#conflits)
5. [Commandes Essentielles](#commandes)

<a name="sync"></a>
## 1. Sync Local ↔ GitHub

**Objectif :** Garder ton code local synchronisé avec GitHub.

**Le problème :**
- Tu codes localement
- Équipe modifie sur GitHub  
- Comment rester à jour ?

**La solution :** **Push** (toi → GitHub) et **Pull** (GitHub → toi)

#### [Retour à la table des matières](#table-des-matieres)

<a name="setup"></a>
## 2. Setup Rapide

```bash
mkdir demo-github-sync && cd demo-github-sync
git init

echo "print('v1')" > app.py
echo "# Demo GitHub Sync" > README.md
git add . && git commit -m "Initial commit"

# Connecter à GitHub (remplace par ton repo)
git remote add origin https://github.com/ton-username/demo-repo.git

# Premier push
git push -u origin main
```

**C'est tout ! Ton code est sur GitHub.**

#### [Retour à la table des matières](#table-des-matieres)

<a name="workflow"></a>
## 3. Push/Pull Workflow

**Scenario typique :**

### **Push : Toi → GitHub**
```bash
# Tu modifies localement
echo "print('v2 updated')" > app.py
git add . && git commit -m "Update to v2"

# Envoyer vers GitHub
git push origin main
```

### **Pull : GitHub → Toi**
```bash
# Récupérer les changes de l'équipe
git pull origin main

# Ou en 2 étapes :
git fetch origin    # Télécharger sans merger
git merge origin/main    # Merger ensuite
```

### **Workflow Quotidien**
```bash
# Matin : récupérer le travail de l'équipe
git pull

# Journée : tu développes
echo "print('new feature')" >> app.py
git add . && git commit -m "Add feature"

# Soir : pousser ton travail
git push
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="conflits"></a>
## 4. Gestion des Conflits

**Conflit = Toi et collègue modifiez la même ligne.**

### **Simulation conflit :**
```bash
# Ton collègue a modifié app.py sur GitHub
# Toi aussi localement
echo "print('my version')" > app.py
git add . && git commit -m "My changes"

git pull    # CONFLIT !
```

### **Résolution :**
```bash
# Git te montre le conflit dans app.py :
# <<<<<<< HEAD
# print('my version')
# =======
# print('colleague version')  
# >>>>>>> abc123

# Édite le fichier pour garder ce que tu veux :
echo "print('merged version')" > app.py

# Finaliser la résolution :
git add app.py
git commit -m "Resolve conflict"
git push
```

**Conflit résolu ! L'équipe récupère ta version fusionnée.**

#### [Retour à la table des matières](#table-des-matieres)

<a name="commandes"></a>
## 5. Commandes Essentielles

| Commande | Action |
|----------|--------|
| `git clone <url>` | Copier un repo GitHub |
| `git remote add origin <url>` | Connecter à GitHub |
| `git push -u origin main` | Premier push (avec tracking) |
| `git push` | Envoyer tes commits |
| `git pull` | Récupérer + merger |
| `git fetch` | Récupérer sans merger |
| `git status` | Voir l'état sync |

### **Workflow de base :**
1. `git pull` (récupérer)
2. Développer + `git add` + `git commit`
3. `git push` (partager)
4. Repeat!

**GitHub sync = Respiration du développeur : pull → code → push !**

#### [Retour à la table des matières](#table-des-matieres)