# 🐳 Portfolio — Application Dockerisée

Application web full-stack de **portfolio personnel** permettant de gérer et présenter ses projets. Le projet est entièrement conteneurisé avec **Docker** et orchestré via **Docker Compose**.

---

## 📋 Table des matières

- [Architecture](#-architecture)
- [Technologies](#-technologies)
- [Prérequis](#-prérequis)
- [Installation & Lancement](#-installation--lancement)
- [Endpoints de l'API](#-endpoints-de-lapi)
- [Structure du projet](#-structure-du-projet)
- [Variables d'environnement](#-variables-denvironnement)
- [Auteur](#-auteur)

---

## 🏗 Architecture

Le projet repose sur une architecture **3-tiers** conteneurisée :

```
┌────────────────┐     ┌────────────────┐     ┌────────────────┐
│   Frontend     │────▶│    Backend     │────▶│   MongoDB      │
│  React + Vite  │     │  Express.js    │     │   (Mongo 6)    │
│   port 5173    │     │   port 3000    │     │  port 27018    │
└────────────────┘     └────────────────┘     └────────────────┘
```

Les trois services communiquent via un réseau Docker interne (`portfolio-network`).

---

## 🛠 Technologies

| Couche     | Technologies                                |
|------------|---------------------------------------------|
| Frontend   | React 19, React Router 7, Vite 8            |
| Backend    | Node.js, Express 5, Mongoose 9, Multer      |
| Base de données | MongoDB 6                              |
| DevOps     | Docker, Docker Compose                      |

---

## ✅ Prérequis

- [Docker](https://docs.docker.com/get-docker/) (v20+)
- [Docker Compose](https://docs.docker.com/compose/install/) (v2+)

---

## 🚀 Installation & Lancement

**1. Cloner le dépôt**

```bash
git clone <URL_DU_DEPOT>
cd PROJET_DOCKER
```

**2. Lancer l'ensemble des services**

```bash
docker compose up --build
```

**3. Accéder à l'application**

| Service   | URL                          |
|-----------|------------------------------|
| Frontend  | http://localhost:5173         |
| Backend   | http://localhost:3000         |
| MongoDB   | `mongodb://localhost:27018`  |

**4. Arrêter les services**

```bash
docker compose down
```

> 💡 Pour supprimer également les volumes (données MongoDB & uploads) :
> ```bash
> docker compose down -v
> ```

---

## 📡 Endpoints de l'API

Base URL : `http://localhost:3000/api/projects`

| Méthode  | Route              | Description                    |
|----------|--------------------|--------------------------------|
| `GET`    | `/`                | Récupérer tous les projets     |
| `GET`    | `/:id`             | Récupérer un projet par son ID |
| `POST`   | `/`                | Ajouter un nouveau projet      |
| `PUT`    | `/:id`             | Modifier un projet existant    |
| `DELETE` | `/:id`             | Supprimer un projet            |

### Modèle de données — `Project`

| Champ          | Type       | Requis | Description                        |
|----------------|------------|--------|------------------------------------|
| `titre`        | String     | ✅     | Titre du projet                    |
| `description`  | String     | ✅     | Description détaillée              |
| `technologies` | [String]   | ✅     | Liste des technologies utilisées   |
| `lien`         | String     | ❌     | Lien vers le projet (URL)          |
| `image`        | String     | ❌     | Nom du fichier image (via upload)  |

> Les images sont uploadées via `multipart/form-data` (champ `image`) et stockées dans le volume `uploads_data`.

---

## 📁 Structure du projet

```
PROJET_DOCKER/
├── docker-compose.yml          # Orchestration des 3 services
│
├── portfolio-api/              # Backend — API REST
│   ├── Dockerfile
│   ├── app.js                  # Point d'entrée du serveur
│   ├── config/
│   │   ├── connectdb.js        # Connexion MongoDB
│   │   └── upload.js           # Configuration Multer (uploads)
│   ├── controllers/
│   │   └── projectController.js
│   ├── models/
│   │   └── project.js          # Schéma Mongoose
│   ├── routes/
│   │   └── projectRoutes.js    # Routes CRUD
│   ├── uploads/                # Stockage des images
│   ├── package.json
│   ├── .dockerignore
│   └── .gitignore
│
├── portfolio-react/            # Frontend — SPA React
│   ├── Dockerfile
│   ├── index.html
│   ├── vite.config.js
│   ├── src/
│   │   ├── main.jsx
│   │   ├── App.jsx             # Routes & layout principal
│   │   ├── App.css
│   │   ├── index.css
│   │   └── components/
│   │       ├── Navbar.jsx
│   │       ├── Footer.jsx
│   │       ├── Dossier.jsx         # Liste des projets
│   │       ├── Projet.jsx          # Carte de projet
│   │       ├── AjouterProjet.jsx   # Formulaire d'ajout
│   │       └── DetaillerProjet.jsx # Page détail d'un projet
│   ├── package.json
│   ├── .dockerignore
│   └── .gitignore
│
└── README.md
```

---

## 🔐 Variables d'environnement

### Backend (`portfolio-api/.env`)

| Variable    | Valeur par défaut                        | Description                |
|-------------|------------------------------------------|----------------------------|
| `PORT`      | `3000`                                   | Port du serveur Express    |
| `MONGO_URI` | `mongodb://localhost:27017/portfolio`     | URI de connexion MongoDB   |

> ⚠️ En mode Docker, la variable `MONGO_URI` est surchargée dans le `docker-compose.yml` pour pointer vers le conteneur `mongodb`.

---

## 👤 Auteur

Projet réalisé dans le cadre de la formation **AWS — ODC** (Orange Digital Center).

---

## 📄 Licence

Ce projet est à usage éducatif.
