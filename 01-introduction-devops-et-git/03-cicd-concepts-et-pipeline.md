<a id="top"></a>

# 03 — CI/CD : concepts et pipeline

## Table des matières

| # | Section |
|---|---|
| 1 | [Qu'est-ce que le CI/CD ?](#section-1) |
| 2 | [CI — Intégration continue](#section-2) |
| 3 | [CD — Livraison vs Déploiement continu](#section-3) |
| 4 | [Le pipeline CI/CD étape par étape](#section-4) |
| 5 | [Un exemple concret de bout en bout](#section-5) |
| 6 | [Les avantages de l'automatisation](#section-6) |
| 7 | [Quiz — Concepts CI/CD](#section-7) |
| 8 | [Pratique — Concevoir son premier pipeline](#section-8) |
| 9 | [Synthèse](#section-9) |

---

<a id="section-1"></a>

<details>
<summary>1 — Qu'est-ce que le CI/CD ?</summary>

<br/>

Le **CI/CD** est l'**épine dorsale technique du DevOps**. C'est l'ensemble des pratiques qui automatisent le chemin entre « le code écrit par un développeur » et « l'application qui tourne en production ».

```mermaid
flowchart LR
    A["👩‍💻 Code<br/>écrit"] --> B["CI<br/>build + tests<br/>automatisés"]
    B --> C["CD<br/>livraison /<br/>déploiement"]
    C --> D["🚀 Application<br/>en production"]
```

| Sigle | Nom | En une phrase |
|---|---|---|
| **CI** | Intégration continue (*Continuous Integration*) | On fusionne et on teste le code **souvent et automatiquement**. |
| **CD** | Livraison continue (*Continuous Delivery*) | Le code testé est **toujours prêt** à être déployé (mise en prod manuelle). |
| **CD** | Déploiement continu (*Continuous Deployment*) | Le code testé part **automatiquement** en production. |

> _Sans CI/CD, livrer un logiciel ressemble à un déménagement à la main : lent, fatigant et risqué. Avec CI/CD, c'est un tapis roulant automatisé : on pose le code à un bout, l'application arrive prête à l'autre._

**🔧 Mini-exercice —** Reliez chaque sigle à sa définition : (1) CI, (2) Continuous Delivery, (3) Continuous Deployment.

<details>
<summary>✅ Voir une solution</summary>

(1) CI = fusion + build + tests automatiques fréquents. (2) Continuous **Delivery** = toujours prêt à déployer, **bouton manuel** pour la prod. (3) Continuous **Deployment** = mise en prod **automatique** après tests réussis.

</details>

</details>

<p align="right"><a href="#top">↑ Retour en haut</a></p>

---

<a id="section-2"></a>

<details>
<summary>2 — CI — Intégration continue</summary>

<br/>

L'**intégration continue** consiste à **fusionner fréquemment** le code de tous les développeurs dans une branche commune, et à **valider automatiquement** chaque fusion par un build et des tests.

```mermaid
flowchart TD
    A["Dev A<br/>push"] --> M["Branche commune<br/>(main)"]
    B["Dev B<br/>push"] --> M
    M --> CI{"Build + Tests<br/>automatiques"}
    CI -->|"✅ vert"| OK["Code intégré<br/>sans risque"]
    CI -->|"❌ rouge"| KO["Alerte immédiate<br/>au développeur"]
```

### Pourquoi « continue » ?

| Sans CI | Avec CI |
|---|---|
| On fusionne tout en fin de mois → gros conflits douloureux | On fusionne plusieurs fois par jour → petits conflits faciles |
| Les bugs sont découverts tard | Les bugs sont découverts en minutes |
| « Ça marchait sur ma machine » | Build reproductible sur le serveur |

> _Analogie : ranger sa cuisine au fur et à mesure (CI) plutôt que d'attendre que tout soit sale (intégration « big bang »). Le petit effort fréquent évite la catastrophe rare._

**🔧 Mini-exercice —** Un développeur garde son code 3 semaines sans le fusionner, puis tente un gros merge. Quel principe d'intégration continue n'a-t-il pas respecté, et quelle conséquence est probable ?

<details>
<summary>✅ Voir une solution</summary>

Il n'a pas fusionné **fréquemment**. Conséquence probable : un **conflit de fusion massif** et difficile à résoudre, et des bugs détectés très tard. La CI recommande de petites fusions fréquentes.

</details>

</details>

<p align="right"><a href="#top">↑ Retour en haut</a></p>

---

<a id="section-3"></a>

<details>
<summary>3 — CD — Livraison vs Déploiement continu</summary>

<br/>

Les deux « CD » se ressemblent mais diffèrent sur **un seul point** : qui appuie sur le bouton de mise en production.

```mermaid
flowchart LR
    subgraph DELIVERY["Continuous DELIVERY (livraison)"]
        A1["Build + Tests"] --> A2["Prêt à déployer"] --> A3["🧑 Validation<br/>HUMAINE"] --> A4["Production"]
    end
    subgraph DEPLOY["Continuous DEPLOYMENT (déploiement)"]
        B1["Build + Tests"] --> B2["Prêt à déployer"] --> B3["🤖 Mise en prod<br/>AUTOMATIQUE"] --> B4["Production"]
    end
```

| | Continuous Delivery | Continuous Deployment |
|---|---|---|
| Mise en production | **Manuelle** (un humain clique) | **Automatique** |
| Contrôle | Décision finale humaine | Aucune intervention |
| Idéal pour | Secteurs réglementés, releases planifiées | Équipes matures, fort taux de tests |

> _Continuous Delivery = la voiture est garée devant la porte, prête, clés sur le contact ; vous décidez quand démarrer. Continuous Deployment = la voiture autonome démarre toute seule dès qu'elle est prête._

**🔧 Mini-exercice —** Une banque veut que chaque mise en production soit approuvée par un responsable. Quel « CD » choisir ?

<details>
<summary>✅ Voir une solution</summary>

**Continuous Delivery** : le pipeline rend la version prête automatiquement, mais la mise en production reste une **décision humaine** (validation du responsable).

</details>

</details>

<p align="right"><a href="#top">↑ Retour en haut</a></p>

---

<a id="section-4"></a>

<details>
<summary>4 — Le pipeline CI/CD étape par étape</summary>

<br/>

Un **pipeline** est une suite d'étapes automatisées (appelées **stages**) qui transforment le code source en application déployée. **Si une étape échoue, le pipeline s'arrête** et l'équipe est notifiée.

```mermaid
flowchart LR
    A["📥 Checkout<br/>(récupérer le code)"] --> B["🔨 Build<br/>(compiler/assembler)"]
    B --> C["🧪 Test<br/>(tests automatisés)"]
    C --> D["📦 Package<br/>(créer l'artefact)"]
    D --> E["🧫 Staging<br/>(préproduction)"]
    E --> F["🚀 Deploy<br/>(production)"]

    C -.->|"❌ échec"| X["Stop + alerte"]
```

| Stage | Rôle | Exemple d'outil |
|---|---|---|
| **Checkout** | Récupérer le code depuis Git | Git |
| **Build** | Compiler / assembler | Maven, npm, javac |
| **Test** | Vérifier automatiquement | JUnit, pytest |
| **Package** | Produire un **artefact** livrable | `.jar`, image Docker |
| **Staging** | Déployer en préproduction | serveur de test |
| **Deploy** | Mettre en production | serveur / cloud |

> _Le principe du « **fail fast** » : on place les étapes rapides et pas chères (compilation, tests unitaires) **en premier**. Inutile de déployer si le code ne compile même pas._

**🔧 Mini-exercice —** Dans quel ordre placer ces stages : `Deploy`, `Build`, `Test`, `Checkout` ?

<details>
<summary>✅ Voir une solution</summary>

`Checkout` → `Build` → `Test` → `Deploy`. On récupère le code, on le compile, on le teste, et on ne déploie **que** si tout est vert.

</details>

</details>

<p align="right"><a href="#top">↑ Retour en haut</a></p>

---

<a id="section-5"></a>

<details>
<summary>5 — Un exemple concret de bout en bout</summary>

<br/>

Suivons le parcours d'**une seule ligne de code** corrigée par une développeuse, Léa, dans une application web.

```mermaid
sequenceDiagram
    participant L as Léa (dev)
    participant G as Git / GitHub
    participant CI as Serveur CI/CD
    participant P as Production

    L->>G: git push (correction du bug #42)
    G->>CI: webhook : « nouveau code ! »
    CI->>CI: Build (compilation)
    CI->>CI: Test (124 tests passent ✅)
    CI->>CI: Package (artefact v1.4.1)
    CI->>P: Déploiement automatique
    P-->>L: ✅ En ligne en 6 minutes
```

**Déroulé pas à pas :**

1. Léa corrige un bug et fait `git push`.
2. Un **webhook** prévient le serveur CI/CD qu'il y a du nouveau code.
3. Le pipeline **compile**, lance les **124 tests** (tous verts), puis **empaquette** la version `v1.4.1`.
4. La version est **déployée automatiquement**.
5. Six minutes après le push, la correction est en ligne — **sans intervention manuelle**.

> _Comparez : avant le CI/CD, cette même correction aurait demandé une mise en production planifiée, un soir, à la main, avec le stress du « pourvu que ça marche ». Ici, c'est une routine de 6 minutes._

**🔧 Mini-exercice —** À l'étape 3, 2 tests sur 124 échouent. Que fait le pipeline, et la version part-elle en production ?

<details>
<summary>✅ Voir une solution</summary>

Le pipeline **s'arrête** au stage Test, notifie Léa, et **ne déploie pas**. La production reste sur la version stable précédente. C'est le principe « fail fast » qui protège la prod.

</details>

</details>

<p align="right"><a href="#top">↑ Retour en haut</a></p>

---

<a id="section-6"></a>

<details>
<summary>6 — Les avantages de l'automatisation</summary>

<br/>

| Avantage | Ce que ça change concrètement |
|---|---|
| **Moins d'erreurs humaines** | Les tâches répétitives sont scriptées : plus d'oubli d'étape |
| **Feedback rapide** | On sait en **minutes** si un changement casse quelque chose |
| **Déploiements fréquents** | On peut livrer **plusieurs fois par jour** en confiance |
| **Reproductibilité** | Chaque build est identique — fini le « ça marche sur ma machine » |
| **Traçabilité** | Chaque changement est enregistré : qui, quoi, quand |

```mermaid
flowchart LR
    A["Petits changements<br/>fréquents"] --> B["Détection rapide<br/>des problèmes"]
    B --> C["Correction facile<br/>(peu de code en jeu)"]
    C --> D["Confiance<br/>de l'équipe"]
    D --> A
```

> _Plus on déploie souvent, plus chaque déploiement est **petit**, donc **moins risqué**. C'est contre-intuitif : déployer plus souvent rend les déploiements plus sûrs, pas plus dangereux._

**🔧 Mini-exercice —** Citez deux raisons pour lesquelles déployer 10 fois par jour de **petits** changements est moins risqué qu'un seul **gros** déploiement par mois.

<details>
<summary>✅ Voir une solution</summary>

1) Chaque déploiement contient peu de code → un bug est **facile à localiser**. 2) Le **rollback** est simple (peu de changements à annuler). Le gros déploiement mensuel concentre au contraire beaucoup de risques d'un coup.

</details>

</details>

<p align="right"><a href="#top">↑ Retour en haut</a></p>

---

<a id="section-7"></a>

<details>
<summary>7 — Quiz — Concepts CI/CD</summary>

<br/>

**Question 1 :** Que signifie « CI » ?

a) Code Inspection

b) Continuous Integration

c) Container Initialization

d) Central Infrastructure

<details>
<summary>💡 Voir la solution</summary>

✅ **Réponse : b)** — *Continuous Integration* : fusion et validation automatiques fréquentes du code.

</details>

---

**Question 2 :** Quelle est la différence entre Continuous Delivery et Continuous Deployment ?

a) Aucune, ce sont des synonymes

b) En Delivery la mise en prod est manuelle ; en Deployment elle est automatique

