# Database Schema

## Entity Relationship Diagram

```
┌──────────────────┐
│      USERS       │
├──────────────────┤
│ id (PK)          │
│ username         │
│ email            │
│ password_hash    │
│ first_name       │
│ last_name        │
│ avatar_url       │
│ bio              │
│ city             │
│ latitude         │
│ longitude        │
│ phone            │
│ is_verified      │
│ reputation_score │
│ created_at       │
│ updated_at       │
└──────────────────┘
        │
        │ 1:N
        ▼
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│    LISTINGS      │     │  TASK_REQUESTS   │     │    OFFERINGS     │
├──────────────────┤     ├──────────────────┤     ├──────────────────┤
│ id (PK)          │     │ id (PK)          │     │ id (PK)          │
│ seller_id (FK)   │     │ creator_id (FK)  │     │ provider_id (FK) │
│ title            │     │ assigned_to (FK) │     │ title            │
│ description      │     │ title            │     │ description      │
│ price            │     │ description      │     │ category         │
│ category         │     │ budget           │     │ price            │
│ condition        │     │ category         │     │ price_type       │
│ images           │     │ latitude         │     │ latitude         │
│ status           │     │ longitude        │     │ longitude        │
│ created_at       │     │ address          │     │ experience       │
│ updated_at       │     │ status           │     │ availability     │
└──────────────────┘     │ deadline         │     │ status           │
                         │ created_at       │     │ created_at       │
                         └──────────────────┘     └──────────────────┘
                                  │
                                  │ 1:N
                                  ▼
                         ┌──────────────────┐
                         │ TASK_APPLICATIONS│
                         ├──────────────────┤
                         │ id (PK)          │
                         │ task_id (FK)     │
                         │ user_id (FK)     │
                         │ message          │
                         │ status           │
                         │ created_at       │
                         └──────────────────┘

┌──────────────────┐     ┌──────────────────┐
│     REVIEWS      │     │    FAVORITES     │
├──────────────────┤     ├──────────────────┤
│ id (PK)          │     │ id (PK)          │
│ reviewer_id (FK) │     │ user_id (FK)     │
│ reviewed_user_id │     │ item_type        │
│ rating           │     │ item_id          │
│ content          │     │ created_at       │
│ created_at       │     └──────────────────┘
│ updated_at       │
└──────────────────┘
```

---

## Table Details

### Users

| Column | Type | Constraints |
|--------|------|-------------|
| id | INTEGER | PRIMARY KEY |
| username | VARCHAR(80) | UNIQUE, NOT NULL |
| email | VARCHAR(120) | UNIQUE, NOT NULL |
| password_hash | VARCHAR(256) | NOT NULL |
| first_name | VARCHAR(50) | |
| last_name | VARCHAR(50) | |
| avatar_url | VARCHAR(256) | |
| bio | TEXT | |
| city | VARCHAR(100) | |
| latitude | FLOAT | |
| longitude | FLOAT | |
| phone | VARCHAR(20) | |
| is_verified | BOOLEAN | DEFAULT FALSE |
| reputation_score | FLOAT | DEFAULT 0.0 |
| created_at | DATETIME | DEFAULT NOW |
| updated_at | DATETIME | |

### Listings

| Column | Type | Constraints |
|--------|------|-------------|
| id | INTEGER | PRIMARY KEY |
| seller_id | INTEGER | FOREIGN KEY (users.id) |
| title | VARCHAR(200) | NOT NULL |
| description | TEXT | |
| price | DECIMAL(10,2) | NOT NULL |
| category | VARCHAR(50) | NOT NULL |
| condition | VARCHAR(20) | |
| images | JSON | |
| status | VARCHAR(20) | DEFAULT 'active' |
| created_at | DATETIME | DEFAULT NOW |
| updated_at | DATETIME | |

### Task Requests

