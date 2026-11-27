# Iris MLOps – Training · FastAPI · Frontend · Docker

Projet de TP : entraîner un classifieur Iris, le servir via une API FastAPI et le tester depuis une interface web, le tout orchestré avec Docker Compose et versionné avec Git.

## Architecture

```
training/  -> entraîne le modèle, écrit models/model.joblib + metadata.json
backend/   -> API FastAPI (charge le modèle, expose /predict)
frontend/  -> Nginx : page web + reverse proxy /api -> backend:8000
models/    -> volume partagé (artefacts, ignoré par Git)
```

## Démarrage rapide (WSL2 + Docker)

```bash
git clone https://github.com/SplatBurger/iris-mlops.git && cd iris-mlops
export GIT_COMMIT=$(git rev-parse --short HEAD)
docker compose up -d --build
```

| Service  | URL                          |
|----------|------------------------------|
| Frontend | http://localhost:8080        |
| API docs | http://localhost:8000/docs   |
| Health   | http://localhost:8000/health |

## Commandes utiles

```bash
make help     # liste des commandes
make train    # ré-entraîne le modèle
make test     # tests training + API
make logs     # logs de la stack
make down     # arrêt
```

## Exemple d'appel API

```bash
curl -X POST http://localhost:8000/predict \
  -H "Content-Type: application/json" \
  -d '{"sepal_length":5.1,"sepal_width":3.5,"petal_length":1.4,"petal_width":0.2}'
```
