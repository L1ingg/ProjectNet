# ProjectNet

A Spring Boot backend service providing JWT-based authentication and Discord account linking via one-time verification codes.

---

## Features

- User registration (email, password, nickname)
- User authentication via email and password
- JWT access token (15 minutes lifetime)
- JWT refresh token (30 days lifetime)
- Access token renewal using refresh token
- Discord account linking via one-time verification code

---

## Authentication

The system is based on JWT (JSON Web Tokens):

### Access Token
- Lifetime: 15 minutes
- Used for accessing protected endpoints

### Refresh Token
- Lifetime: 30 days
- Used to obtain a new access token

### Refresh Endpoint

```http
POST /auth/refresh
````

---

## API

### Register User

```http
POST /auth/register
```

Creates a new user account.

#### Request Body

```json
{
  "email": "user@example.com",
  "password": "password123",
  "nickname": "nickname"
}
```

---

## Discord Account Linking

This module allows linking a Discord account to an existing application user using a one-time verification code.

The mechanism ensures that only the legitimate owner of both accounts can complete the linking process.

---

## Entities

### Application User

A user of the system:

* Created via registration
* Identified by `userId`
* Authenticated via email and password

---

### Discord User

Represents a Discord account:

* `discordUserId` (primary identifier)
* Username (optional)
* Discriminator (legacy support)

---

### Linking Code

A temporary one-time code used for verification.

Fields:

* `code` — string or numeric value
* `userId` — owner in the application
* `expiresAt` — expiration timestamp
* `used` — boolean flag

---

## Discord Linking Flow

### Step 1: Request linking code

The authenticated user requests a linking code:

```http
GET /auth/linking?discord=true
Authorization: Bearer <access_token>
```

The response contains a one-time code used to verify ownership of the account.


- или :contentReference[oaicite:2]{index=2}
```