| Column | Type | Constraints |
|--------|------|-------------|
| id | INTEGER | PRIMARY KEY |
| creator_id | INTEGER | FOREIGN KEY (users.id) |
| assigned_to_id | INTEGER | FOREIGN KEY (users.id), NULLABLE |
| title | VARCHAR(200) | NOT NULL |
| description | TEXT | |
| budget | DECIMAL(10,2) | NOT NULL |
| category | VARCHAR(50) | |
| latitude | FLOAT | NOT NULL |
| longitude | FLOAT | NOT NULL |
| address | VARCHAR(256) | |
| status | VARCHAR(30) | DEFAULT 'open' |
| deadline | DATETIME | |
| created_at | DATETIME | DEFAULT NOW |

**Status values**: open, assigned, pending_confirmation, completed, cancelled, disputed

### Task Applications

| Column | Type | Constraints |
|--------|------|-------------|
| id | INTEGER | PRIMARY KEY |
| task_id | INTEGER | FOREIGN KEY (task_requests.id) |
| user_id | INTEGER | FOREIGN KEY (users.id) |
| message | TEXT | |
| status | VARCHAR(20) | DEFAULT 'pending' |
| created_at | DATETIME | DEFAULT NOW |

**Status values**: pending, accepted, rejected

### Offerings

| Column | Type | Constraints |
|--------|------|-------------|
| id | INTEGER | PRIMARY KEY |
| provider_id | INTEGER | FOREIGN KEY (users.id) |
| title | VARCHAR(200) | NOT NULL |
| description | TEXT | |
| category | VARCHAR(50) | NOT NULL |
| price | DECIMAL(10,2) | |
| price_type | VARCHAR(20) | DEFAULT 'hourly' |
| latitude | FLOAT | |
| longitude | FLOAT | |
| experience | TEXT | |
| availability | VARCHAR(256) | |
| status | VARCHAR(20) | DEFAULT 'active' |
| created_at | DATETIME | DEFAULT NOW |

**Price types**: hourly, fixed, negotiable

### Reviews

| Column | Type | Constraints |
|--------|------|-------------|
| id | INTEGER | PRIMARY KEY |
| reviewer_id | INTEGER | FOREIGN KEY (users.id) |
| reviewed_user_id | INTEGER | FOREIGN KEY (users.id) |
| rating | INTEGER | CHECK (1-5) |
| content | TEXT | |
| created_at | DATETIME | DEFAULT NOW |
| updated_at | DATETIME | |

### Favorites

| Column | Type | Constraints |
|--------|------|-------------|
| id | INTEGER | PRIMARY KEY |
| user_id | INTEGER | FOREIGN KEY (users.id) |
| item_type | VARCHAR(20) | NOT NULL |
| item_id | INTEGER | NOT NULL |
| created_at | DATETIME | DEFAULT NOW |

**Item types**: task, offering, listing

**Unique constraint**: (user_id, item_type, item_id)

---

## Indexes

```sql
-- Users
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);

-- Listings
CREATE INDEX idx_listings_seller ON listings(seller_id);
CREATE INDEX idx_listings_category ON listings(category);
CREATE INDEX idx_listings_status ON listings(status);

-- Tasks
CREATE INDEX idx_tasks_creator ON task_requests(creator_id);
CREATE INDEX idx_tasks_assigned ON task_requests(assigned_to_id);
CREATE INDEX idx_tasks_location ON task_requests(latitude, longitude);
CREATE INDEX idx_tasks_status ON task_requests(status);

-- Offerings
CREATE INDEX idx_offerings_provider ON offerings(provider_id);
CREATE INDEX idx_offerings_category ON offerings(category);
CREATE INDEX idx_offerings_location ON offerings(latitude, longitude);

-- Reviews
CREATE INDEX idx_reviews_reviewed ON reviews(reviewed_user_id);

-- Favorites
CREATE INDEX idx_favorites_user ON favorites(user_id);
```

---

## Migrations

Currently using auto-create with `db.create_all()`. For production:

```bash
# Setup Flask-Migrate
pip install Flask-Migrate

# Initialize
flask db init
flask db migrate -m "Initial migration"
flask db upgrade
```
