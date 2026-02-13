# Late API - Connect Endpoint Documentation

## Overview
The Connect API provides OAuth-based authentication flows for linking social media accounts to the Late platform. Each platform follows a specific OAuth or credential-based flow, and additional endpoints allow selection of sub-entities such as Pages, Organizations, Boards, Locations, and Subreddits.

---

## OAuth Connection Flow

### Step 1: Initiate Connection
**GET** `/v1/connect/{platform}`

Redirects the user to the platform's OAuth authorization page. After the user grants permission, they are redirected back to Late with an authorization code.

**Path Parameters:**
- `platform` (string, required): The platform identifier. Supported values: `facebook`, `linkedin`, `pinterest`, `google-business`, `snapchat`, `bluesky`, `telegram`, `twitter`, `instagram`, `youtube`, `tiktok`, `reddit`

**Query Parameters:**
- `profileId` (string, required): The profile ID to associate the account with
- `callbackUrl` (string, optional): Custom callback URL after OAuth completion
- `headless` (boolean, optional): When `true`, returns JSON responses instead of redirects (see Headless Mode)

**Response (302):**
Redirects to the platform's OAuth consent screen.

### Step 2: OAuth Callback
**GET** `/v1/connect/{platform}/callback`

Handles the OAuth callback from the platform. Exchanges the authorization code for access tokens and creates the account connection.

**Query Parameters:**
- `code` (string): Authorization code from the platform
- `state` (string): State parameter for CSRF protection

**Response (200):**
```json
{
  "message": "Account connected successfully",
  "account": {
    "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
    "platform": "twitter",
    "platformUsername": "@exampleuser",
    "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
    "status": "active",
    "connectedAt": "2024-11-01T10:00:00Z"
  }
}
```

---

## Platform-Specific Flows

### Facebook
**GET** `/v1/connect/facebook`

Initiates Facebook OAuth flow. After authorization, the user must select a Facebook Page to connect.

**Required Scopes:** `pages_manage_posts`, `pages_read_engagement`, `pages_show_list`

**Query Parameters:**
- `profileId` (string, required)
- `callbackUrl` (string, optional)
- `headless` (boolean, optional)

### LinkedIn
**GET** `/v1/connect/linkedin`

Initiates LinkedIn OAuth 2.0 flow. Supports both personal profiles and organization pages.

**Required Scopes:** `w_member_social`, `r_liteprofile`, `r_organization_social`

**Query Parameters:**
- `profileId` (string, required)
- `callbackUrl` (string, optional)
- `headless` (boolean, optional)

### Pinterest
**GET** `/v1/connect/pinterest`

Initiates Pinterest OAuth flow. After authorization, the user selects a board for pinning.

**Required Scopes:** `boards:read`, `pins:read`, `pins:write`

**Query Parameters:**
- `profileId` (string, required)
- `callbackUrl` (string, optional)
- `headless` (boolean, optional)

### Google Business
**GET** `/v1/connect/google-business`

Initiates Google OAuth flow for Google Business Profile. After authorization, the user selects a business location.

**Required Scopes:** `https://www.googleapis.com/auth/business.manage`

**Query Parameters:**
- `profileId` (string, required)
- `callbackUrl` (string, optional)
- `headless` (boolean, optional)

### Snapchat
**GET** `/v1/connect/snapchat`

Initiates Snapchat OAuth flow for connecting a Snapchat account.

**Required Scopes:** `snapchat-marketing-api`

**Query Parameters:**
- `profileId` (string, required)
- `callbackUrl` (string, optional)
- `headless` (boolean, optional)

### Bluesky
**POST** `/v1/connect/bluesky`

Connects a Bluesky account using app password credentials (no OAuth redirect).

**Request Body:**
- `identifier` (string, required): Bluesky handle or DID (e.g., `user.bsky.social`)
- `appPassword` (string, required): App-specific password generated from Bluesky settings
- `profileId` (string, required): Profile ID to associate

**Response (200):**
```json
{
  "message": "Bluesky account connected successfully",
  "account": {
    "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
    "platform": "bluesky",
    "platformUsername": "user.bsky.social",
    "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
    "status": "active",
    "connectedAt": "2024-11-01T10:00:00Z"
  }
}
```

### Telegram
**POST** `/v1/connect/telegram`

Connects a Telegram bot using a bot token.

**Request Body:**
- `botToken` (string, required): Telegram Bot API token obtained from @BotFather
- `profileId` (string, required): Profile ID to associate

**Response (200):**
```json
{
  "message": "Telegram bot connected successfully",
  "account": {
    "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
    "platform": "telegram",
    "platformUsername": "MyBotName",
    "profileId": "6507a1b2c3d4e5f6a7b8c9d0",
    "status": "active",
    "connectedAt": "2024-11-01T10:00:00Z"
  }
}
```

---

## Account Management Endpoints

### List Facebook Pages
**GET** `/v1/connect/facebook/pages`

Returns a list of Facebook Pages the authenticated user has admin access to, for selection after OAuth.

**Query Parameters:**
- `connectionId` (string, required): Temporary connection ID from OAuth callback

**Response (200):**
```json
{
  "pages": [
    {
      "id": "123456789",
      "name": "My Business Page",
      "category": "Business",
      "picture": "https://graph.facebook.com/123456789/picture"
    }
  ]
}
```

### Select Facebook Page
**POST** `/v1/connect/facebook/pages`

Selects a specific Facebook Page to complete the connection.

**Request Body:**
- `connectionId` (string, required): Temporary connection ID
- `pageId` (string, required): The selected Facebook Page ID

