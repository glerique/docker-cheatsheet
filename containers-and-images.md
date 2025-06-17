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