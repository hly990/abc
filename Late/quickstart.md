# Late API Quickstart Guide

## Overview
This guide provides a comprehensive walkthrough for scheduling your first social media post using the Late API. Users will learn to create profiles, connect social accounts, and schedule posts across 13+ platforms.

## Prerequisites
- An active API key from Late

## Step-by-Step Process

### Step 1: Create a Profile
Profiles organize multiple social media accounts. The API endpoint is:

```
POST https://getlate.dev/api/v1/profiles
```

**Required Headers:**
- `Authorization: Bearer YOUR_API_KEY`
- `Content-Type: application/json`

**Request Body:**
```json
{
  "name": "My First Profile",
  "description": "Testing the Late API"
}
```

**Response includes:** Profile ID (save this for next steps)

### Step 2: Connect a Social Account
Use OAuth to link social media accounts. The endpoint pattern is:

```
GET https://getlate.dev/api/v1/connect/{platform}?profileId={profile_id}
```

**Supported Platforms:** Twitter/X, Instagram, Facebook, LinkedIn, TikTok, YouTube, Pinterest, Reddit, Bluesky, Threads, Google Business, Telegram, Snapchat

The response returns an authentication URL for user authorization.

### Step 3: Retrieve Connected Accounts
Fetch account details:

```
GET https://getlate.dev/api/v1/accounts
```

Response returns account IDs needed for posting.

### Step 4: Schedule Your First Post
Create and schedule a post:

```
POST https://getlate.dev/api/v1/posts
```

**Key Parameters:**
- `content`: Post text
- `scheduledFor`: ISO timestamp
- `timezone`: User's timezone
- `platforms`: Array with platform and account ID

**Optional Features:**
- `publishNow: true` – Posts immediately
- Omit scheduling parameters to save as draft
- Add multiple platforms for cross-posting

## Advanced Features

**Multi-Platform Posting:** Include multiple entries in the platforms array to publish simultaneously across different networks.

**Immediate Publishing:** Set `publishNow: true` to bypass scheduling.

**Draft Creation:** Submit without scheduling parameters to save work-in-progress content.

## Next Steps
- Explore platform-specific guides
- Upload media files to posts
- Configure posting queues
- Access analytics dashboards
- Manage team collaboration

## Support
Contact: miki@getlate.dev
