# Git Cherry-Pick

<a name="table-des-matieres"></a>

## Table des matières

1. [Qu'est-ce que Cherry-Pick ?](#definition)
2. [Pratique Rapide](#pratique)
3. [Scénario d'Utilisation](#scenario)
4. [Commandes Essentielles](#commandes)

<a name="definition"></a>
## 1. Qu'est-ce que Cherry-Pick ?

**Git cherry-pick** = Copier UN commit spécifique d'une branche vers une autre.

**Exemple concret :**
- Sur `feature-login` : commit de correction bug critique
- Sur `main` : tu veux JUSTE cette correction, pas toute la branche
- Solution : `git cherry-pick <commit-hash>`

#### [Retour à la table des matières](#table-des-matieres)

<a name="pratique"></a>
## 2. Pratique Rapide

**Création du projet :**

```bash
mkdir demo-cherry-pick && cd demo-cherry-pick
git init

echo "print('v1')" > app.py
git add . && git commit -m "Version initiale"

# Créer branche feature
git checkout -b feature-new
echo "print('v2-feature')" > app.py  
git add . && git commit -m "Nouvelle fonctionnalité"

echo "print('v2-bugfix')" > app.py
git add . && git commit -m "Fix critique"

# Revenir à main et cherry-pick SEULEMENT le bugfix
git checkout main
git cherry-pick <hash-du-commit-bugfix>
```

**Voilà ! Le fix est sur main, sans la fonctionnalité.**

#### [Retour à la table des matières](#table-des-matieres)

<a name="scenario"></a>
## 3. Scénario d'Utilisation

**Situation typique :**
1. **Développement** sur `feature-branch`
2. **Bug critique** découvert et fixé
3. **Production** (`main`) a besoin du fix MAINTENANT
4. **Mais pas** de toute la feature en développement

**Solution :**
```bash
git log --oneline feature-branch    # Trouver le hash du fix
git checkout main
git cherry-pick abc123f             # Appliquer JUSTE ce commit
```

#### [Retour à la table des matières](#table-des-matieres)

<a name="commandes"></a>
## 4. Commandes Essentielles

| Commande | Action |
|----------|--------|
| `git cherry-pick <hash>` | Copier un commit |
| `git cherry-pick <hash1> <hash2>` | Copier plusieurs commits |
| `git cherry-pick --continue` | Continuer après résolution conflit |
| `git cherry-pick --abort` | Annuler l'opération |

**C'est tout ! Cherry-pick est simple : 1 commit → 1 copie.**

#### [Retour à la table des matières](#table-des-matieres)