# MySportsPlaylist API Documentation

This document provides comprehensive documentation for the MySportsPlaylist REST API endpoints. The API allows clients to manage sports matches, user playlists, and authentication.

## Base URL

All API endpoints are relative to the base URL:

- **HTTPS**: `https://localhost:7161/api`
- **HTTP**: `http://localhost:5163/api`

## Authentication

The API uses JWT (JSON Web Token) authentication. Include the JWT token in the Authorization header for protected endpoints:

```
Authorization: Bearer <your_jwt_token>
```

### How to obtain a token

Use the login or register endpoints to obtain a JWT token.

## Endpoints

### Authentication

#### Register

Creates a new user account and returns an authentication token.

- **URL**: `/auth/register`
- **Method**: `POST`
- **Auth required**: No
- **Request Body**:

  ```json
  {
    "username": "string",
    "email": "string",
    "password": "string"
  }
  ```

- **Response**:

  ```json
  {
    "userId": "integer",
    "username": "string",
    "email": "string",
    "token": "string"
  }
  ```

- **Status Codes**:
  - `200 OK`: Registration successful
  - `400 Bad Request`: Invalid input or username/email already exists

#### Login

Authenticates a user and returns a JWT token.

- **URL**: `/auth/login`
- **Method**: `POST`
- **Auth required**: No
- **Request Body**:

  ```json
  {
    "username": "string",
    "password": "string"
  }
  ```

- **Response**:

  ```json
  {
    "userId": "integer",
    "username": "string",
    "email": "string",
    "token": "string"
  }
  ```

- **Status Codes**:
  - `200 OK`: Login successful
  - `400 Bad Request`: Invalid credentials

### Matches

#### Get All Matches

Returns a list of all available matches.

- **URL**: `/matches`
- **Method**: `GET`
- **Auth required**: No
- **Response**: Array of match objects

  ```json
  [
    {
      "id": "integer",
      "title": "string",
      "competition": "string",
      "description": "string",
      "thumbnailUrl": "string",
      "matchDate": "datetime",
      "status": "integer" // 0: Upcoming, 1: Live, 2: Replay
    }
  ]
  ```

- **Status Codes**:
  - `200 OK`: Successful operation

#### Get Match by ID

Returns a specific match by its ID.

- **URL**: `/matches/{id}`
- **Method**: `GET`
- **Auth required**: No
- **URL Parameters**: `id=[integer]` match ID
- **Response**: A match object

  ```json
  {
    "id": "integer",
    "title": "string",
    "competition": "string",
    "description": "string",
    "thumbnailUrl": "string",
    "matchDate": "datetime",
    "status": "integer" // 0: Upcoming, 1: Live, 2: Replay
  }
  ```

- **Status Codes**:
  - `200 OK`: Successful operation
  - `404 Not Found`: Match not found

#### Get Live Matches

Returns all matches currently live.

- **URL**: `/matches/live`
- **Method**: `GET`
- **Auth required**: No
- **Response**: Array of match objects with status = 1 (Live)
- **Status Codes**:
  - `200 OK`: Successful operation

#### Get Replay Matches

Returns all matches available for replay.

- **URL**: `/matches/replay`
- **Method**: `GET`
- **Auth required**: No
- **Response**: Array of match objects with status = 2 (Replay)
- **Status Codes**:
  - `200 OK`: Successful operation

#### Search Matches

Search for matches by query string.

- **URL**: `/matches/search`
- **Method**: `GET`
- **Auth required**: No
- **Query Parameters**: `query=[string]` search term
- **Response**: Array of match objects matching the search criteria
- **Status Codes**:
  - `200 OK`: Successful operation

#### Create Match (Admin Only)

Creates a new match.

- **URL**: `/matches`
- **Method**: `POST`
- **Auth required**: Yes (Admin role)
- **Request Body**: Match object without ID

  ```json
  {
    "title": "string",
    "competition": "string",
    "description": "string",
    "thumbnailUrl": "string",
    "matchDate": "datetime",
    "status": "integer"
  }
  ```

- **Response**: Created match object with ID
- **Status Codes**:
  - `201 Created`: Match created successfully
  - `400 Bad Request`: Invalid input
  - `401 Unauthorized`: Authentication missing
  - `403 Forbidden`: Insufficient permissions (non-admin)

