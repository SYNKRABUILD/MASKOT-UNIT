# API Documentation

## Base URL

```
Development: http://localhost:3001/api
Production: https://api.maskotunit.com/api
```

## Authentication

All protected endpoints require a JWT token in the `Authorization` header:

```
Authorization: Bearer <token>
```

## Response Format

All responses follow a standard format:

```json
{
  "success": true,
  "data": { /* response data */ },
  "message": "Success message",
  "timestamp": "2026-08-30T20:00:00Z"
}
```

Error responses:

```json
{
  "success": false,
  "error": "Error code",
  "message": "Error description",
  "timestamp": "2026-08-30T20:00:00Z"
}
```

## Authentication Endpoints

### Register User

```
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword",
  "firstName": "John",
  "lastName": "Doe",
  "role": "creator" | "admin" | "brand_ambassador"
}

Response: 201 Created
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "role": "creator",
    "createdAt": "2026-08-30T20:00:00Z"
  }
}
```

### Login

```
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword"
}

Response: 200 OK
{
  "success": true,
  "data": {
    "token": "jwt_token",
    "refreshToken": "refresh_token",
    "user": {
      "id": "uuid",
      "email": "user@example.com",
      "role": "creator"
    }
  }
}
```

### Get Current User

```
GET /auth/me
Authorization: Bearer <token>

Response: 200 OK
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "role": "creator",
    "profile": { /* profile data */ }
  }
}
```

### Refresh Token

```
POST /auth/refresh
Content-Type: application/json

{
  "refreshToken": "refresh_token"
}

Response: 200 OK
{
  "success": true,
  "data": {
    "token": "new_jwt_token"
  }
}
```

## Creator Endpoints

### Get All Creators

```
GET /creators?page=1&limit=20&verified=true
Authorization: Bearer <token> (optional)

Response: 200 OK
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "userId": "uuid",
      "displayName": "John Doe",
      "bio": "Content creator",
      "profilePictureUrl": "https://...",
      "verified": true,
      "rating": 4.5,
      "followerCount": 10000
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150
  }
}
```

### Get Creator by ID

```
GET /creators/:id
Authorization: Bearer <token> (optional)

Response: 200 OK
{
  "success": true,
  "data": {
    "id": "uuid",
    "userId": "uuid",
    "displayName": "John Doe",
    "bio": "Content creator",
    "profilePictureUrl": "https://...",
    "coverImageUrl": "https://...",
    "location": "Dagoretti",
    "website": "https://...",
    "verified": true,
    "rating": 4.5,
    "totalReviews": 50,
    "socialMediaAccounts": [ /* accounts */ ],
    "portfolio": [ /* portfolio items */ ],
    "createdAt": "2026-08-30T20:00:00Z"
  }
}
```

### Create/Update Creator Profile

```
POST /creators
PUT /creators/:id
Content-Type: application/json
Authorization: Bearer <token>

{
  "displayName": "John Doe",
  "bio": "Content creator from Dagoretti",
  "location": "Dagoretti",
  "website": "https://example.com",
  "profilePictureUrl": "https://...",
  "coverImageUrl": "https://..."
}

Response: 201/200 OK
{
  "success": true,
  "data": { /* created/updated creator */ }
}
```

## Social Media Endpoints

### Link Social Media Account

```
POST /social/accounts
Content-Type: application/json
Authorization: Bearer <token>

{
  "platform": "instagram" | "tiktok" | "facebook" | "youtube",
  "accountId": "123456789",
  "username": "@username",
  "accessToken": "platform_access_token"
}

Response: 201 Created
{
  "success": true,
  "data": {
    "id": "uuid",
    "platform": "instagram",
    "username": "@username",
    "followersCount": 5000,
    "connectedAt": "2026-08-30T20:00:00Z"
  }
}
```

### Get Social Media Accounts

```
GET /social/accounts
Authorization: Bearer <token>

Response: 200 OK
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "platform": "instagram",
      "username": "@username",
      "followersCount": 5000,
      "isActive": true
    }
  ]
}
```

### Schedule Social Media Post

```
POST /social/schedule
Content-Type: application/json
Authorization: Bearer <token>

{
  "platforms": ["instagram", "tiktok"],
  "caption": "Check out my new content!",
  "imageUrl": "https://...",
  "scheduleTime": "2026-08-31T14:00:00Z"
}

Response: 201 Created
{
  "success": true,
  "data": {
    "id": "uuid",
    "status": "scheduled",
    "scheduleTime": "2026-08-31T14:00:00Z"
  }
}
```

## Music Endpoints

### Upload Music

