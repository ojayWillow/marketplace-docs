# Development Setup Guide

## Prerequisites

- Python 3.9+
- Node.js 18+
- Git
- Docker Desktop (optional, for PostgreSQL + Redis)

---

## Backend Setup

### 1. Clone Repository

```bash
git clone https://github.com/ojayWillow/marketplace-backend.git
cd marketplace-backend
```

### 2. Create Virtual Environment

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Mac/Linux
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configure Environment

```bash
cp .env.example .env
# Edit .env with your settings
```

### 5. Start Database (Optional Docker)

```bash
# Using Docker
docker-compose up -d

# Or use local SQLite (default, no setup needed)
```

### 6. Run Server

```bash
python wsgi.py
# API at http://localhost:5000
```

### 7. Test

```bash
curl http://localhost:5000/health
# Should return: {"status": "ok"}
```

---

## Frontend Setup

### 1. Clone Repository

```bash
git clone https://github.com/ojayWillow/marketplace-frontend.git
cd marketplace-frontend
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment

```bash
cp .env.example .env
# Set VITE_API_URL=http://localhost:5000
```

### 4. Run Development Server

```bash
npm run dev
# UI at http://localhost:5173
```

---

## Troubleshooting

### psycopg2 Installation Failed (Windows)

Use `psycopg2` instead of `psycopg2-binary`:

```bash
pip install psycopg2==2.9.6
```

### Cannot Connect to Database

1. Check Docker is running: `docker ps`
2. Verify PostgreSQL container: `docker-compose ps`
3. Check .env DATABASE_URL
4. Restart Docker: `docker-compose down && docker-compose up -d`

### Port 5000 Already in Use

```bash
# Windows
netstat -ano | findstr :5000
taskkill /PID <pid> /F

# Mac/Linux
lsof -i :5000
kill -9 <pid>
```

### Module Not Found Errors

```bash
# Ensure venv is activated
venv\Scripts\activate  # Windows
source venv/bin/activate  # Mac/Linux

# Reinstall dependencies
pip install -r requirements.txt
```

### CORS Errors

Check frontend is running on allowed port (5173 or 3000). Backend CORS config in `app/__init__.py`.

---

## Database Credentials (Development)

| Setting | Value |
|---------|-------|
| Host | localhost |
| Port | 5432 |
| Database | marketplace_db |
| Username | marketplace_user |
| Password | marketplace_password |

---

## Redis (Development)

| Setting | Value |
|---------|-------|
| Host | localhost |
| Port | 6379 |
| Database | 0 |

---

## Running Tests

### Backend

```bash
cd marketplace-backend
pip install pytest pytest-cov
pytest -v
```

### Frontend

```bash
cd marketplace-frontend
npm test
```

---

## Project Structure

```
marketplace-backend/
├── app/
│   ├── __init__.py
│   ├── models/
│   └── routes/
├── tests/
├── uploads/
├── requirements.txt
├── wsgi.py
└── docker-compose.yml

marketplace-frontend/
├── src/
│   ├── components/
│   ├── pages/
│   ├── stores/
│   └── i18n/
├── public/
├── package.json
└── vite.config.ts
```

---

## Useful Commands

```bash
# Backend
python wsgi.py          # Start server
pytest -v               # Run tests
pytest --cov=app        # Coverage report

# Frontend
npm run dev             # Development server
npm run build           # Production build
npm run preview         # Preview production build
```
