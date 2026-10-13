# FastAPI Blog API

A simple REST API for managing blog posts, built with FastAPI and SQLAlchemy.

## Features

- Create, read, update and delete blog posts
- Pagination and title search on the blog list
- JWT-protected routes for creating, updating and deleting posts
- Interactive API docs (Swagger UI) at `/docs`
- Secrets loaded from environment variables

## Tech stack

- Python 3
- FastAPI
- SQLAlchemy
- python-jose (JWT)
- Uvicorn

## Getting started

### 1. Clone the repo

```bash
git clone https://github.com/neroxes/FastAPI-blog-API.git
cd FastAPI-blog-API
```

### 2. Create a virtual environment and install dependencies

```bash
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # macOS / Linux

pip install -r requirements.txt
```

### 3. Configure environment variables

Copy `.env.example` to `.env` and fill in your own values:

```
DATABASE_URL=mysql+pymysql://user:password@localhost/dbname
SECRET_KEY=change-me
```

Generate a strong secret key with:

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

If your database password contains special characters (such as `@`), URL-encode them (`@` becomes `%40`).

### 4. Run the server

```bash
python -m uvicorn main:app --reload
```

Open http://127.0.0.1:8000/docs to try the API.

## Endpoints

| Method | Path           | Auth | Description                               |
| ------ | -------------- | ---- | ----------------------------------------- |
| GET    | `/`            | No   | Welcome message                           |
| POST   | `/login`       | No   | Get an access token                       |
| GET    | `/blogs`       | No   | List blogs (`page`, `limit`, `search`)    |
| GET    | `/blogs/{id}`  | No   | Get one blog                              |
| POST   | `/blogs`       | Yes  | Create a blog                             |
| PUT    | `/blogs/{id}`  | Yes  | Update a blog                             |
| DELETE | `/blogs/{id}`  | Yes  | Delete a blog                             |

### Example request body (create / update)

```json
{
  "title": "My first blog",
  "content": "Hello, this is my first post.",
  "author": "Thomas"
}
```

### Example list response

`GET /blogs?page=1&limit=5&search=first`

```json
{
  "page": 1,
  "limit": 5,
  "total": 1,
  "data": [
    {
      "id": 1,
      "title": "My first blog",
      "content": "Hello, this is my first post.",
      "author": "Thomas"
    }
  ]
}
```

## Authentication

Protected routes expect an `Authorization: Bearer <token>` header. In Swagger UI, call `POST /login`, copy the `access_token`, and use the **Authorize** button.

> **Note:** `/login` currently issues a token without checking credentials. This is a learning project; user accounts with hashed passwords are on the roadmap.

## Project structure

```
.
├── main.py          # FastAPI app and routes
├── auth.py          # JWT creation and verification
├── database.py      # Engine, session and Base
├── models.py        # SQLAlchemy models
├── schemas.py       # Pydantic schemas
├── requirements.txt
└── .env.example
```

## Roadmap

- [ ] User registration and login with hashed passwords
- [ ] Link blogs to their owners and restrict edits to the owner
- [ ] Tests
- [ ] Docker setup