```
POST /music/upload
Content-Type: multipart/form-data
Authorization: Bearer <token>

{
  "title": "Song Title",
  "description": "Song description",
  "artistName": "Artist Name",
  "genre": "Hip Hop",
  "file": <audio file>,
  "coverImage": <image file>
}

Response: 201 Created
{
  "success": true,
  "data": {
    "id": "uuid",
    "title": "Song Title",
    "status": "submitted",
    "fileUrl": "https://...",
    "createdAt": "2026-08-30T20:00:00Z"
  }
}
```

### Get Music Details

```
GET /music/:id
Authorization: Bearer <token>

Response: 200 OK
{
  "success": true,
  "data": {
    "id": "uuid",
    "title": "Song Title",
    "artistName": "Artist Name",
    "genre": "Hip Hop",
    "status": "published",
    "playsCount": 5000,
    "downloadsCount": 200,
    "releaseDate": "2026-08-30",
    "analytics": { /* analytics data */ }
  }
}
```

### Distribute Music

```
POST /music/:id/distribute
Content-Type: application/json
Authorization: Bearer <token>

{
  "platforms": ["spotify", "apple_music", "youtube"],
  "releaseDate": "2026-09-15"
}

Response: 200 OK
{
  "success": true,
  "data": {
    "id": "uuid",
    "status": "distributing",
    "platforms": ["spotify", "apple_music", "youtube"]
  }
}
```

## Payment Endpoints

### Initiate Payment

```
POST /payments/initiate
Content-Type: application/json
Authorization: Bearer <token>

{
  "amount": 5000,
  "currency": "KES",
  "paymentMethod": "mpesa",
  "phoneNumber": "+254720357606",
  "description": "Service payment"
}

Response: 200 OK
{
  "success": true,
  "data": {
    "transactionId": "uuid",
    "status": "pending",
    "checkoutUrl": "https://...",
    "expiresAt": "2026-08-30T21:00:00Z"
  }
}
```

### Get Transaction

```
GET /payments/:transactionId
Authorization: Bearer <token>

Response: 200 OK
{
  "success": true,
  "data": {
    "id": "uuid",
    "amount": 5000,
    "currency": "KES",
    "status": "completed",
    "mpesaRef": "LIL12345678",
    "createdAt": "2026-08-30T20:00:00Z"
  }
}
```

### Get Transactions History

```
GET /transactions?page=1&limit=20&status=completed
Authorization: Bearer <token>

Response: 200 OK
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "amount": 5000,
      "status": "completed",
      "createdAt": "2026-08-30T20:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 45
  }
}
```

## Analytics Endpoints

### Get Dashboard Analytics

```
GET /analytics/dashboard?startDate=2026-08-01&endDate=2026-08-31
Authorization: Bearer <token>

Response: 200 OK
{
  "success": true,
  "data": {
    "totalViews": 50000,
    "totalEngagement": 5000,
    "totalRevenue": 25000,
    "topContent": [ /* top performing content */ ],
    "platforms": {
      "instagram": { views: 20000, engagement: 2000 },
      "tiktok": { views: 30000, engagement: 3000 }
    }
  }
}
```

### Get Creator Analytics

```
GET /analytics/creators/:id?period=monthly
Authorization: Bearer <token>

Response: 200 OK
{
  "success": true,
  "data": {
    "creatorId": "uuid",
    "period": "monthly",
    "views": 10000,
    "engagement": 1000,
    "followers": 5000,
    "revenue": 2500,
    "trends": [ /* trend data */ ]
  }
}
```

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| AUTH_001 | 401 | Unauthorized - Invalid token |
| AUTH_002 | 401 | Unauthorized - Token expired |
| AUTH_003 | 403 | Forbidden - Insufficient permissions |
| VALID_001 | 400 | Validation error |
| NOT_FOUND | 404 | Resource not found |
| CONFLICT | 409 | Resource already exists |
| RATE_LIMIT | 429 | Too many requests |
| SERVER_ERROR | 500 | Internal server error |

## Rate Limiting

- Public endpoints: 100 requests per hour per IP
- Authenticated endpoints: 1000 requests per hour per user
- Payment endpoints: 50 requests per hour per user

Headers:
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1693382400
```

## Pagination

Use `page` and `limit` query parameters:

```
GET /creators?page=1&limit=20

Response includes:
{
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "pages": 8
  }
}
```

## Webhooks

### Payment Webhook

```
POST /webhooks/payments
Content-Type: application/json

{
  "event": "payment.completed",
  "data": {
    "transactionId": "uuid",
    "amount": 5000,
    "mpesaRef": "LIL12345678"
  }
}
```

### Music Distribution Webhook

```
POST /webhooks/music
Content-Type: application/json

{
  "event": "music.distributed",
  "data": {
    "musicId": "uuid",
    "platform": "spotify",
    "status": "live"
  }
}
```

---

**Last Updated:** August 30, 2026
**API Version:** 1.0.0