c) Le Deployment ne fait pas de tests

d) La Delivery déploie automatiquement

<details>
<summary>💡 Voir la solution</summary>

✅ **Réponse : b)** — Les deux préparent une version prête ; seule la **mise en production** diffère (manuelle vs automatique).

</details>

---

**Question 3 :** Que se passe-t-il si le stage **Test** échoue dans un pipeline ?

a) Le pipeline continue quand même jusqu'en production

b) Le pipeline s'arrête et l'équipe est notifiée

c) Le code est supprimé du dépôt

d) Le pipeline recommence à l'infini

<details>
<summary>💡 Voir la solution</summary>

✅ **Réponse : b)** — Principe « fail fast » : un échec stoppe le pipeline et déclenche une alerte ; rien ne part en production.

</details>

---

**Question 4 :** Qu'est-ce qu'un **artefact** dans un pipeline ?

a) Un bug introduit par erreur

b) Le résultat empaqueté d'un build (ex. un `.jar`, une image)

c) Un message dans les logs

d) Un utilisateur du système

<details>
<summary>💡 Voir la solution</summary>

✅ **Réponse : b)** — L'artefact est le livrable produit par le build, prêt à être déployé.

</details>

---

**Question 5 :** Pourquoi déployer souvent rend-il les déploiements plus sûrs ?

