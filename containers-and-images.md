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