**Response (200):**
```json
{
  "message": "Facebook Page connected successfully",
  "account": {
    "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
    "platform": "facebook",
    "platformUsername": "My Business Page",
    "pageId": "123456789"
  }
}
```

### List LinkedIn Organizations
**GET** `/v1/connect/linkedin/organizations`

Returns LinkedIn Organizations the authenticated user can manage.

**Query Parameters:**
- `connectionId` (string, required): Temporary connection ID from OAuth callback

**Response (200):**
```json
{
  "organizations": [
    {
      "id": "urn:li:organization:12345678",
      "name": "Acme Corporation",
      "vanityName": "acme-corp",
      "logoUrl": "https://media.licdn.com/..."
    }
  ]
}
```

### Select LinkedIn Organization
**POST** `/v1/connect/linkedin/organizations`

Selects a specific LinkedIn Organization to complete the connection.

**Request Body:**
- `connectionId` (string, required): Temporary connection ID
- `organizationUrn` (string, required): The LinkedIn Organization URN

**Response (200):**
```json
{
  "message": "LinkedIn Organization connected successfully",
  "account": {
    "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
    "platform": "linkedin",
    "platformUsername": "Acme Corporation",
    "organizationUrn": "urn:li:organization:12345678"
  }
}
```

### List Pinterest Boards
**GET** `/v1/connect/pinterest/boards`

Returns Pinterest Boards owned by the authenticated user.

**Query Parameters:**
- `connectionId` (string, required): Temporary connection ID from OAuth callback

**Response (200):**
```json
{
  "boards": [
    {
      "id": "board-123",
      "name": "Marketing Inspiration",
      "description": "Ideas for marketing campaigns",
      "pinCount": 42
    }
  ]
}
```

### Select Pinterest Board
**POST** `/v1/connect/pinterest/boards`

Selects a Pinterest Board to associate with the connection.

**Request Body:**
- `connectionId` (string, required): Temporary connection ID
- `boardId` (string, required): The selected Pinterest Board ID

**Response (200):**
```json
{
  "message": "Pinterest Board connected successfully",
  "account": {
    "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
    "platform": "pinterest",
    "platformUsername": "user",
    "boardId": "board-123"
  }
}
```

### List Google Business Locations
**GET** `/v1/connect/google-business/locations`

Returns Google Business locations the authenticated user can manage.

**Query Parameters:**
- `connectionId` (string, required): Temporary connection ID from OAuth callback

**Response (200):**
```json
{
  "locations": [
    {
      "name": "locations/12345678901234567890",
      "title": "My Coffee Shop",
      "storefrontAddress": {
        "addressLines": ["123 Main St"],
        "locality": "San Francisco",
        "regionCode": "US"
      }
    }
  ]
}
```

### Select Google Business Location
**POST** `/v1/connect/google-business/locations`

Selects a Google Business location to complete the connection.

**Request Body:**
- `connectionId` (string, required): Temporary connection ID
- `locationId` (string, required): The Google Business location name/ID

**Response (200):**
```json
{
  "message": "Google Business location connected successfully",
  "account": {
    "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
    "platform": "google-business",
    "platformUsername": "My Coffee Shop",
    "locationId": "locations/12345678901234567890"
  }
}
```

### List Reddit Subreddits
**GET** `/v1/connect/reddit/subreddits`

Returns subreddits the authenticated Reddit user moderates or can post to.

**Query Parameters:**
- `connectionId` (string, required): Temporary connection ID from OAuth callback

**Response (200):**
```json
{
  "subreddits": [
    {
      "name": "r/mybusiness",
      "displayName": "My Business",
      "subscribers": 15000,
      "isModerator": true
    }
  ]
}
```

### Select Reddit Subreddit
**POST** `/v1/connect/reddit/subreddits`

Selects a subreddit to associate with the Reddit connection.

**Request Body:**
- `connectionId` (string, required): Temporary connection ID
- `subredditName` (string, required): The subreddit name (e.g., `mybusiness`)

**Response (200):**
```json
{
  "message": "Reddit subreddit connected successfully",
  "account": {
    "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
    "platform": "reddit",
    "platformUsername": "u/exampleuser",
    "subredditName": "r/mybusiness"
  }
}
```

---

## Headless Mode

Headless mode is designed for server-to-server integrations where browser redirects are not possible. When `headless=true` is passed as a query parameter, the API returns JSON responses instead of HTTP redirects.

### Initiating Headless OAuth
**GET** `/v1/connect/{platform}?headless=true&profileId={profileId}`

**Response (200):**
```json
{
  "authorizationUrl": "https://platform.com/oauth/authorize?client_id=...&redirect_uri=...&state=...",
  "state": "abc123def456",
  "expiresIn": 600
}
```

### Completing Headless OAuth
**POST** `/v1/connect/{platform}/callback`

Instead of the platform redirecting the user, the client submits the authorization code directly.

**Request Body:**
- `code` (string, required): Authorization code from OAuth
- `state` (string, required): State parameter for validation

**Response (200):**
```json
{
  "message": "Account connected successfully",
  "account": {
    "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
    "platform": "twitter",
    "status": "active"
  },
  "requiresSelection": false
}
```

If `requiresSelection` is `true`, the client must call the appropriate sub-entity selection endpoint (e.g., Facebook Pages, LinkedIn Organizations).

---

## Error Responses

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters or missing required fields |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Insufficient permissions for the requested operation |
| **404** | Connection or platform not found |
| **409** | Account already connected to this profile |
| **422** | OAuth flow failed or authorization was denied |
| **429** | Rate limit exceeded |
| **500** | Internal server error |

---
