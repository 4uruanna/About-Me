# 🐳 Docker Cheatsheet

> *Cheatsheet complet des commandes Docker essentielles, inspiré de [docker.how](https://docker.how/)*

---

## 📋 Table des Matières

- [📦 Commandes de Base](#-commandes-de-base)
- [🖥️ Gestion des Conteneurs](#-gestion-des-conteneurs)
- [📦 Gestion des Images](#-gestion-des-images)
- [📜 Dockerfile](#-dockerfile)
- [🔗 Réseaux](#-réseaux)
- [💾 Volumes](#-volumes)
- [🎭 Docker Compose](#-docker-compose)
- [🔒 Sécurité](#-sécurité)
- [🧹 Nettoyage](#-nettoyage)
- [💡 Astuces](#-astuces)

## 📦 Commandes de Base


| Commande                  | Description                                 |
| ------------------------- | ------------------------------------------- |
| `docker --version`        | Affiche la version de Docker                |
| `docker info`             | Affiche les informations système de Docker  |
| `docker help`             | Affiche l'aide générale                     |
| `docker <command> --help` | Affiche l'aide pour une commande spécifique |


---

## 🖥️ Gestion des Conteneurs

### 🔄 Cycle de Vie


| Commande                                        | Description                                              |
| ----------------------------------------------- | -------------------------------------------------------- |
| `docker run [OPTIONS] IMAGE [COMMAND] [ARG...]` | Crée et démarre un conteneur                             |
| `docker run -d --name mon_app nginx`            | Démarre un conteneur en arrière-plan                     |
| `docker run -it ubuntu bash`                    | Démarre un conteneur interactif                          |
| `docker run -p 8080:80 nginx`                   | Mappe le port 8080 de l'hôte sur le port 80 du conteneur |
| `docker run -v /host:/container nginx`          | Monte un volume                                          |
| `docker run --rm nginx`                         | Supprime le conteneur après arrêt                        |
| `docker start <container>`                      | Démarre un conteneur arrêté                              |
| `docker stop <container>`                       | Arrête un conteneur en cours d'exécution                 |
| `docker restart <container>`                    | Redémarre un conteneur                                   |
| `docker pause <container>`                      | Met en pause un conteneur                                |
| `docker unpause <container>`                    | Reprend un conteneur en pause                            |
| `docker kill <container>`                       | Tue un conteneur de force                                |
| `docker rm <container>`                         | Supprime un conteneur                                    |
| `docker rm -f <container>`                      | Supprime un conteneur en force                           |


### 📊 Inspection


| Commande                             | Description                                           |
| ------------------------------------ | ----------------------------------------------------- |
| `docker ps`                          | Liste les conteneurs en cours d'exécution             |
| `docker ps -a`                       | Liste tous les conteneurs (y compris arrêtés)         |
| `docker ps -aq`                      | Liste tous les IDs de conteneurs                      |
| `docker ps --filter "status=exited"` | Liste les conteneurs avec un filtre                   |
| `docker inspect <container>`         | Affiche les détails d'un conteneur                    |
| `docker logs <container>`            | Affiche les logs d'un conteneur                       |
| `docker logs -f <container>`         | Suit les logs en temps réel                           |
| `docker logs --tail 100 <container>` | Affiche les 100 dernières lignes des logs             |
| `docker exec -it <container> bash`   | Ouvre un shell dans un conteneur en cours d'exécution |
| `docker exec <container> ls /app`    | Exécute une commande dans un conteneur                |
| `docker top <container>`             | Affiche les processus en cours dans un conteneur      |
| `docker stats`                       | Affiche les statistiques d'utilisation des ressources |
| `docker stats <container>`           | Statistiques pour un conteneur spécifique             |


### 🏷️ Gestion des Noms

```bash
# Renommer un conteneur
docker rename ancien_nom nouveau_nom

# Voir le nom d'un conteneur à partir de son ID
docker inspect --format '{{.Name}}' <container_id>
```

---

## 📦 Gestion des Images

### 📥 Téléchargement et Construction


| Commande                                 | Description                                  |
| ---------------------------------------- | -------------------------------------------- |
| `docker pull <image>`                    | Télécharge une image                         |
| `docker pull nginx:latest`               | Télécharge une image avec un tag spécifique  |
| `docker pull nginx:1.23`                 | Télécharge une version spécifique            |
| `docker build -t mon_image .`            | Construit une image à partir d'un Dockerfile |
| `docker build -t mon_image:1.0 .`        | Construit une image avec un tag              |
| `docker build --no-cache -t mon_image .` | Construit sans utiliser le cache             |
| `docker build -f Dockerfile.dev .`       | Utilise un Dockerfile personnalisé           |


### 📤 Upload et Partage

```bash
# Se connecter à Docker Hub
docker login

# Taguer une image pour Docker Hub
docker tag mon_image username/mon_image

# Pousser une image vers Docker Hub
docker push username/mon_image

# Pousser toutes les tags d'une image
docker push -a username/mon_image
```

### 📋 Liste et Inspection


| Commande                                 | Description                                        |
| ---------------------------------------- | -------------------------------------------------- |
| `docker images`                          | Liste toutes les images locales                    |
| `docker images -a`                       | Liste toutes les images (y compris intermédiaires) |
| `docker images --filter "dangling=true"` | Liste les images sans tag                          |
| `docker inspect <image>`                 | Affiche les détails d'une image                    |
| `docker history <image>`                 | Affiche l'historique d'une image                   |


### 🗑️ Nettoyage

```bash
# Supprimer une image
docker rmi <image>

# Supprimer une image de force
docker rmi -f <image>

# Supprimer toutes les images non utilisées
docker image prune

# Supprimer toutes les images non taguées (dangling)
docker image prune -a
```

---

## 📜 Dockerfile

### Structure de Base

```dockerfile
# Spécifie l'image de base
FROM ubuntu:22.04

# Définir le maintainer (déprécié, utiliser LABEL)
LABEL maintainer="nom@email.com"

# Exécute des commandes pendant la construction
RUN apt update && apt install -y curl

# Définit le répertoire de travail
WORKDIR /app

# Copie des fichiers depuis le host
COPY . .

# Ajoute des fichiers depuis une URL
ADD https://example.com/file.tar.gz /app/

# Définit les variables d'environnement
ENV NODE_ENV=production

# Expose un port
EXPOSE 8080

# Définit la commande par défaut
CMD ["npm", "start"]

# Définit le point d'entrée
ENTRYPOINT ["/app/start.sh"]

# Crée un utilisateur
RUN useradd -m myuser
USER myuser

# Utilise un volume
VOLUME ["/data"]

# Définit des arguments de construction
ARG VERSION=1.0
ENV APP_VERSION=$VERSION
```

### Bonnes Pratiques

```dockerfile
# Utiliser des images officielles et minimalistes
FROM node:18-alpine

# Regrouper les commandes pour réduire les couches
RUN apk add --no-cache python3 make g++ && \
    rm -rf /var/cache/apk/*

# Utiliser .dockerignore pour exclure des fichiers
# Créer un fichier .dockerignore avec:
# node_modules
# .git
# *.log

# Éviter de faire tourner le conteneur en root
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Utiliser des multi-stage builds pour réduire la taille
FROM node:18 as builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

### Multi-Stage Build

```dockerfile
# Étape de construction
FROM golang:1.21 as builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /app/server

# Étape finale
FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/server .
EXPOSE 8080
CMD ["./server"]
```

---

## 🔗 Réseaux

### Commandes de Base


| Commande                                          | Description                         |
| ------------------------------------------------- | ----------------------------------- |
| `docker network ls`                               | Liste tous les réseaux              |
| `docker network inspect <network>`                | Affiche les détails d'un réseau     |
| `docker network create <network>`                 | Crée un réseau                      |
| `docker network rm <network>`                     | Supprime un réseau                  |
| `docker network connect <network> <container>`    | Connecte un conteneur à un réseau   |
| `docker network disconnect <network> <container>` | Déconnecte un conteneur d'un réseau |


### Types de Réseaux

```bash
# Crée un réseau bridge personnalisé
docker network create mon_reseau

# Crée un réseau host (partage l'IP de l'hôte)
docker run --network host nginx

# Crée un réseau none (pas de réseau)
docker run --network none nginx

# Crée un réseau overlay (pour Swarm)
docker network create --driver overlay mon_overlay
```

### Exemple : Communication entre Conteneurs

```bash
# Créer un réseau
docker network create app_network

# Démarrer un conteneur Redis dans ce réseau
docker run -d --name redis --network app_network redis

# Démarrer un conteneur Node.js qui peut accéder à Redis
docker run -d --name app --network app_network -e REDIS_HOST=redis mon_app
```

---

## 💾 Volumes

### Commandes de Base


| Commande                         | Description                            |
| -------------------------------- | -------------------------------------- |
| `docker volume ls`               | Liste tous les volumes                 |
| `docker volume inspect <volume>` | Affiche les détails d'un volume        |
| `docker volume create <volume>`  | Crée un volume                         |
| `docker volume rm <volume>`      | Supprime un volume                     |
| `docker volume prune`            | Supprime tous les volumes non utilisés |


### Utilisation des Volumes

```bash
# Monter un volume nommé
docker run -v mon_volume:/data nginx

# Monter un volume anonyme
docker run -v /data nginx

# Monter un répertoire de l'hôte
docker run -v /host/path:/container/path nginx

# Monter un répertoire de l'hôte en lecture seule
docker run -v /host/path:/container/path:ro nginx

# Utiliser un volume avec un conteneur spécifique
docker run -v mon_volume:/app/data --name mon_app mon_image
```

### Bind Mount vs Volume


| Type           | Description                   | Persistance | Géré par Docker |
| -------------- | ----------------------------- | ----------- | --------------- |
| **Bind Mount** | Monte un répertoire de l'hôte | ✅ Oui       | ❌ Non           |
| **Volume**     | Stockage géré par Docker      | ✅ Oui       | ✅ Oui           |
| **tmpfs**      | Stockage en mémoire           | ❌ Non       | ❌ Non           |


### Exemple : Base de Données avec Volume

```bash
# Créer un volume pour PostgreSQL
docker volume create pg_data

# Démarrer PostgreSQL avec le volume
docker run -d --name pg \
  -e POSTGRES_PASSWORD=secret \
  -v pg_data:/var/lib/postgresql/data \
  postgres:14
```

---

## 🎭 Docker Compose

### Structure de Base

```yaml
version: '3.8'

services:
  web:
    image: nginx:alpine
    container_name: web_server
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
    environment:
      - NGINX_ENV=production
    restart: unless-stopped
    networks:
      - app_network

  db:
    image: postgres:14
    container_name: postgres_db
    environment:
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: myapp
    volumes:
      - pg_data:/var/lib/postgresql/data
    networks:
      - app_network

  redis:
    image: redis:alpine
    container_name: redis_cache
    networks:
      - app_network

volumes:
  pg_data:

networks:
  app_network:
    driver: bridge
```

### Commandes Compose


| Commande                             | Description                              |
| ------------------------------------ | ---------------------------------------- |
| `docker compose up`                  | Démarre tous les services                |
| `docker compose up -d`               | Démarre en arrière-plan                  |
| `docker compose down`                | Arrête et supprime les conteneurs        |
| `docker compose down -v`             | Supprime aussi les volumes               |
| `docker compose ps`                  | Liste les services en cours              |
| `docker compose logs`                | Affiche les logs de tous les services    |
| `docker compose logs -f`             | Suit les logs en temps réel              |
| `docker compose logs <service>`      | Affiche les logs d'un service spécifique |
| `docker compose exec <service> bash` | Exécute une commande dans un service     |
| `docker compose build`               | Reconstruit les images                   |
| `docker compose pull`                | Télécharge les images                    |
| `docker compose push`                | Pousse les images                        |
| `docker compose restart <service>`   | Redémarre un service                     |
| `docker compose stop <service>`      | Arrête un service                        |
| `docker compose start <service>`     | Démarre un service arrêté                |


### Variables d'Environnement

```bash
# Utiliser un fichier .env
docker compose --env-file .env.prod up

# Passer des variables directement
docker compose up -e DB_PASSWORD=secret
```

### Exemple : Stack Full-Stack

```yaml
version: '3.8'

services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend
    environment:
      - REACT_APP_API_URL=http://backend:5000

  backend:
    build: ./backend
    ports:
      - "5000:5000"
    depends_on:
      - db
      - redis
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
      - REDIS_URL=redis://redis:6379

  db:
    image: postgres:14
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:alpine
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

---

## 🔒 Sécurité

### Bonnes Pratiques

```bash
# Ne jamais faire tourner les conteneurs en root
# Dans le Dockerfile:
FROM alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Limiter les ressources
# Dans docker-compose.yml:
services:
  app:
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 512M

# Utiliser des images minimalistes
FROM alpine:latest  # au lieu de ubuntu

# Mettre à jour régulièrement les images
# Dans le Dockerfile:
FROM node:18-alpine
RUN apk update && apk upgrade

# Scanner les images pour les vulnérabilités
# Installer docker-scan plugin
docker scan nginx:latest

# Utiliser des secrets (Docker Swarm)
echo "mon_secret" | docker secret create db_password -
```

### Gestion des Secrets

```yaml
# docker-compose.yml avec secrets
version: '3.8'

services:
  db:
    image: postgres:14
    secrets:
      - db_password
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password

secrets:
  db_password:
    file: ./secrets/db_password.txt
```

---

## 🧹 Nettoyage

### Commandes de Nettoyage


| Commande                 | Description                                        |
| ------------------------ | -------------------------------------------------- |
| `docker system df`       | Affiche l'espace disque utilisé par Docker         |
| `docker system prune`    | Supprime tous les objets non utilisés              |
| `docker system prune -a` | Supprime TOUT (y compris les images non utilisées) |
| `docker container prune` | Supprime tous les conteneurs arrêtés               |
| `docker image prune`     | Supprime toutes les images non utilisées           |
| `docker volume prune`    | Supprime tous les volumes non utilisés             |
| `docker network prune`   | Supprime tous les réseaux non utilisés             |
| `docker builder prune`   | Supprime le cache de construction                  |


### Nettoyage Complet

```bash
# Supprimer TOUT : conteneurs, images, volumes, réseaux
docker system prune -a --volumes

# Supprimer les conteneurs arrêtés
docker rm $(docker ps -aq)

# Supprimer les images non utilisées
docker rmi $(docker images -q)

# Supprimer tous les volumes
docker volume rm $(docker volume ls -q)

# Supprimer tous les réseaux
docker network rm $(docker network ls -q)
```

---

## 💡 Astuces

### Alias Utiles

```bash
# Ajouter à votre ~/.bashrc ou ~/.zshrc

# Lister tous les conteneurs (y compris arrêtés)
alias docker-ps-all='docker ps -a'

# Nettoyer Docker
alias docker-clean='docker system prune -a --volumes'

# Voir les logs des conteneurs
alias docker-logs='docker logs -f'

# Entrer dans un conteneur
alias docker-enter='docker exec -it'

# Voir l'espace disque
alias docker-df='docker system df'

# Arrêter tous les conteneurs
alias docker-stop-all='docker stop $(docker ps -q)'

# Supprimer tous les conteneurs
alias docker-rm-all='docker rm $(docker ps -aq)'
```

### Debugging

```bash
# Vérifier si Docker fonctionne
docker run hello-world

# Tester la connectivité réseau dans un conteneur
docker run --rm busybox ping -c 4 google.com

# Vérifier les DNS dans un conteneur
docker run --rm busybox nslookup google.com

# Voir les processus Docker
docker stats

# Inspecter un conteneur pour voir la configuration réseau
docker inspect <container> | grep IPAddress

# Voir les ports mappés
docker port <container>
```

### Optimisation

```bash
# Utiliser le cache de construction
docker build --cache-from=mon_image:latest -t mon_image:new .

# Limiter la mémoire pendant la construction
docker build --memory=2g .

# Utiliser des tags multiples
docker build -t mon_image:latest -t mon_image:1.0 .

# Exporter un conteneur
docker export <container> > mon_conteneur.tar

# Importer un conteneur
docker import mon_conteneur.tar mon_image:latest

# Sauvegarder une image
docker save mon_image > mon_image.tar

# Charger une image
docker load < mon_image.tar
```

---

## 📚 Ressources Utiles

- [**Docker Documentation**](https://docs.docker.com/) - Documentation officielle
- [**Docker Hub**](https://hub.docker.com/) - Registry d'images Docker
- [**docker.how**](https://docker.how/) - Guide pratique Docker
- [**Docker Cheat Sheet (PDF)**](https://dockerlabs.collabnix.com/docker/cheatsheet/) - Cheatsheet officiel
- [**Docker Best Practices**](https://docs.docker.com/develop/dev-best-practices/) - Bonnes pratiques

---

## 🎯 Commandes les Plus Utilisées

```bash
# Démarrer un conteneur
docker run -d -p 8080:80 --name mon_app nginx

# Voir les conteneurs en cours
docker ps

# Voir tous les conteneurs
docker ps -a

# Arrêter un conteneur
docker stop mon_app

# Supprimer un conteneur
docker rm mon_app

# Construire une image
docker build -t mon_image .

# Lister les images
docker images

# Supprimer une image
docker rmi mon_image

# Voir les logs
docker logs -f mon_app

# Entrer dans un conteneur
docker exec -it mon_app bash
```

---

> *Cheatsheet créé par Adrien Villalonga - Inspiré de [docker.how](https://docker.how/)* 🚀