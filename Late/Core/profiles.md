# Profiles - API Reference

## Overview
The Profiles API enables users to create and manage profiles for organizing connected social media accounts within the Late API ecosystem.

## Core Endpoints

### List Profiles
**GET** `/v1/profiles`

Returns profiles accessible to the authenticated user, sorted by creation date (oldest first). An optional query parameter `includeOverLimit=true` retrieves profiles exceeding plan limits.

**Query Parameters:**
- `includeOverLimit` (boolean, optional): Includes over-limit profiles marked with `isOverLimit: true`

**Response (200):**
```json
{
  "profiles": [
    {
      "_id": "64f0...",
      "name": "Personal Brand",
      "color": "#ffeda0",
      "isDefault": true
    }
  ]
}
```

### Create Profile
**POST** `/v1/profiles`

Creates a new profile with customizable name, description, and color.

**Request Body:**
- `name` (string): Profile identifier
- `description` (string, optional): Additional details
- `color` (string, optional): Hex color code

**Response (201):**
```json
{
  "message": "Profile created successfully",
  "profile": {
    "_id": "64f0a1b2c3d4e5f6a7b8c9d0",
    "userId": "6507a1b2c3d4e5f6a7b8c9d0",
    "name": "Marketing Team",
    "description": "Profile for marketing campaigns",
    "color": "#4CAF50",
    "isDefault": false,
    "createdAt": "2024-11-01T10:00:00Z"
  }
}
```

### Get Profile by ID
**GET** `/v1/profiles/{profileId}`

Retrieves a specific profile's details.

### Delete Profile
**DELETE** `/v1/profiles/{profileId}`

Removes a profile. Profiles must have no connected accounts prior to deletion.

### Update Profile
**PUT** `/v1/profiles/{profileId}`

Modifies profile properties including name, description, color, and default status.

**Request Body:**
- `name` (string, optional)
- `description` (string, optional)
- `color` (string, optional)
- `isDefault` (boolean, optional)

## Error Responses
- **401:** Unauthorized access
- **403:** Insufficient permissions
- **404:** Profile not found
- **400:** Invalid request parameters

---
