# Marketplace Platform Documentation

**Last Updated**: January 7, 2026

Centralized documentation for the Latvian Marketplace platform - a dual-segment marketplace with Buy/Sell classifieds and Quick Help services.

---

## 📁 Documentation Structure

```
marketplace-docs/
├── README.md                    # This file - project overview
├── backend/                     # Backend (Flask) documentation
│   ├── PROJECT_STATUS.md        # Current backend status & progress
│   ├── DEVELOPMENT_ROADMAP.md   # Backend development phases
│   ├── API_ENDPOINTS.md         # Complete API reference
│   ├── TESTING_GUIDE.md         # Running tests with pytest
│   └── SESSION_LOGS.md          # Development session history
├── frontend/                    # Frontend (React) documentation
│   ├── ROADMAP.md               # Frontend development phases
│   ├── MAP_INTEGRATION.md       # Leaflet map setup guide
│   └── QUICKHELP_SETUP.md       # Quick Help feature docs
├── architecture/                # System-wide documentation
│   ├── SYSTEM_ARCHITECTURE.md   # Technical architecture
│   └── DATABASE_SCHEMA.md       # Database models & relationships
└── setup/                       # Getting started guides
    └── SETUP_GUIDE.md           # Windows development setup
```

---

## 🚀 Project Overview

### What is this?

A marketplace platform for Latvia with three main segments:

1. **Buy/Sell Classifieds** - Traditional listings like ss.lv
2. **Quick Help (Jobs)** - Task marketplace with interactive map
3. **Service Offerings** - Advertise skills and services

### Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | React 18 + TypeScript + Vite |
| **Styling** | Tailwind CSS |
| **Maps** | Leaflet + react-leaflet |
| **State** | Zustand |
| **Backend** | Flask 3.x + Python |
| **Database** | SQLite (dev) / PostgreSQL (prod) |
| **Auth** | JWT (PyJWT) |
| **Testing** | pytest + pytest-cov |

### Repositories

| Repository | Description | Status |
|------------|-------------|--------|
| [marketplace-frontend](https://github.com/ojayWillow/marketplace-frontend) | React web UI | ✅ MVP Complete |
| [marketplace-backend](https://github.com/ojayWillow/marketplace-backend) | Flask REST API | ✅ MVP Complete |
| [marketplace-docs](https://github.com/ojayWillow/marketplace-docs) | Documentation (this repo) | 📚 Active |

---

## 📊 Current Status

### Backend - 100% MVP Complete
- ✅ User authentication (JWT)
- ✅ Buy/Sell listings (full CRUD)
- ✅ Quick Help tasks (full workflow)
- ✅ Service Offerings (full CRUD)
- ✅ Reviews & ratings
- ✅ File uploads
- ✅ Location-based search
- ✅ Automated test suite

### Frontend - 97% MVP Complete
- ✅ Authentication UI
- ✅ Classifieds browsing & management
- ✅ Interactive map with Leaflet
- ✅ Task creation & workflow
- ✅ Service offerings
- ✅ Reviews system
- ✅ i18n (LV/RU/EN)
- ✅ SEO & accessibility

---

## 🏃 Quick Start

### Backend
```bash
cd marketplace-backend
python -m venv venv
venv\Scripts\activate  # Windows
pip install -r requirements.txt
python wsgi.py
# API at http://localhost:5000
```

### Frontend
```bash
cd marketplace-frontend
npm install
npm run dev
# UI at http://localhost:5173
```

---

## 📖 Documentation Index

### Getting Started
- [Setup Guide](setup/SETUP_GUIDE.md) - Local development setup

### Backend
- [Project Status](backend/PROJECT_STATUS.md) - Current state & features
- [Development Roadmap](backend/DEVELOPMENT_ROADMAP.md) - Phases & progress
- [API Endpoints](backend/API_ENDPOINTS.md) - Complete API reference
- [Testing Guide](backend/TESTING_GUIDE.md) - Running pytest

### Frontend
- [Roadmap](frontend/ROADMAP.md) - Development phases
- [Map Integration](frontend/MAP_INTEGRATION.md) - Leaflet setup
- [Quick Help Setup](frontend/QUICKHELP_SETUP.md) - Task feature docs

### Architecture
- [System Architecture](architecture/SYSTEM_ARCHITECTURE.md) - Technical overview
- [Database Schema](architecture/DATABASE_SCHEMA.md) - Models & relationships

---

## 🎯 Next Steps

### High Priority
1. **Phone number authentication** - SMS verification for trust
2. **Messaging system** - Real-time chat between users
3. **Favorites/Watchlist** - Save items for later

### Medium Priority
4. **Payment integration** - Stripe for task escrow
5. **Email notifications** - Task updates
6. **Admin dashboard** - Content moderation

### Future
7. **PWA support** - Offline mode, installable
8. **Cloud storage** - AWS S3 for images
9. **Analytics** - Usage tracking

---

## 📝 Contributing

When updating documentation:
1. Keep timestamps updated (`Last Updated: ...`)
2. Update progress percentages when features complete
3. Add session logs for significant changes
4. Cross-reference related documents

---

**Maintained by**: [@ojayWillow](https://github.com/ojayWillow)
