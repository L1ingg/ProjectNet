# ProjectNet

Spring Boot backend with JWT authentication and Discord account linking module.

## Features

- User registration with email, password, and nickname
- User login with email and password
- JWT-based authentication
  - Access token: 15 minutes lifetime
  - Refresh token: 30 days lifetime
- Token refresh endpoint
- Discord bot integration for account linking via one-time code

## Authentication

The system uses JWT tokens:

- **Access token**: valid for 15 minutes
- **Refresh token**: valid for 30 days

When the access token expires, it can be refreshed using the `/auth/refresh` endpoint.

## API Endpoints

### `POST /auth/register`

Registers a new user.

#### Request body

```json
{
  "email": "user@example.com",
  "password": "password123",
  "nickname": "nickname"
}
