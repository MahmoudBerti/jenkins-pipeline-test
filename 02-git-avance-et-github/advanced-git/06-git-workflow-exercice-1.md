# Git Workflow - Premier Exercice

<a name="table-des-matieres"></a>

## Table des matières

1. [Setup Initial](#setup)
2. [Premier Commit](#first-commit)
3. [Modifications & Status](#modifications)
4. [Remote & Push](#remote)
5. [Vérification](#verification)

<a name="setup"></a>
## 1. Setup Initial

**Mission :** Créer ton premier projet Git de A à Z.

```bash
# Créer dossier et initialiser Git
mkdir mon-premier-projet && cd mon-premier-projet
git init

# Vérifier le statut
git status    # Should show "On branch main, no commits yet"
```

**Résultat :** Dossier `.git/` créé = ton projet est maintenant sous Git !

#### [Retour à la table des matières](#table-des-matieres)

<a name="first-commit"></a>
## 2. Premier Commit

```bash
# Créer fichiers de base
echo "print('Hello World')" > app.py
echo "# Mon Premier Projet Git" > README.md

# Voir ce que Git voit
git status                    # Fichiers "untracked"

# Ajouter au staging
git add .                     # Tous les fichiers
git status                    # Fichiers "staged for commit"

# Premier commit
git commit -m "Initial commit: Hello World app"

# Vérifier l'historique  
git log --oneline
```

**Résultat :** Ton premier commit est dans l'historique !

#### [Retour à la table des matières](#table-des-matieres)

<a name="modifications"></a>
## 3. Modifications & Status

**Mission :** Apprendre le cycle modify → add → commit.

```bash
# Modifier app.py
echo "print('Hello Git!')" > app.py

git status                    # "modified: app.py"
git diff                      # Voir les changements

# Ajouter nouvelle modification
git add app.py
git status                    # "Changes to be committed"

# Committer
git commit -m "Update: Hello Git message"

# Ajouter nouveau fichier
echo "def helper(): return True" > utils.py
git add utils.py && git commit -m "Add utils module"

# Voir l'évolution
git log --oneline             # 3 commits maintenant
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="remote"></a>
## 4. Remote & Push

**Mission :** Connecter ton projet à GitHub.

### **Étape 1: Créer repo GitHub**
1. Aller sur github.com
2. Créer nouveau repository 
3. Copier l'URL (ex: `https://github.com/ton-username/mon-projet.git`)

### **Étape 2: Connecter local → GitHub**
```bash
# Ajouter remote (remplace par ton URL)
git remote add origin https://github.com/ton-username/mon-projet.git

# Vérifier connexion
git remote -v                 # Voir l'URL configurée

# Premier push
git push -u origin main       # Push + setup tracking

# Futurs pushes plus simples
git push                      # Suffit maintenant
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="verification"></a>
## 5. Vérification

**Checklist finale :**

- [ ] `git status` → "nothing to commit, working tree clean"
- [ ] `git log --oneline` → au moins 3 commits
- [ ] Ton code visible sur GitHub
- [ ] `git remote -v` → URL GitHub configurée

### **Test de compréhension :**

1. **Fais une modification :**
   ```bash
   echo "print('Final version')" >> app.py
   ```

2. **Cycle complet :**
   ```bash
   git status        # Voir le changement
   git add app.py    # Stager  
   git commit -m "Final update"    # Committer
   git push          # Partager sur GitHub
   ```

3. **Vérifier sur GitHub :** Ton changement est-il visible ?

### **Si tout fonctionne → tu maîtrises le workflow Git de base !**

**Prochaine étape :** Apprendre les branches pour collaborer en équipe.

#### [Retour à la table des matières](#table-des-matieres)