a) Parce que chaque déploiement est plus petit et donc plus facile à diagnostiquer et à annuler

b) Parce qu'on supprime les tests

c) Parce que les serveurs deviennent plus puissants

d) Parce que les utilisateurs ne s'en aperçoivent pas

<details>
<summary>💡 Voir la solution</summary>

✅ **Réponse : a)** — De petits changements fréquents réduisent la surface de risque et facilitent le rollback.

</details>

</details>

<p align="right"><a href="#top">↑ Retour en haut</a></p>

---

<a id="section-8"></a>

<details>
<summary>8 — Pratique — Concevoir son premier pipeline</summary>

<br/>

### Consigne

Une équipe développe une application Java. Concevez (sur papier / en pseudo-pipeline) les **stages** d'un pipeline CI/CD pour cette application, en indiquant pour chaque stage : son **nom**, son **rôle**, et **ce qui doit arrêter le pipeline**. Précisez aussi si vous recommandez du *Continuous Delivery* ou du *Continuous Deployment*, et pourquoi.

---

### Correction proposée

| Ordre | Stage | Rôle | Condition d'arrêt |
|---|---|---|---|
| 1 | **Checkout** | Récupérer le code depuis Git | Dépôt inaccessible |
| 2 | **Build** | Compiler avec Maven (`mvn package`) | Erreur de compilation |
| 3 | **Test** | Lancer les tests JUnit | Un test échoue |
| 4 | **Package** | Produire l'artefact `.jar` / image | Échec d'empaquetage |
| 5 | **Staging** | Déployer en préproduction | Démarrage applicatif KO |
| 6 | **Deploy** | Mettre en production | Validation manuelle refusée |

