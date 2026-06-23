# Aide-mémoire des commandes (Webhook GitHub → Jenkins local via ngrok)

> Ouvrez un terminal **dans ce dossier** (`projet06-jenkins-webhook-ngrok`), là où se trouve `docker-compose.yml`.

## Préparer le jeton ngrok (une seule fois)

```bash
cp .env.example .env
# puis editez .env et collez votre NGROK_AUTHTOKEN
# (jeton gratuit : https://dashboard.ngrok.com/get-started/your-authtoken)
```

## Démarrer Jenkins + ngrok

```bash
docker compose up -d --build
```

- Jenkins : **http://localhost:8080** (aucun mot de passe : assistant désactivé).
- Tableau de bord ngrok (URL publique) : **http://localhost:4040**.

> **Le port 8080 est déjà occupé ?** Voir l'**[Annexe — Le port 8080 est déjà occupé ?](#annexe--le-port-8080-est-déjà-occupé-)** en fin de document.

## Récupérer l'URL publique ngrok (en ligne de commande)

```bash
curl -s http://localhost:4040/api/tunnels
```

L'URL `https://....ngrok-free.app` est dans le champ `public_url`. Le **Payload URL** du webhook = cette URL **+ `/github-webhook/`**.

## Vérifier les outils dans le conteneur Jenkins

```bash
docker exec jenkins-webhook-tp which java     # /opt/java/openjdk/bin/java
docker exec jenkins-webhook-tp which python3  # /usr/bin/python3
docker exec jenkins-webhook-tp which git      # /usr/bin/git
```

## Voir les logs

```bash
docker compose logs -f jenkins
docker compose logs -f ngrok
```

## Déclencher un build par webhook (test)

```bash
cd depot-exemple
echo "// test webhook" >> HelloWorld.java
git add .
git commit -m "Test webhook"
git push origin main
# -> un build doit demarrer tout seul dans Jenkins
```

## Tester le code en local (sans Jenkins)

```bash
# Java
docker run --rm -v "${PWD}/depot-exemple:/app" -w /app eclipse-temurin:17-jdk \
  bash -lc "javac HelloWorld.java && java HelloWorld"

# Python
docker run --rm -v "${PWD}/depot-exemple:/app" -w /app python:3.12-slim python3 hello.py
```

## Arrêter / réinitialiser

```bash
docker compose stop          # arreter (donnees conservees)
docker compose down          # supprimer les conteneurs (volume conserve)
docker compose down -v       # tout supprimer (config Jenkins incluse)
```

---

## Annexe — Le port 8080 est déjà occupé ?

Si `docker compose up` affiche une erreur du type :

```text
Error response from daemon: ports are not available: exposing port TCP 0.0.0.0:8080 ...
bind: Only one usage of each socket address ... is normally permitted.
```

…c'est qu'**une autre application utilise déjà le port 8080** (souvent un autre Jenkins, Tomcat ou un serveur Java déjà lancé).

Vous avez **deux solutions** au choix : **arrêter le processus** qui occupe le port (Solution A), ou **utiliser un autre port** sans rien arrêter (Solution B).

### Solution A — Trouver et arrêter le processus qui occupe le port 8080

**Étape 1 — repérer le programme.** Avant de tuer un processus, vérifiez d'abord s'il s'agit d'un conteneur Docker.

```bash
docker ps                    # un conteneur apparait-il sur le port 8080 ?
```

- **Si c'est un conteneur Docker**, arrêtez-le proprement :

```bash
docker stop <nom>            # ex: docker stop jenkins-webhook-tp
```

- **Sinon** (programme classique sur le PC), suivez les commandes selon votre système.

**Windows (PowerShell) :**

```powershell
# 1. Trouver le PID (numero de processus) qui ecoute sur le port 8080
Get-NetTCPConnection -LocalPort 8080 -State Listen | Select-Object OwningProcess

# 2. Voir de quel programme il s'agit (remplacez 8144 par le PID trouve)
Get-Process -Id 8144

# 3. Arreter ce processus (remplacez 8144 par le PID trouve)
Stop-Process -Id 8144 -Force
```

**Windows (ancien `cmd`) :**

```bat
:: 1. Trouver le PID qui ecoute sur 8080 (derniere colonne = PID)
netstat -ano | findstr :8080

:: 2. Arreter ce processus (remplacez 8144 par le PID trouve)
taskkill /PID 8144 /F
```

**macOS / Linux :**

```bash
# 1. Trouver le PID qui occupe le port 8080
lsof -i :8080
# 2. Arreter ce processus (remplacez 8144 par le PID trouve)
kill -9 8144
```

**Étape 2 — relancer :**

```bash
docker compose up -d --build
```

### Solution B — Utiliser un autre port (sans rien arrêter)

Dans `docker-compose.yml`, changez le **port de gauche** du service `jenkins` (celui du PC) :

```yaml
    ports:
      - "8081:8080"   # 8081 = port sur votre PC, 8080 = port interne de Jenkins
```

> Le service `ngrok` pointe vers `jenkins:8080` (port **interne**) : il n'y a **rien à changer** côté ngrok, l'URL publique continue de fonctionner.

Puis relancez et ouvrez **http://localhost:8081** :

```bash
docker compose up -d --build
```

---

<p align="center">
  <strong>Cours créé par Dr. Haythem REHOUMA — Développement et déploiement de solutions de données</strong>
</p>
