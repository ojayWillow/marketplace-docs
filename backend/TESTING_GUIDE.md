# Testing Guide

**Framework**: pytest + pytest-cov

---

## Setup

```bash
# Install test dependencies
pip install pytest pytest-cov

# Or use requirements-test.txt
pip install -r requirements-test.txt
```

---

## Running Tests

### Run All Tests
```bash
pytest
```

### Run with Verbose Output
```bash
pytest -v
```

### Run Specific Test File
```bash
pytest tests/test_auth.py -v
```

### Run Specific Test
```bash
pytest tests/test_auth.py::TestAuthEndpoints::test_register_success -v
```

### Run with Coverage
```bash
pytest --cov=app --cov-report=html
```

Coverage report will be in `htmlcov/index.html`

---

## Test Structure

```
tests/
├── conftest.py           # Shared fixtures
├── test_auth.py          # Authentication tests
├── test_listings.py      # Listings CRUD tests
├── test_tasks.py         # Tasks workflow tests
├── test_offerings.py     # Offerings tests
├── test_reviews.py       # Reviews tests
├── test_api_endpoints.py # Health checks
└── test_users.py         # Public profiles
```

---

## Key Fixtures (conftest.py)

### `client`
Flask test client with in-memory SQLite database.

```python
def test_something(client):
    response = client.get('/health')
    assert response.status_code == 200
```

### `auth_token`
JWT token for authenticated requests.

```python
def test_protected(client, auth_token):
    response = client.get(
        '/api/auth/profile',
        headers={'Authorization': f'Bearer {auth_token}'}
    )
    assert response.status_code == 200
```

### `test_user`
Pre-created user for testing.

```python
def test_with_user(client, test_user):
    # test_user is already in database
    assert test_user['email'] == 'test@example.com'
```

---

## Test Examples

### Testing Registration
```python
def test_register_success(client):
    response = client.post('/api/auth/register', json={
        'username': 'newuser',
        'email': 'new@example.com',
        'password': 'password123'
    })
    assert response.status_code == 201
    assert 'id' in response.json
```

### Testing Protected Routes
```python
def test_create_listing(client, auth_token):
    response = client.post(
        '/api/listings',
        json={
            'title': 'Test Item',
            'price': 100,
            'category': 'electronics'
        },
        headers={'Authorization': f'Bearer {auth_token}'}
    )
    assert response.status_code == 201
```

### Testing Authorization
```python
def test_cannot_edit_others_listing(client, auth_token, other_user_listing):
    response = client.put(
        f'/api/listings/{other_user_listing["id"]}',
        json={'title': 'Hacked!'},
        headers={'Authorization': f'Bearer {auth_token}'}
    )
    assert response.status_code == 403
```

---

## Coverage Goals

| Area | Target |
|------|--------|
| Auth endpoints | 90%+ |
| CRUD operations | 90%+ |
| Error cases | 80%+ |
| Authorization | 90%+ |

---

## Writing New Tests

1. Create test file in `tests/` directory
2. Import fixtures from conftest.py
3. Name tests with `test_` prefix
4. Use descriptive names: `test_create_listing_without_auth_fails`
5. Test both success and error cases
6. Test authorization (can't modify others' resources)

---

## Related Documentation

- [Project Status](PROJECT_STATUS.md)
- [API Endpoints](API_ENDPOINTS.md)