**Choix recommandé pour débuter : Continuous Delivery.**

> _Justification : tant que la couverture de tests n'est pas mûre, on garde une **validation humaine** avant la production (Delivery). Quand l'équipe a confiance dans ses tests automatisés, elle peut passer en Continuous Deployment (mise en prod automatique)._

**Schéma attendu :**

```mermaid
flowchart LR
    A[Checkout] --> B[Build] --> C[Test] --> D[Package] --> E[Staging] --> F["Deploy (validation humaine)"]
```

</details>

<p align="right"><a href="#top">↑ Retour en haut</a></p>

---

<a id="section-9"></a>

<details>
<summary>9 — Synthèse</summary>

<br/>

#### Points à retenir

1. **CI/CD = l'automatisation du chemin du code à la production**, cœur technique du DevOps.
2. **CI** : fusionner et tester **souvent et automatiquement**.
3. **CD** : *Delivery* (prêt à déployer, bouton manuel) vs *Deployment* (mise en prod automatique).
4. **Le pipeline** enchaîne des **stages** ; un échec **arrête** tout (fail fast).
5. **Déployer souvent et petit** = moins de risque, feedback rapide, rollback facile.

```mermaid
mindmap
  root((CI/CD))
    CI
      Fusions fréquentes
      Build automatique
      Tests automatiques
    CD
      Delivery (manuel)
      Deployment (auto)
    Pipeline
      Checkout
      Build
      Test
      Package
      Deploy
    Bénéfices
      Feedback rapide
      Reproductibilité
      Traçabilité
```

#### La suite

Leçon **04 — Stratégies de déploiement** : comment passer de la v1 à la v2 en production sans casser le service (Blue/Green, Canary, Rolling…).

</details>

<p align="right"><a href="#top">↑ Retour en haut</a></p>

---

<p align="center">
  <em>Tous droits réservés. Toute reproduction, diffusion, utilisation ou adaptation de ce cours, en tout ou en partie, est strictement interdite sans l'autorisation écrite préalable de Dr. Haythem REHOUMA.</em>
</p>

<p align="center">
  <strong>Cours créé par Dr. Haythem REHOUMA — Développement et déploiement de solutions de données</strong>
</p>
