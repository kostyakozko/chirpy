# Chirpy

A Twitter-like social media API built with Go and PostgreSQL.

## Features

- User authentication with JWT tokens
- Refresh token management
- Create, read, and delete chirps (short messages)
- Premium user upgrades via Polka webhooks
- Profanity filtering
- Query filtering and sorting

## Tech Stack

- **Language**: Go
- **Database**: PostgreSQL
- **Authentication**: JWT + Refresh Tokens
- **Password Hashing**: Argon2id
- **SQL Generation**: sqlc
- **Database Migrations**: goose

## Prerequisites

- Go 1.21+
- PostgreSQL
- sqlc
- goose

## Setup

1. Clone the repository
2. Create a `.env` file:
```env
DB_URL="postgres://username:password@localhost:5432/chirpy?sslmode=disable"
PLATFORM="dev"
JWT_SECRET="your-secret-key"
POLKA_KEY="your-polka-api-key"
```

3. Run database migrations:
```bash
goose -dir sql/schema postgres "your-db-url" up
```

4. Generate database code:
```bash
sqlc generate
```

5. Run the server:
```bash
go run .
```

The server will start on `http://localhost:8080`

## Project Structure

```
chirpy/
├── internal/
│   ├── auth/          # Authentication utilities
│   └── database/      # Generated database code
├── sql/
│   ├── queries/       # SQL queries for sqlc
│   └── schema/        # Database migrations
├── main.go            # Application entry point
└── .env               # Environment variables
```

## API Documentation

See [API.md](API.md) for detailed API documentation.

## Development

### Reset Database (Dev Only)
```bash
POST /admin/reset
```

### View Metrics
```bash
GET /admin/metrics
```
