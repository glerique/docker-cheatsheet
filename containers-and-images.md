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

