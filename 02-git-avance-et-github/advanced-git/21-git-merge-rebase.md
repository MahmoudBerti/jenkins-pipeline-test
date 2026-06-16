# Merge vs Rebase - Guide Décisionnel

<a name="table-des-matieres"></a>

## Table des matières

1. [Le Dilemme](#dilemme)
2. [Merge Expliqué](#merge)
3. [Rebase Expliqué](#rebase)
4. [Tableau Comparatif](#comparatif)
5. [Guide de Décision](#decision)

<a name="dilemme"></a>
## 1. Le Dilemme

**Tu développes sur une branche. Main a avancé. Comment intégrer ?**

```
    A---B---C main
   /
  D---E feature-branch
```

**2 options :**
- **MERGE** : Fusionner avec commit de merge
- **REBASE** : "Déplacer" tes commits après main

**Lequel choisir ? Ça dépend de ton objectif !**

#### [Retour à la table des matières](#table-des-matieres)

<a name="merge"></a>
## 2. Merge Expliqué

### **Commande :**
   ```bash
   git checkout main
git merge feature-branch
```

### **Résultat visuel :**
```
    A---B---C-------M main
   /               /
  D---E-----------/  (M = merge commit)
```

### **Ce qui se passe :**
1. Git crée un nouveau commit (M)
2. Ce commit combine les changements des 2 branches
3. L'historique montre qu'il y a eu 2 lignes de développement

### **Avantages :**
- ✅ **Historique véridique** : On voit qu'il y a eu 2 développements parallèles
- ✅ **Simple** : Une seule commande, une seule résolution de conflit
- ✅ **Sûr** : N'altère jamais les commits existants
- ✅ **Standard** : Approche par défaut dans la plupart des équipes

### **Inconvénients :**
- ❌ **Historique complexe** : Beaucoup de merge commits
- ❌ **Moins lisible** : Difficile de suivre l'évolution linéaire

#### [Retour à la table des matières](#table-des-matieres)

<a name="rebase"></a>
## 3. Rebase Expliqué

### **Commande :**
   ```bash
git checkout feature-branch
   git rebase main
   ```

### **Résultat visuel :**
```
    A---B---C---D'---E' main
```

### **Ce qui se passe :**
1. Git "détache" tes commits D et E
2. Les "rejoue" après le commit C
3. Créé de nouveaux commits D' et E' (mêmes changements, nouveaux hashs)
4. L'historique devient linéaire

### **Avantages :**
- ✅ **Historique propre** : Ligne droite, facile à lire
- ✅ **Fast-forward merge** : Pas de commit de merge
- ✅ **Story-like** : Comme si tu avais développé après main

### **Inconvénients :**
- ❌ **Réécrit l'historique** : D et E deviennent D' et E'
- ❌ **Plus complexe** : Plusieurs résolutions de conflit possibles
- ❌ **Dangereux** : Ne jamais rebase des commits partagés !

#### [Retour à la table des matières](#table-des-matieres)

<a name="comparatif"></a>
## 4. Tableau Comparatif

| Critère | **MERGE** | **REBASE** |
|---------|-----------|------------|
| **Historique** | Conserve branches parallèles | Linéarise tout |
| **Commits** | Ajoute commit de merge | Réécrit les commits |
| **Complexité** | Simple (1 commande) | Plus complexe |
| **Conflits** | 1 résolution maximum | Plusieurs résolutions possibles |
| **Sécurité** | Très sûr | Dangereux si mal utilisé |
| **Traçabilité** | Voit quand branches créées/mergées | Perd info temporelle |
| **Collaboration** | Parfait équipe | Attention commits partagés |
| **Lisibilité** | Complexe avec beaucoup branches | Très claire, linéaire |

#### [Retour à la table des matières](#table-des-matieres)

<a name="decision"></a>
## 5. Guide de Décision

### **Utilise MERGE quand :**

**Situations :**
- Tu débutes avec Git
- Tu travailles en équipe
- Tu veux garder l'historique "vrai"
- Tu veux la solution la plus sûre
- Tu merges des branches longue durée

**Commandes :**
   ```bash
git checkout main
git pull origin main
git merge feature-branch
   git push origin main
   ```

### **Utilise REBASE quand :**

**Situations :**
- Tu veux un historique propre et linéaire
- Ta branche est courte et personnelle
- Tu n'as pas encore partagé tes commits
- Ton équipe préfère l'historique linéaire
- Tu maîtrises bien Git

**Commandes :**
   ```bash
git checkout feature-branch
git rebase main               # Résoudre conflits si nécessaire
     git checkout main
git merge feature-branch      # Fast-forward merge
     git push origin main
     ```

### **RÈGLE D'OR :**

**NEVER REBASE SHARED COMMITS !**

   ```bash
# DANGER - Ne JAMAIS faire ça :
git push origin feature-branch    # Commits partagés
git rebase main                   # Réécrit commits partagés = CHAOS équipe

# OK - Rebase uniquement local :
git rebase main                   # Commits encore locaux = OK
git push origin feature-branch    # Push après rebase = OK
```

### **Recommandation par niveau :**

| **Niveau** | **Stratégie recommandée** |
|------------|---------------------------|
| **Débutant** | Toujours MERGE |
| **Intermédiaire** | MERGE pour collaboration, REBASE pour cleanup local |
| **Expert** | Équipe décide une stratégie cohérente |

### **Stratégies d'équipe populaires :**

1. **"Merge only"** : Simple, sûr, historique complet
2. **"Rebase then merge"** : Rebase local + merge sur main
3. **"Squash merge"** : 1 commit par feature (GitHub style)

**L'important = cohérence dans l'équipe, pas la perfection technique !**

#### [Retour à la table des matières](#table-des-matieres)