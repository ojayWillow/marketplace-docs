# API Endpoints Reference

**Base URL**: `http://localhost:5000/api`

---

## Authentication

### Register
```http
POST /auth/register
Content-Type: application/json

{
  "username": "string",
  "email": "string",
  "password": "string",
  "first_name": "string",
  "last_name": "string"
}
```

### Login
```http
POST /auth/login
Content-Type: application/json

{
  "email": "string",
  "password": "string"
}
```

**Response**:
```json
{
  "token": "jwt_token_here",
  "user": { ... }
}
```

### Get Profile
```http
GET /auth/profile
Authorization: Bearer <token>
```

### Update Profile
```http
PUT /auth/profile
Authorization: Bearer <token>
Content-Type: application/json

{
  "first_name": "string",
  "last_name": "string",
  "bio": "string",
  "city": "string"
}
```

### Get Public User Profile
```http
GET /auth/users/:id
```

### Get User Reviews
```http
GET /auth/users/:id/reviews
```

---

## Listings

### Browse Listings
```http
GET /listings?page=1&per_page=10&category=electronics&status=active
```

### Create Listing
```http
POST /listings
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "string",
  "description": "string",
  "price": 100.00,
  "category": "string",
  "condition": "new|used|like_new",
  "images": ["url1", "url2"]
}
```

### Get Listing
```http
GET /listings/:id
```

### Update Listing
```http
PUT /listings/:id
Authorization: Bearer <token>
```

### Delete Listing
```http
DELETE /listings/:id
Authorization: Bearer <token>
```

---

## Tasks (Quick Help)

### Browse Tasks
```http
GET /tasks?latitude=56.9496&longitude=24.1052&radius=10
```

### Create Task
```http
POST /tasks
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "string",
  "description": "string",
  "budget": 50.00,
  "category": "string",
  "latitude": 56.9496,
  "longitude": 24.1052,
  "address": "string",
  "deadline": "2026-01-15T10:00:00Z"
}
```

### Apply for Task
```http
POST /tasks/:id/apply
Authorization: Bearer <token>
Content-Type: application/json

{
  "message": "I can help with this!"
}
```

### Accept Application
```http
POST /tasks/:id/accept-application
Authorization: Bearer <token>
Content-Type: application/json

{
  "application_id": 123
}
```

### Mark Task Done (Worker)
```http
POST /tasks/:id/done
Authorization: Bearer <token>
```

### Confirm Completion (Creator)
```http
POST /tasks/:id/confirm
Authorization: Bearer <token>
```

### Dispute Task (Creator)
```http
POST /tasks/:id/dispute
Authorization: Bearer <token>
Content-Type: application/json

{
  "reason": "string"
}
```

### My Assigned Tasks
```http
GET /tasks/my
Authorization: Bearer <token>
```

### My Created Tasks
```http
GET /tasks/created
Authorization: Bearer <token>
```

---

## Offerings

### Browse Offerings
```http
GET /offerings?latitude=56.9496&longitude=24.1052&radius=10&category=cleaning
```

### Create Offering
```http
POST /offerings
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "string",
  "description": "string",
  "category": "string",
  "price": 25.00,
  "price_type": "hourly|fixed|negotiable",
  "latitude": 56.9496,
  "longitude": 24.1052,
  "experience": "string",
  "availability": "string"
}
```

### Get/Update/Delete Offering
```http
GET /offerings/:id
PUT /offerings/:id
DELETE /offerings/:id
```

### My Offerings
```http
GET /offerings/my
Authorization: Bearer <token>
```

---

## Reviews

### Create Review
```http
POST /reviews
Authorization: Bearer <token>
Content-Type: application/json

{
  "reviewed_user_id": 123,
  "rating": 5,
  "content": "Great experience!"
}
```

### Get/Update/Delete Review
```http
GET /reviews/:id
PUT /reviews/:id
DELETE /reviews/:id
```

---

## Favorites

### Toggle Favorite
```http
POST /favorites
Authorization: Bearer <token>
Content-Type: application/json

{
  "item_type": "task|offering|listing",
  "item_id": 123
}
```

### Get My Favorites
```http
GET /favorites
Authorization: Bearer <token>
```

### Check If Favorited
```http
GET /favorites/check?item_type=task&item_id=123
Authorization: Bearer <token>
```

### Remove Favorite
```http
DELETE /favorites/:id
Authorization: Bearer <token>
```

---

## Uploads

### Upload Image
```http
POST /uploads/image
Authorization: Bearer <token>
Content-Type: multipart/form-data

file: <image file>
```

### Get Uploaded File
```http
GET /uploads/:filename
```

---

## Error Responses

```json
{
  "error": "Error message here"
}
```

Common status codes:
- `400` - Bad request (validation error)
- `401` - Unauthorized (missing/invalid token)
- `403` - Forbidden (not your resource)
- `404` - Not found
- `500` - Server error
