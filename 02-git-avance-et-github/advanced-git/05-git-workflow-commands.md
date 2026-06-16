# Git Workflow & Commandes

<a name="table-des-matieres"></a>

## Table des matières

1. [Local vs Remote](#local-remote)
2. [Workflow de Base](#workflow)
3. [Commandes Essentielles](#commandes)
4. [Cycle Quotidien](#quotidien)

<a name="local-remote"></a>
## 1. Local vs Remote

**Local** = Sur ton PC
- Tu codes, modifies, commites
- Privé, personne d'autre ne voit
- Fonctionne hors-ligne

**Remote** = Sur serveur (GitHub)
- Code partagé avec l'équipe
- Sauvegarde online
- Nécessite Internet

**Analogie simple :** Local = brouillon, Remote = version finale partagée.

#### [Retour à la table des matières](#table-des-matieres)

<a name="workflow"></a>
## 2. Workflow de Base

```
1. MODIFIER tes fichiers
     ↓
2. git ADD (stager)
     ↓  
3. git COMMIT (sauvegarder localement)
     ↓
4. git PUSH (envoyer vers remote)
```

**Visual :**
```
Working Directory → Staging Area → Local Repo → Remote Repo
     edit             add           commit        push
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="commandes"></a>
## 3. Commandes Essentielles

### **Quotidiennes :**
| Commande | Action |
|----------|--------|
| `git status` | Voir l'état des fichiers |
| `git add .` | Stager tous les changements |
| `git commit -m "message"` | Committer avec message |
| `git push` | Envoyer vers remote |
| `git pull` | Récupérer du remote |

### **Setup initial :**
| Commande | Action |
|----------|--------|
| `git init` | Initialiser repo local |
| `git clone <url>` | Copier repo distant |
| `git remote add origin <url>` | Connecter au remote |

### **Informations :**
| Commande | Action |
|----------|--------|
| `git log --oneline` | Historique concis |
| `git diff` | Voir les changements |
| `git branch` | Lister les branches |

#### [Retour à la table des matières](#table-des-matieres)

<a name="quotidien"></a>
## 4. Cycle Quotidien

### **Matin : Récupérer le travail de l'équipe**
```bash
git pull origin main    # Synchroniser avec l'équipe
```

### **Journée : Développer**
```bash
# Modifier tes fichiers...
git status              # Voir ce qui a changé
git add .               # Stager tous les changements
git commit -m "Add new feature"    # Committer
```

### **Soir : Partager ton travail**
```bash
git push origin main    # Envoyer tes commits
```

### **Workflow ultra-rapide :**
```bash
git add . && git commit -m "Quick update" && git push
```

### **En cas d'urgence :**
```bash
git stash              # Sauvegarder work in progress
# Fix urgent...
git stash pop          # Récupérer ton travail
```

### **Règles d'or :**
1. **Toujours pull avant de pusher**
2. **Commit souvent, push quotidiennement**  
3. **Messages de commit descriptifs**
4. **Never push broken code**

**C'est tout ! Maîtrise ces commandes = 90% de ton usage Git quotidien.**

#### [Retour à la table des matières](#table-des-matieres)