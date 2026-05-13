
# ProjectNet

Backend service written in Spring Boot providing JWT-based authentication and Discord account linking via one-time verification codes and a Discord bot.

---

## Features

- User registration (email, password, nickname)
- User login via email and password
- JWT authentication
  - Access token: 15 minutes lifetime
  - Refresh token: 30 days lifetime
- Token refresh endpoint
- Discord account linking via one-time code + Discord bot command

---

## Authentication System

Authentication is based on JWT tokens.

### Tokens

- **Access Token**
  - Valid for 15 minutes
  - Used for accessing protected endpoints

- **Refresh Token**
  - Valid for 30 days
  - Used to generate new access tokens

---

## API Endpoints

### Register

```http
POST /auth/register
```

Creates a new user account.

#### Body

```json
{
  "email": "user@example.com",
  "password": "password123",
  "username": "username"
}
```

---

### Login

```http
POST /auth/login
```

Authenticates user and returns JWT tokens.

#### Body

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

---

### Refresh Token

```http
POST /auth/refresh
```

Generates a new access token using a valid refresh token.

---

## Discord Integration

The project includes a Discord bot that allows linking a Discord account to an application user.

### Overview

Linking is performed using a one-time verification code:

1. User requests a linking code from backend
2. Backend generates a temporary one-time code
3. User sends this code to a Discord bot using `/link`
4. Bot validates the code
5. Accounts are linked

---

## Request Linking Code

```http
GET /auth/linking?DISCORD=true
Authorization: Bearer <access_token>
```

### Response

Returns a one-time verification code tied to the authenticated user.

---

## Discord Bot Command

```
/link code:<verification_code>
```

Example:

```
/link code:123456
```

---

## Linking Logic

### Linking Code

* Single-use
* Bound to a specific userId
* Has expiration time
* Invalid after successful use

### Flow

1. Backend generates linking code for authenticated user
2. User receives code
3. User sends code to Discord bot
4. Bot validates:

   * code exists
   * code not expired
   * code not used
5. If valid → Discord account is linked to application user
6. Code is marked as used

---

## Result

After successful linking:

* Application user is associated with a Discord user ID
* Linking code becomes invalid
* Discord bot confirms successful binding


