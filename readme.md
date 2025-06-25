# Blog API

Une API REST minimaliste pour la gestion de posts de blog, construite avec **Django & Django REST Framework** et protégée par une authentification **Token**. Le projet sert de démonstration compacte : bonnes pratiques Django, séparation claire des apps (`posts`, `users`), permissions objet, et endpoints prêts pour un frontend ou une intégration mobile.

- **CRUD sécurisé :** chaque post est lié à son auteur (`created_by`) ; seules les personnes authentifiées peuvent créer/modifier/supprimer leur propre contenu.
- **Auth par Token :** endpoints d’inscription et de connexion renvoyant un token à inclure dans l’en‑tête `Authorization`.
- **Code modulaire :** logique utilisateur dans l’app `users`, logique métier dans l’app `posts`.
---

## Stack & dépendances

| Outil                         | Version        | Rôle           |
| ----------------------------- | -------------- | -------------- |
| Python                        | ≥ 3.11         | Langage        |
| Django                        | 5.x            | Framework Web  |
| Django REST Framework         | 3.x            | API REST       |
| djangorestframework‑authtoken | fourni par DRF | Auth par token |

---

## Setup rapide

```bash
# 1. Cloner le repo
$ git clone https://github.com/mamadou288/blog_api.git
$ cd blog_api

# 2. Créer un environnement virtuel
$ python -m venv .venv
$ source .venv/bin/activate  # Windows : .venv\Scripts\activate

# 3. Installer les dépendances
$ pip install -r requirements.txt  # ou pip install django djangorestframework

# 4. Initialiser la base de données
$ python manage.py migrate

# 6. Lancer le serveur
$ python manage.py runserver
# API accessible sur http://127.0.0.1:8000/api/
```

---

## 🔑 Endpoints principaux

### Auth

| Méthode | URL                   | Corps JSON                                       | Réponse (succès)         |
| ------- | --------------------- | ------------------------------------------------ | ------------------------ |
| `POST`  | `/api/auth/register/` | `{ "username": "alice", "password": "pass123" }` | `{ "token": "<token>" }` |
| `POST`  | `/api/auth/login/`    | `{ "username": "alice", "password": "pass123" }` | `{ "token": "<token>" }` |

> **Header à ajouter ensuite :** `Authorization: Token <token>`

### Posts

| Méthode  | URL                | Auth requise | Description                     |
| -------- | ------------------ | ------------ | ------------------------------- |
| `GET`    | `/api/posts/`      | ❌            | Liste tous les posts            |
| `POST`   | `/api/posts/`      | ✅            | Créer un post (titre + contenu) |
| `GET`    | `/api/posts/<id>/` | ❌            | Détails d’un post               |
| `PUT`    | `/api/posts/<id>/` | ✅ (auteur)   | Mettre à jour son post          |
| `DELETE` | `/api/posts/<id>/` | ✅ (auteur)   | Supprimer son post              |

---

## `curl`

### 1. Inscription

```bash
curl -X POST http://127.0.0.1:8000/api/auth/register/ \
     -H "Content-Type: application/json" \
     -d '{"username": "alice", "password": "pass123"}'
```

### 2. Connexion

```bash
curl -X POST http://127.0.0.1:8000/api/auth/login/ \
     -H "Content-Type: application/json" \
     -d '{"username": "alice", "password": "pass123"}'
# Réponse → { "token": "abc123..." }
```

### 3. Création d’un post

```bash
curl -X POST http://127.0.0.1:8000/api/posts/ \
     -H "Content-Type: application/json" \
     -H "Authorization: Token abc123..." \
     -d '{"title": "Hello", "content": "Mon premier post"}'
```

### 4. Liste des posts

```bash
curl http://127.0.0.1:8000/api/posts/
```

---

## pistes d’amélioration

- Endpoint `GET /api/auth/me/` pour retourner l’utilisateur courant
- Mise en place de tests unitaires avec Pytest
- Déploiement 

---

