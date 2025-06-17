# Docker Cheat Sheet - Gestion des Conteneurs et Images

## 🔄 Gestion des Conteneurs Docker

### Cycle de vie d'un conteneur
```
Created → Running → Stopped → Removed
    ↓         ↓         ↓         ↓
 docker run  En cours  docker stop  docker rm
```

### Commandes de base

#### `docker run` - Créer et démarrer un conteneur
```bash
# Syntaxe de base
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

# Exemples pratiques
docker run -d nginx                           # Démarrer en arrière-plan
docker run -it ubuntu bash                   # Mode interactif avec terminal
docker run --name mon-nginx nginx            # Nommer le conteneur
docker run --rm alpine echo "Hello"          # Suppression automatique après arrêt
docker run -d -p 8080:80 nginx              # Mapping de port
docker run -e ENV_VAR=value alpine          # Variable d'environnement
```

#### `docker start/stop` - Contrôler les conteneurs existants
```bash
docker start CONTAINER_ID                    # Démarrer un conteneur arrêté
docker start mon-conteneur                   # Démarrer par nom
docker stop CONTAINER_ID                     # Arrêter proprement (SIGTERM puis SIGKILL)
docker stop $(docker ps -q)                 # Arrêter tous les conteneurs actifs
docker restart CONTAINER_ID                 # Redémarrer
```

#### `docker ps` - Lister les conteneurs
```bash
docker ps                                    # Conteneurs en cours d'exécution
docker ps -a                                # Tous les conteneurs (actifs + arrêtés)
docker ps -q                                # Afficher seulement les IDs
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"  # Format personnalisé
```

#### `docker logs` - Consulter les logs
```bash
docker logs CONTAINER_ID                     # Afficher tous les logs
docker logs -f CONTAINER_ID                 # Suivre les logs en temps réel (tail -f)
docker logs --tail 50 CONTAINER_ID          # Afficher les 50 dernières lignes
docker logs --since 1h CONTAINER_ID         # Logs depuis 1 heure
docker logs -t CONTAINER_ID                 # Avec timestamps
```

#### `docker exec` - Exécuter des commandes dans un conteneur actif
```bash
docker exec -it CONTAINER_ID bash           # Ouvrir un shell interactif
docker exec CONTAINER_ID ls -la             # Exécuter une commande simple
docker exec -u root CONTAINER_ID bash       # Exécuter en tant que root
docker exec -w /app CONTAINER_ID pwd        # Définir le répertoire de travail
```

### Mapping des ports
```bash
# Syntaxe : -p HOST_PORT:CONTAINER_PORT
docker run -p 8080:80 nginx                 # Port 8080 de l'hôte → port 80 du conteneur
docker run -p 127.0.0.1:8080:80 nginx      # Lier à une IP spécifique
docker run -p 8080-8090:80 nginx           # Range de ports
docker run -P nginx                         # Mapping automatique des ports exposés

# Vérifier les mappings
docker port CONTAINER_ID
```

### 🌍 Variables d'environnement
```bash
# Définir une variable
docker run -e DATABASE_URL=mysql://... app

# Définir plusieurs variables
docker run -e DB_HOST=localhost -e DB_PORT=3306 app

# Utiliser un fichier d'environnement
docker run --env-file .env app

# Exemple de fichier .env
# DB_HOST=localhost
# DB_PORT=3306
# DEBUG=true
```

## 🐳 Gestion des Images Docker

### Structure des images (Layers)
```
Image = Base Layer + Layer 1 + Layer 2 + ... + Layer N
        |           |         |         |     |
        Ubuntu      Install   Copy      CMD   Final Layer
                   packages   files
```

### Télécharger des images depuis Docker Hub
```bash
docker pull IMAGE_NAME                       # Télécharger la dernière version (latest)
docker pull IMAGE_NAME:TAG                   # Télécharger une version spécifique
docker pull nginx:1.21-alpine              # Exemple avec tag spécifique
docker pull --all-tags nginx               # Télécharger tous les tags disponibles

# Rechercher des images
docker search nginx                         # Rechercher des images nginx
```

### Lister et supprimer des images
```bash
# Lister les images
docker images                               # Toutes les images locales
docker images nginx                         # Images nginx uniquement
docker images --filter "dangling=true"     # Images orphelines
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"

# Supprimer des images
docker rmi IMAGE_ID                         # Supprimer une image
docker rmi nginx:latest                     # Supprimer par nom:tag
docker rmi $(docker images -q)             # Supprimer toutes les images
docker image prune                          # Supprimer les images orphelines
docker image prune -a                       # Supprimer toutes les images inutilisées
```

### 🏷️ Système de tags
```bash
# Structure : REPOSITORY:TAG
nginx:latest                                # Tag par défaut
nginx:1.21                                 # Version spécifique
nginx:1.21-alpine                          # Version + variante

# Créer des tags personnalisés
docker tag nginx:latest mon-nginx:v1.0     # Créer un nouveau tag
docker tag IMAGE_ID mon-app:production     # Tag à partir d'un ID

# Conventions de nommage
registry.com/user/app:version              # Registry privé
user/app:tag                               # Docker Hub personnel
app:tag                                    # Local seulement
```

## 🛠️ Commandes utiles supplémentaires

### Nettoyage système
```bash
docker system df                            # Utilisation d'espace disque
docker system prune                         # Nettoyer les ressources inutilisées
docker system prune -a                      # Nettoyage agressif (tout supprimer)
docker container prune                      # Supprimer les conteneurs arrêtés
docker volume prune                         # Supprimer les volumes inutilisés
docker network prune                        # Supprimer les réseaux inutilisés
```

### Informations système
```bash
docker info                                 # Informations système Docker
docker version                              # Version de Docker
docker stats                                # Statistiques des conteneurs en temps réel
docker inspect CONTAINER_ID                 # Détails complets d'un conteneur
```