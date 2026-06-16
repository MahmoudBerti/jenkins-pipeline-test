# Git Revert

<a name="table-des-matieres"></a>

## Table des matières

1. [Qu'est-ce que Revert ?](#definition)
2. [Revert vs Reset](#vs-reset)
3. [Pratique Immédiate](#pratique)
4. [Cas d'Usage](#cas-usage)

<a name="definition"></a>
## 1. Qu'est-ce que Revert ?

**Git revert** = Annuler UN commit en créant un NOUVEAU commit inverse.

**Concept clé :**
- Ne supprime PAS l'historique
- Crée un commit qui "défait" les changements
- Sûr pour le travail en équipe

**Analogie :** C'est comme dire "Oops, je retire ce que j'ai dit" publiquement.

#### [Retour à la table des matières](#table-des-matieres)

<a name="vs-reset"></a>
## 2. Revert vs Reset

| Aspect | `git revert` | `git reset` |
|--------|--------------|-------------|
| **Historique** | Conservé | Modifié/Supprimé |
| **Sécurité** | Sûr en équipe | Dangereux si partagé |
| **Méthode** | Nouveau commit inverse | Recule dans le temps |
| **Traçabilité** | Visible que c'est annulé | Comme si ça n'avait jamais existé |

**Règle simple :** 
- Commit déjà partagé → `revert`
- Commit encore local → `reset`

#### [Retour à la table des matières](#table-des-matieres)

<a name="pratique"></a>
## 3. Pratique Immédiate

```bash
mkdir demo-revert && cd demo-revert
git init

echo "print('Version 1')" > app.py
git add . && git commit -m "Version 1"

echo "print('Version 2 - Bug!')" > app.py
git add . && git commit -m "Version 2 avec bug"

echo "print('Version 3')" > app.py  
git add . && git commit -m "Version 3"

# Oups ! La version 2 avait un bug
git log --oneline              # Voir l'historique
git revert <hash-version-2>    # Annuler JUSTE la version 2

# Résultat : Version 3 reste, mais le bug de v2 est annulé !
cat app.py                     # Vérifier le contenu
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="cas-usage"></a>
## 4. Cas d'Usage

**Situations typiques :**

1. **Bug en production :**
   ```bash
   git revert <commit-buggy>    # Annulation propre
   git push origin main         # Fix immédiat
   ```

2. **Feature pose problème :**
   ```bash
   git revert <commit-feature>  # Retirer la feature
   # Garder l'historique pour plus tard
   ```

3. **Annuler plusieurs commits :**
   ```bash
   git revert <commit1> <commit2> <commit3>
   # Ou en une fois :
   git revert <oldest-commit>..<newest-commit>
   ```

**Revert = Ton filet de sécurité quand tout va mal !**

#### [Retour à la table des matières](#table-des-matieres)