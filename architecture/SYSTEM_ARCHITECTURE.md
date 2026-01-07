# System Architecture

## Overview

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Frontend      │────▶│   Backend       │────▶│   Database      │
│   (React)       │     │   (Flask)       │     │   (PostgreSQL)  │
│   Port 5173     │     │   Port 5000     │     │                 │
└─────────────────┘     └─────────────────┘     └─────────────────┘
         │                       │
         │                       ▼
         │              ┌─────────────────┐
         │              │   File Storage  │
         │              │   (Local/S3)    │
         │              └─────────────────┘
         │
         ▼
┌─────────────────┐
│   Map Tiles     │
│   (OpenStreetMap)│
└─────────────────┘
```

---

## Request Flow: User Registration

```
Client POST /api/auth/register
    ↓
wsgi.py → create_app()
    ↓
app/__init__.py
    ├─ Flask(__name__)
    ├─ SQLAlchemy init
    ├─ CORS setup
    └─ register_routes()
    ↓
app/routes/auth.py
    ├─ Validate input
    ├─ Check uniqueness
    ├─ Hash password
    └─ Create User
    ↓
app/models/user.py
    ├─ set_password()
    └─ to_dict()
    ↓
Response: 201 + user data
```

---

## Database Schema

### Tables

| Table | Description |
|-------|-------------|
| `users` | User accounts |
| `listings` | Buy/sell items |
| `task_requests` | Quick help jobs |
| `task_applications` | Job applications |
| `offerings` | Service offerings |
| `reviews` | User reviews |
| `favorites` | Saved items |

### Key Relationships

```
User (1) ──── (N) Listing
User (1) ──── (N) TaskRequest (as creator)
User (1) ──── (N) TaskRequest (as assigned_to)
User (1) ──── (N) TaskApplication
User (1) ──── (N) Offering
User (1) ──── (N) Review (as reviewer)
User (1) ──── (N) Review (as reviewed_user)
User (1) ──── (N) Favorite
```

---

## Authentication Flow

```
┌────────┐     ┌────────┐     ┌────────┐
│ Login  │────▶│ Verify │────▶│ Token  │
│ Form   │     │ Creds  │     │ (JWT)  │
└────────┘     └────────┘     └────────┘
                                  │
                                  ▼
┌────────┐     ┌────────┐     ┌────────┐
│ Store  │◀────│ Return │◀────│ Sign   │
│ Token  │     │ Token  │     │ w/Key  │
└────────┘     └────────┘     └────────┘
```

### JWT Structure
```json
{
  "user_id": 123,
  "exp": 1704672000
}
```

### Protected Routes
```python
@token_required
def protected_endpoint(current_user):
    # current_user is injected from token
    pass
```

---

## Task Workflow

```
┌────────┐     ┌────────┐     ┌────────┐     ┌────────┐
│  OPEN  │────▶│ASSIGNED│────▶│PENDING │────▶│COMPLETE│
│        │     │        │     │CONFIRM │     │        │
└────────┘     └────────┘     └────────┘     └────────┘
     │              │              │
     │              │              ▼
     │              │         ┌────────┐
     │              │         │DISPUTED│
     │              │         └────────┘
     ▼              ▼
┌────────┐     ┌────────┐
│CANCELED│     │CANCELED│
└────────┘     └────────┘
```

---

## Location Search

Uses Haversine formula for radius-based search:

```python
def haversine(lat1, lon1, lat2, lon2):
    R = 6371  # Earth radius in km
    # ... calculate distance
    return distance_km

# Query tasks within 10km
tasks = [t for t in all_tasks 
         if haversine(user_lat, user_lon, t.lat, t.lon) <= 10]
```

---

## File Upload Flow

```
┌────────┐     ┌────────┐     ┌────────┐
│ Select │────▶│Validate│────▶│  Save  │
│ File   │     │ Type   │     │ Local  │
└────────┘     └────────┘     └────────┘
                                  │
                                  ▼
                              ┌────────┐
                              │ Return │
                              │  URL   │
                              └────────┘
```

### Validation Rules
- Max size: 5MB
- Allowed types: jpg, jpeg, png, gif, webp
- Unique filename generation

---

## CORS Configuration

```python
CORS(app, resources={
    r"/api/*": {
        "origins": ["http://localhost:3000", "http://localhost:5173"],
        "methods": ["GET", "POST", "PUT", "DELETE"],
        "allow_headers": ["Content-Type", "Authorization"]
    }
})
```

---

## Environment Variables

```env
# Backend
FLASK_ENV=development
DATABASE_URL=sqlite:///marketplace.db
JWT_SECRET_KEY=your-secret-key

# Frontend
VITE_API_URL=http://localhost:5000
```

---

## Production Considerations

### Database
- Switch to PostgreSQL
- Add connection pooling
- Set up migrations (Flask-Migrate)

### Storage
- Move uploads to AWS S3
- Use CloudFront CDN

### Security
- Rate limiting
- Input sanitization
- HTTPS only

### Performance
- Redis for caching
- Database indexing
- Query optimization
