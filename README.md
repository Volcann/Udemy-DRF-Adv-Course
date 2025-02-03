# Recipe API with Authentication Token

This project is a **Recipe API** built with **Django REST Framework (DRF)**, using **Docker** and **PostgreSQL** as the database. The API includes authentication and authorization using token-based authentication and supports managing recipes, ingredients, tags, and user accounts.

## Features
- Token-based authentication for secure access
- User management (signup, login, authentication)
- CRUD operations for recipes, ingredients, and tags
- Group-based permissions
- PostgreSQL as the database
- Docker and Docker Compose for easy deployment

## Technologies Used
- Python 3
- Django & Django REST Framework
- PostgreSQL
- Docker & Docker Compose

---

## Installation and Setup

### Prerequisites
Ensure you have the following installed:
- **Docker** and **Docker Compose**
- **Python 3** (if running without Docker)

### Clone the Repository
```sh
git clone https://github.com/yourusername/recipe-api.git
cd recipe-api
```

### Setup with Docker
```sh
docker-compose up --build
```

This will:
- Build the Docker images
- Start the PostgreSQL database container
- Start the Django API container

### Running Migrations
Once the containers are running, apply database migrations:
```sh
docker-compose exec web python manage.py migrate
```

### Creating a Superuser
To create an admin user:
```sh
docker-compose exec web python manage.py createsuperuser
```

---

## Authentication & Authorization
This API uses **Token-based authentication**. To obtain an authentication token:

### Obtain Auth Token
```sh
POST /api/user/token/
```
Request:
```json
{
    "email": "user@example.com",
    "password": "password123"
}
```
Response:
```json
{
    "token": "your-auth-token"
}
```
Include this token in the `Authorization` header for authenticated requests:
```sh
Authorization: Token your-auth-token
```

---

## API Endpoints

### User Management
| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/user/create/` | POST | Register a new user |
| `/api/user/token/` | POST | Obtain authentication token |
| `/api/user/me/` | GET | Retrieve user details |
| `/api/user/me/` | PATCH | Update user details |

### Recipe Management
| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/recipe/` | GET | List all recipes |
| `/api/recipe/` | POST | Create a new recipe |
| `/api/recipe/{id}/` | GET | Retrieve recipe details |
| `/api/recipe/{id}/` | PUT | Update a recipe |
| `/api/recipe/{id}/` | DELETE | Delete a recipe |

### Ingredients
| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/ingredient/` | GET | List all ingredients |
| `/api/ingredient/` | POST | Add a new ingredient |

### Tags
| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/tag/` | GET | List all tags |
| `/api/tag/` | POST | Create a new tag |

---

## Managing Groups and Permissions
Django's built-in group-based permissions allow restricting access:
```sh
docker-compose exec web python manage.py shell
```
Inside Django shell:
```python
from django.contrib.auth.models import Group, User
admin_group = Group.objects.get(name='Admin')
user = User.objects.get(username='john_doe')
user.groups.add(admin_group)
```
This assigns `john_doe` to the `Admin` group.

---

## Running Tests
To run automated tests:
```sh
docker-compose exec web python manage.py test
```

---

## Deployment
To deploy the project:
```sh
docker-compose up --build -d
```
This runs the project in detached mode.

For production, consider using **Gunicorn** and **NGINX**.
