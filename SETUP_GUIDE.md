# Backend Setup Guide - Windows Development

## Problem Solved
✅ Fixed `psycopg2-binary` compatibility error on Windows
- Changed from `psycopg2-binary==2.9.6` to `psycopg2==2.9.6`
- psycopg2 (without -binary suffix) has pre-compiled wheels for Windows

## Prerequisites
- Python 3.9+
- Git
- Docker Desktop (for PostgreSQL + Redis, or use local installations)

## Step-by-Step Setup

### 1. Clone and Navigate
```bash
git clone https://github.com/ojayWillow/marketplace-backend.git
cd marketplace-backend
```

### 2. Create Virtual Environment
```bash
python -m venv venv
venv\\Scripts\\activate  # On Windows PowerShell
# OR
venv\\Scripts\\activate.bat  # On Windows CMD
```

### 3. Install Dependencies
```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Setup Environment Variables
```bash
cp .env.example .env
# Edit .env and update PostgreSQL/Redis connection strings if needed
```

### 5. Start Database Services

**Option A: Using Docker (Recommended)**
```bash
docker-compose up -d
# Wait 10 seconds for containers to start
```

**Option B: Local PostgreSQL/Redis**
- PostgreSQL: Default at localhost:5432
- Redis: Default at localhost:6379

### 6. Run Flask Application
```bash
python wsgi.py
```

You should see:
```
 * Running on http://0.0.0.0:5000
```

### 7. Test the API
```bash
curl http://localhost:5000/health
```

Expected response:
```json
{"status": "ok"}
```

## Troubleshooting

### Error: "psycopg2 installation failed"
- Ensure you're using `psycopg2` (NOT `psycopg2-binary`)
- Run: `pip install --upgrade pip setuptools wheel`
- Then: `pip install psycopg2==2.9.6`

### Error: "Cannot connect to database"
- Check Docker is running: `docker ps`
- Verify PostgreSQL container: `docker-compose ps`
- Check .env DATABASE_URL is correct
- Try Docker Compose again: `docker-compose down && docker-compose up -d`

### Error: "Port 5000 already in use"
- Kill existing process on port 5000
- Or change FLASK port in wsgi.py

### ModuleNotFoundError: No module named 'flask_sqlalchemy'
- Ensure venv is activated
- Run `pip install -r requirements.txt` again

## Next Steps

1. Create database models (models/user.py, models/listings.py)
2. Build auth routes (core/auth.py)
3. Build classifieds routes (classifieds/routes.py)
4. Build tasks routes (tasks/routes.py)
5. Test all endpoints with Postman

## Project Structure
```
marketplace-backend/
├── app/
│   ├── __init__.py          # Flask app factory
│   ├── core/               # Auth, utils
│   ├── classifieds/        # Buy/sell endpoints
│   ├── tasks/              # Quick help endpoints
│   └── models/             # Database models
├── requirements.txt        # Dependencies
├── .env.example           # Environment template
├── docker-compose.yml     # Database services
└── wsgi.py                # Entry point
```

## Database Info
- Username: `marketplace_user`
- Password: `marketplace_password`
- Database: `marketplace_db`
- Host: `localhost` (or `db` inside Docker)
- Port: `5432`

## Redis Info
- Host: `localhost` (or `redis` inside Docker)
- Port: `6379`
- Database: `0`
