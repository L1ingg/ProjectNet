# ProjectNet

Spring Boot backend with JWT authentication and Discord account linking via one-time verification codes.

---

## Features

- User registration with email, password, and nickname
- Login with email and password
- JWT access token (15 minutes lifetime)
- JWT refresh token (30 days lifetime)
- Access token renewal via refresh token
- Discord account linking via one-time verification code

---

## Authentication

The system uses JWT-based authentication:

- **Access Token** — valid for 15 minutes
- **Refresh Token** — valid for 30 days

After the access token expires, a new one can be obtained using:

```http
POST /auth/refresh
```

---

## API

### Register User

`POST /auth/register`

Registers a new user.

#### Request Body

```json
{
  "email": "user@example.com",
  "password": "password123",
  "nickname": "nickname"
}
```

---

## Discord Account Linking (Extended)

This module is responsible for binding a Discord account to an existing application user account using a one-time verification code.

The system is designed to prevent unauthorized linking and ensure that only the owner of both accounts can perform the binding.

---

## Entities Involved

### Application User

A registered user in the backend system:

* authenticated via email + password
* identified internally by `userId`

### Discord User

A user identified by:

* Discord `userId`
* optionally username + discriminator (legacy)

### Linking Code

A temporary, single-use code used as proof of ownership.

Fields:

* `code` (string / numeric)
* `userId` (application user)
* `expiresAt` (timestamp)
* `used` (boolean)

---

## Linking Flow (Step-by-Step)

### 1. Request linking code (backend → user)

The user initiates linking from the application:

```http
GET /auth/linking?DISCORD=true
Authorization: Bearer <access_token>
```

