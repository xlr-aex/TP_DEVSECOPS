<img width="937" height="362" alt="image" src="https://github.com/user-attachments/assets/4f00e2dc-273d-4b41-a512-852130030d07" />
<img width="1915" height="945" alt="image" src="https://github.com/user-attachments/assets/d609271a-652e-43f5-9b5a-a3088887870b" />
<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/3ee780d5-1d92-4463-aba7-8b99e7296de5" />
<img width="1918" height="961" alt="image" src="https://github.com/user-attachments/assets/9a31f360-2164-4fa7-8b94-7244973c0492" />

# Node.js Application - DevSecOps & Monitoring

[![DevSecOps Pipeline](https://github.com/xlr-aex/TP_DEVSECOPS/actions/workflows/devsecops.yml/badge.svg)](https://github.com/xlr-aex/TP_DEVSECOPS/actions/workflows/devsecops.yml)

Ce dépôt contient le code source et la configuration système pour le TP DevSecOps et Monitoring. 

## Structure du Projet

L'application est une API Node.js utilisant `express` et `prom-client` pour exposer des métriques Prometheus personnalisées (`http_requests_total` et `http_request_duration_seconds`).

* `app.js` : Le serveur et la configuration des métriques. (Inclut une route `/error` de test pour visualiser les taux d'erreur 5xx).
* `Dockerfile` : Utilise `node:20-alpine`, multistage et un utilisateur système non-root `appuser`.
* `docker-compose.yml` : La stack complète (App, Prometheus, Grafana, Node Exporter).
* `.github/workflows/devsecops.yml` : Le pipeline CI/CD avec :
    - Build & Test
    - Linting avec Hadolint
    - Scan de vulnérabilités avec Trivy
    - Analyse statique de code avec GitHub CodeQL
    - Déploiement vers GHCR (GitHub Container Registry)

## Déploiement Local

Pour lancer l'application et la stack de monitoring locale :

```bash
docker compose up -d --build
```

### URLs Locales :
* Application Node.js : [http://localhost:3000](http://localhost:3000)
* Endpoint Metrics de l'App : [http://localhost:3000/metrics](http://localhost:3000/metrics)
* Prometheus : [http://localhost:9090](http://localhost:9090)
* Grafana : [http://localhost:3001](http://localhost:3001) (Credentials: `admin` / `admin123`)

## Dashboards de Monitoring (Grafana)

La partie monitoring est gérée par Grafana, qui récupère ses données depuis la base de données temporelle locale Prometheus. Deux tableaux de bord principaux sont configurés.

### 1. Dashboard Application (Node.js App - Monitoring)
Ce tableau de bord personnalisé affiche les performances RED (Rate, Errors, Duration) de l'API Node.js :
* **Requêtes par seconde** (Rate)
* **Taux d'erreur HTTP** (Errors - statuts 5xx)
* **Latence p95** en millisecondes (Duration)

![Dashboard Application Node.js](images/nodejs_dashboard.png)

### 2. Dashboard Système (Node Exporter Full)
Les ressources de la machine hôte Docker (CPU, RAM, Réseau, Disque) sont surveillées grâce au tableau de bord communautaire (ID 1860).

![Dashboard Système Node Exporter](images/node_exporter.png)



