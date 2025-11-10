# Chirpy API Documentation

Base URL: `http://localhost:8080`

## Authentication

Most endpoints require authentication via JWT tokens passed in the `Authorization` header:
```
Authorization: Bearer <token>
```

## Endpoints

### Health Check

#### GET /api/healthz
Check API health status.

**Response**: `200 OK`
```
OK
```

---

### Users

#### POST /api/users
Create a new user account.

**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**Response**: `201 Created`
```json
{
  "id": "uuid",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "email": "user@example.com",
  "is_chirpy_red": false
}
```

#### PUT /api/users
Update user email and password. Requires authentication.

**Headers**: `Authorization: Bearer <token>`

**Request Body**:
```json
{
  "email": "newemail@example.com",
  "password": "newpassword"
}
```

**Response**: `200 OK`
```json
{
  "id": "uuid",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "email": "newemail@example.com",
  "is_chirpy_red": false
}
```

---

### Authentication

#### POST /api/login
Login and receive access and refresh tokens.

**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**Response**: `200 OK`
```json
{
  "id": "uuid",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "email": "user@example.com",
  "is_chirpy_red": false,
  "token": "jwt-access-token",
  "refresh_token": "refresh-token"
}
```

#### POST /api/refresh
Get a new access token using a refresh token.

**Headers**: `Authorization: Bearer <refresh_token>`

**Response**: `200 OK`
```json
{
  "token": "new-jwt-access-token"
}
```

#### POST /api/revoke
Revoke a refresh token.

**Headers**: `Authorization: Bearer <refresh_token>`

**Response**: `204 No Content`

---

### Chirps

#### POST /api/chirps
Create a new chirp. Requires authentication.

**Headers**: `Authorization: Bearer <token>`

**Request Body**:
```json
{
  "body": "This is my chirp!"
}
```

**Response**: `201 Created`
```json
{
  "id": "uuid",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "body": "This is my chirp!",
  "user_id": "uuid"
}
```

**Notes**:
- Maximum body length: 140 characters
- Profane words are automatically replaced with `****`

#### GET /api/chirps
Get all chirps with optional filtering and sorting.

**Query Parameters**:
- `author_id` (optional): Filter by author UUID
- `sort` (optional): Sort order (`asc` or `desc`, default: `asc`)

**Examples**:
```
GET /api/chirps
GET /api/chirps?author_id=uuid
GET /api/chirps?sort=desc
GET /api/chirps?author_id=uuid&sort=desc
```

**Response**: `200 OK`
```json
[
  {
    "id": "uuid",
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z",
    "body": "Chirp content",
    "user_id": "uuid"
  }
]
```

#### GET /api/chirps/{chirpID}
Get a specific chirp by ID.

**Response**: `200 OK`
```json
{
  "id": "uuid",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "body": "Chirp content",
  "user_id": "uuid"
}
```

#### DELETE /api/chirps/{chirpID}
Delete a chirp. Requires authentication. Users can only delete their own chirps.

**Headers**: `Authorization: Bearer <token>`

**Response**: `204 No Content`

---

### Webhooks

#### POST /api/polka/webhooks
Webhook endpoint for Polka payment processor to upgrade users.

**Headers**: `Authorization: ApiKey <polka_key>`

**Request Body**:
```json
{
  "event": "user.upgraded",
  "data": {
    "user_id": "uuid"
  }
}
```

**Response**: `204 No Content`

**Notes**:
- Only processes `user.upgraded` events
- Requires valid Polka API key

---

## Error Responses

### 400 Bad Request
```json
{
  "error": "Something went wrong"
}
```

### 401 Unauthorized
No body, status code only.

### 403 Forbidden
No body, status code only.

### 404 Not Found
No body, status code only.

### 500 Internal Server Error
```json
{
  "error": "Something went wrong"
}
```

---

## Admin Endpoints (Dev Only)

#### GET /admin/metrics
View server metrics.

**Response**: `200 OK` (HTML)

#### POST /admin/reset
Reset database and metrics. Only available in dev environment.

**Response**: `200 OK`