#### Update Match (Admin Only)

Updates an existing match.

- **URL**: `/matches/{id}`
- **Method**: `PUT`
- **Auth required**: Yes (Admin role)
- **URL Parameters**: `id=[integer]` match ID
- **Request Body**: Complete match object with ID
- **Status Codes**:
  - `204 No Content`: Match updated successfully
  - `400 Bad Request`: Invalid input or ID mismatch
  - `401 Unauthorized`: Authentication missing
  - `403 Forbidden`: Insufficient permissions (non-admin)
  - `404 Not Found`: Match not found

#### Delete Match (Admin Only)

Deletes a match.

- **URL**: `/matches/{id}`
- **Method**: `DELETE`
- **Auth required**: Yes (Admin role)
- **URL Parameters**: `id=[integer]` match ID
- **Status Codes**:
  - `204 No Content`: Match deleted successfully
  - `401 Unauthorized`: Authentication missing
  - `403 Forbidden`: Insufficient permissions (non-admin)
  - `404 Not Found`: Match not found

### Playlists

#### Get User Playlist

Returns all matches in the current user's playlist.

- **URL**: `/playlists`
- **Method**: `GET`
- **Auth required**: Yes
- **Response**: Array of match objects in the user's playlist
- **Status Codes**:
  - `200 OK`: Successful operation
  - `401 Unauthorized`: Authentication missing

#### Add Match to Playlist

Adds a match to the current user's playlist.

- **URL**: `/playlists/{matchId}`
- **Method**: `POST`
- **Auth required**: Yes
- **URL Parameters**: `matchId=[integer]` match ID
- **Response**:

  ```json
  {
    "message": "Match added to playlist"
  }
  ```

- **Status Codes**:
  - `200 OK`: Match added successfully
  - `400 Bad Request`: Match already in playlist
  - `401 Unauthorized`: Authentication missing
  - `404 Not Found`: Match not found

#### Remove Match from Playlist

Removes a match from the current user's playlist.

- **URL**: `/playlists/{matchId}`
- **Method**: `DELETE`
- **Auth required**: Yes
- **URL Parameters**: `matchId=[integer]` match ID
- **Response**:

  ```json
  {
    "message": "Match removed from playlist"
  }
  ```

- **Status Codes**:
  - `200 OK`: Match removed successfully
  - `401 Unauthorized`: Authentication missing
  - `404 Not Found`: Match not found in playlist

#### Check if Match is in Playlist

Checks if a match is in the current user's playlist.

- **URL**: `/playlists/contains/{matchId}`
- **Method**: `GET`
- **Auth required**: Yes
- **URL Parameters**: `matchId=[integer]` match ID
- **Response**: Boolean value

  ```json
  true
  ```

  or

  ```json
  false
  ```
  
- **Status Codes**:
  - `200 OK`: Successful operation
  - `401 Unauthorized`: Authentication missing

## Real-time Notifications

The API supports real-time notifications for playlist changes and match status updates using SignalR.

### SignalR Hub

- **Hub URL**: `/hubs/notifications`
- **Authentication**: Use the same JWT token as for REST API endpoints
- **Events**:
  - `ReceivePlaylistNotification`: Notifies when a match is added to or removed from a playlist
  - `ReceiveMatchStatusUpdate`: Notifies when a match status changes (e.g., from Upcoming to Live)

## Error Handling

The API returns appropriate HTTP status codes along with error messages in the response body:

```json
{
  "message": "Error description"
}
```

## Data Models

### Match

```json
{
  "id": "integer",
  "title": "string",
  "competition": "string",
  "description": "string",
  "thumbnailUrl": "string",
  "matchDate": "datetime",
  "status": "integer" // 0: Upcoming, 1: Live, 2: Replay
}
```

### Playlist Item

```json
{
  "userId": "integer",
  "matchId": "integer",
  "dateAdded": "datetime"
}
```

### User

```json
{
  "id": "integer",
  "username": "string",
  "email": "string",
  "passwordHash": "string" // Not returned in responses
}
```

## Rate Limiting

The API currently does not implement rate limiting but may do so in future versions.

## CORS

The API is configured to allow cross-origin requests from `http://localhost:4200` (Angular frontend).

---
