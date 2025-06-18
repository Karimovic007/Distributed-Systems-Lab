# Distributed Systems Lab - Item Management System

A cloud-native application implementing a distributed item management system with separate frontend (Nuxt.js 3) and backend (FastAPI) services, following the 12-factor app methodology.

## 📋 Table of Contents

- [Features](#-features)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Development Setup](#-development-setup)
  - [Backend Setup (FastAPI)](#backend-setup-with-virtual-environment)
  - [Frontend Setup (Nuxt.js)](#frontend-setup)
- [Docker Deployment](#-docker-deployment)
- [Kubernetes Deployment](#-kubernetes-deployment)
- [API Documentation](#-api-documentation)
- [12-Factor Implementation](#-12-factor-implementation)
- [Testing](#-testing)
- [Container Images](#-container-images)

## 🚀 Features

- **Frontend**: Server-Side Rendering (SSR) with Nuxt.js 3
- **Backend**: RESTful API with FastAPI and automatic OpenAPI documentation
- **Containerization**: Docker and Docker Compose ready
- **Orchestration**: Kubernetes manifests with HPA
- **Configuration**: Fully externalized configuration (12-factor compliant)
- **CORS**: Configurable cross-origin resource sharing
- **Health Checks**: Built-in health endpoints for monitoring

## 🏗️ Architecture

```
┌─────────────────┐         ┌─────────────────┐
│   Nuxt.js 3     │  HTTP   │    FastAPI      │
│   Frontend      │ ──────> │    Backend      │
│   (Port 3000)   │         │   (Port 8080)   │
└─────────────────┘         └─────────────────┘
```

## 📋 Prerequisites

- **Python 3.9+** (for backend)
- **Node.js 18+** (for frontend)
- **Docker & Docker Compose**
- **Kubernetes** (optional, for K8s deployment)
- **Git**

## 🚀 Quick Start

### Using Docker Compose (Recommended)

```bash
# Clone the repository
git clone https://github.com/Yahyasarmini/Distributed-Systems-Lab
cd Distributed-Systems-Lab

# Start all services
docker-compose up

# Access the application
# Frontend: http://localhost:3000
# Backend API: http://localhost:8080
# API Docs: http://localhost:8080/docs
```

## 🛠️ Development Setup

### Backend Setup with Virtual Environment

1. **Navigate to backend directory**
```bash
cd backend
```

2. **Create a Python virtual environment**

**Windows:**
```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
venv\Scripts\activate
```

**macOS/Linux:**
```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate
```

3. **Install dependencies**
```bash
# Ensure pip is up to date
pip install --upgrade pip

# Install requirements
pip install -r requirements.txt
```

4. **Set environment variables**
```bash
# Windows (PowerShell)
$env:PORT = "8080"
$env:HOST = "0.0.0.0"
$env:ENVIRONMENT = "development"
$env:ALLOWED_ORIGINS = "*"

# macOS/Linux
export PORT=8080
export HOST=0.0.0.0
export ENVIRONMENT=development
export ALLOWED_ORIGINS="*"
```

5. **Run the backend**
```bash
# With uvicorn directly
uvicorn main:app --reload --host 0.0.0.0 --port 8080

# Or using the Python script
python main.py
```

6. **Access API documentation**
- Swagger UI: http://localhost:8080/docs
- ReDoc: http://localhost:8080/redoc

7. **Deactivate virtual environment (when done)**
```bash
deactivate
```

### Frontend Setup

1. **Navigate to frontend directory**
```bash
cd nuxt-app
```

2. **Install dependencies**
```bash
npm install
# or
yarn install
```

3. **Configure environment**
```bash
# Create .env file
cp .env.example .env

# Edit .env file with your configuration
NUXT_PUBLIC_API_URL=http://localhost:8080
NUXT_PUBLIC_APP_NAME=Item Management System
```

4. **Run development server**
```bash
npm run dev
# or
yarn dev
```

5. **Access frontend**
````
- Application: http://localhost:3000
````

### Environment Variables

Create a `.env` file in the root directory:

```env
# Backend
ENVIRONMENT=production
PORT=8080
HOST=0.0.0.0
LOG_LEVEL=info
ALLOWED_ORIGINS=*

# Frontend
NODE_ENV=production
NUXT_PUBLIC_API_URL=http://fastapi-backend:8080
NUXT_PUBLIC_APP_NAME=Item Management System
API_SECRET=your-secret-key
```

## ☸️ Kubernetes Deployment

### Deploy to Kubernetes

```bash
# Create namespace
kubectl apply -f k8s/namespace.yaml

# Apply all manifests
kubectl apply -k k8s/

# Or apply individually
kubectl apply -f backend/k8s/
kubectl apply -f nuxt-app/k8s/
```

### Verify Deployment

```bash
# Check all resources
kubectl get all -n items-app

# View logs
kubectl logs -n items-app -l app=items-backend
kubectl logs -n items-app -l app=items-frontend

# Port forward for local access
kubectl port-forward -n items-app service/frontend-service 3000:3000
kubectl port-forward -n items-app service/backend-service 8080:8080
```

## 📚 API Documentation

The backend automatically generates OpenAPI documentation:

- **Swagger UI**: http://localhost:8080/docs
- **ReDoc**: http://localhost:8080/redoc

### API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/items` | Get all items |
| POST | `/items` | Create new item |
| GET | `/items/{id}` | Get item by ID |
| PUT | `/items/{id}` | Update item |
| DELETE | `/items/{id}` | Delete item |
| GET | `/health` | Health check |

### Example Requests

```bash
# Get all items
curl http://localhost:8080/items

# Create item
curl -X POST http://localhost:8080/items \
  -H "Content-Type: application/json" \
  -d '{"name":"Apple","quantity":42}'

# Update item
curl -X PUT http://localhost:8080/items/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Green Apple","quantity":50}'

# Delete item
curl -X DELETE http://localhost:8080/items/1
```

## 📝 12-Factor Implementation

This application follows the [12-factor app methodology](https://12factor.net/):

1. **Codebase**: Single codebase tracked in Git
2. **Dependencies**: Explicitly declared in requirements.txt/package.json
3. **Config**: Configuration via environment variables
4. **Backing services**: No database in this lab (stateless)
5. **Build, release, run**: Separated via Docker
6. **Processes**: Stateless processes
7. **Port binding**: Services bind to ports via env vars
8. **Concurrency**: Horizontal scaling via replicas
9. **Disposability**: Fast startup/graceful shutdown
10. **Dev/prod parity**: Docker ensures consistency
11. **Logs**: Logs to stdout/stderr
12. **Admin processes**: Not applicable for this lab

## 🧪 Testing

### Backend Tests

```bash
cd backend
# Activate virtual environment first
source venv/bin/activate  # Linux/macOS
# or
venv\Scripts\activate  # Windows

# Run tests
pytest
```

### Manual Testing

1. **Create Item**: Click "Add Item" and fill the form
2. **List Items**: View all items in the grid
3. **Edit Item**: Click "Edit" on any item
4. **Delete Item**: Click "Delete" to remove an item

## 🐋 Container Images
#Frontend image:
```
docker pull ghcr.io/karimovic007/distributed-systems-lab/distributed-systems-lab-backend:latest
````
#Backend image:
````
docker pull ghcr.io/karimovic007/distributed-systems-lab/distributed-systems-lab-backend:latest
````
Clone or download this repository.

## ✅ 2. Run with Docker Compose

```bash
docker compose up --build
```

This will:

    ✅ Build and start both the backend and frontend containers

    🔗 Bind the backend to http://localhost:8080

    🔗 Bind the frontend to http://localhost:3000

3. Test the Application
```
    🌐 Frontend UI: http://localhost:3000

    🧪 Backend health check: http://localhost:8080/health
```


## Useful Docker Compose Commands
````


| Command                      | Description                     |
|-----------------------------|---------------------------------|
| `docker compose up --build` | Build and start all services    |
| `docker compose down`       | Stop and remove all containers  |
| `docker compose ps`         | List running services           |
| `docker compose logs -f`    | Live logs                       |

`````

## 📄 Project Structure

```
Distributed-Systems-Lab/
├── backend/
│   ├── main.py              # FastAPI application
│   ├── requirements.txt     # Python dependencies
│   ├── Dockerfile          # Backend container definition
│   ├── test_main.py        # Backend tests
│   └── k8s/                # Backend K8s manifests
├── nuxt-app/
│   ├── pages/              # Vue pages
│   ├── components/         # Vue components
│   ├── Dockerfile          # Frontend container definition
│   ├── package.json        # Node dependencies
│   └── k8s/                # Frontend K8s manifests
├── docker-compose.yml      # Multi-container setup
├── REST_API.json          # API specification
└── README.md              # This file
```

## 📄 License

This project is part of the Distributed Systems Lab course.

## 👥 Team

- Yahya Sarmini
- Karim ashraf

---
