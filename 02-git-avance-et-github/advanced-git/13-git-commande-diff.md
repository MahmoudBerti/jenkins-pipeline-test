# Git Diff

<a name="table-des-matieres"></a>

## Table des matières

1. [Qu'est-ce que Diff ?](#definition)
2. [Voir les Différences](#voir)
3. [Types de Diff](#types)
4. [Lecture Rapide](#lecture)
5. [Commandes Pratiques](#commandes)

<a name="definition"></a>
## 1. Qu'est-ce que Diff ?

**Git diff** = Voir EXACTEMENT ce qui a changé dans tes fichiers.

**Pourquoi c'est crucial ?**
- Avant de committer : vérifier tes modifs
- Debugging : comprendre ce qui a cassé
- Review : voir le travail des collègues

**En gros : diff = tes lunettes pour voir les changements !**

#### [Retour à la table des matières](#table-des-matieres)

<a name="voir"></a>
## 2. Voir les Différences

```bash
mkdir demo-diff && cd demo-diff
git init

echo "print('Version 1')" > app.py
git add . && git commit -m "Version initiale"

# Modifier le fichier
echo "print('Version 2 - Updated!')" > app.py

# Voir les changements
git diff                    # Comparaison working directory vs derniers commits

# Ajouter au staging
git add app.py

# Voir différence staged vs derniers commits  
git diff --staged

# Voir différence entre 2 commits
git log --oneline          # Récupérer les hashs
git diff <hash1> <hash2>
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="types"></a>
## 3. Types de Diff

| Commande | Compare |
|----------|---------|
| `git diff` | Working directory vs dernier commit |
| `git diff --staged` | Staging area vs dernier commit |
| `git diff HEAD` | Working directory + staging vs dernier commit |
| `git diff <commit1> <commit2>` | Entre 2 commits spécifiques |
| `git diff <branche1> <branche2>` | Entre 2 branches |

#### [Retour à la table des matières](#table-des-matieres)

<a name="lecture"></a>
## 4. Lecture Rapide

**Format typique :**
```
diff --git a/app.py b/app.py
index abc123..def456 100644
--- a/app.py          # Ancien fichier
+++ b/app.py          # Nouveau fichier
@@ -1 +1 @@           # Position des changements
-print('Version 1')   # Ligne supprimée (rouge)
+print('Version 2')   # Ligne ajoutée (vert)
```

**Lecture express :**
- `---` et `+++` = fichiers comparés
- `-` = supprimé (rouge dans terminal)
- `+` = ajouté (vert dans terminal)

#### [Retour à la table des matières](#table-des-matieres)

<a name="commandes"></a>
## 5. Commandes Pratiques

| Commande | Utilité |
|----------|---------|
| `git diff --name-only` | Juste les noms de fichiers modifiés |
| `git diff --stat` | Statistiques des changements |
| `git diff --word-diff` | Différences au niveau des mots |
| `git diff HEAD~1 HEAD` | Dernier commit vs avant-dernier |

**Diff = ton détecteur de changements. Utilise-le TOUT LE TEMPS !**

#### [Retour à la table des matières](#table-des-matieres)