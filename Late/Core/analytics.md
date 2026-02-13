# Analytics - API Reference

## Overview
The Analytics API provides access to post performance metrics, follower statistics, and platform-specific analytics data. Use these endpoints to track engagement, growth, and content performance across connected social media accounts.

---

## Endpoints

### Get Post Analytics
**GET** `/v1/analytics`

Returns performance analytics for published posts across all connected accounts.

**Query Parameters:**
- `profileId` (string, optional): Filter by profile ID
- `accountId` (string, optional): Filter by specific account ID
- `platform` (string, optional): Filter by platform (e.g., `twitter`, `facebook`, `instagram`, `linkedin`)
- `postId` (string, optional): Get analytics for a specific post
- `startDate` (string, optional): Start date in ISO 8601 format (e.g., `2024-10-01`)
- `endDate` (string, optional): End date in ISO 8601 format (e.g., `2024-11-01`)
- `metrics` (string, optional): Comma-separated list of metrics to include (e.g., `impressions,engagements,clicks`)
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `25`, max: `100`)
- `sort` (string, optional): Sort by metric field (e.g., `impressions`, `engagements`)
- `order` (string, optional): Sort order - `asc`, `desc` (default: `desc`)

**Response (200):**
```json
{
  "analytics": [
    {
      "postId": "65a0b1c2d3e4f5a6b7c8d9e0",
      "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
      "platform": "twitter",
      "publishedAt": "2024-11-20T14:00:05Z",
      "metrics": {
        "impressions": 15420,
        "engagements": 823,
        "likes": 312,
        "comments": 45,
        "shares": 67,
        "clicks": 189,
        "reach": 12300,
        "saves": 28,
        "videoViews": 0,
        "engagementRate": 5.34
      },
      "lastUpdated": "2024-11-22T06:00:00Z"
    }
  ],
  "summary": {
    "totalPosts": 48,
    "totalImpressions": 245000,
    "totalEngagements": 12500,
    "averageEngagementRate": 5.1
  },
  "pagination": {
    "page": 1,
    "limit": 25,
    "total": 48,
    "totalPages": 2
  }
}
```

**Limitations:**
- Analytics data is typically available 1-2 hours after post publication.
- Historical data availability varies by platform (typically 90 days).
- Some metrics may not be available for all platforms.
- Rate limited to 100 requests per minute.

---

### Get YouTube Daily Views
**GET** `/v1/analytics/youtube/daily-views`

Returns daily view counts for YouTube videos published through Late.

**Query Parameters:**
- `accountId` (string, required): YouTube account ID
- `startDate` (string, required): Start date in ISO 8601 format
- `endDate` (string, required): End date in ISO 8601 format
- `videoId` (string, optional): Filter by specific video/post ID

**Response (200):**
```json
{
  "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
  "platform": "youtube",
  "dailyViews": [
    {
      "date": "2024-11-01",
      "views": 1250,
      "estimatedMinutesWatched": 3400,
      "averageViewDuration": 163,
      "likes": 89,
      "dislikes": 3,
      "comments": 12,
      "shares": 8,
      "subscribersGained": 5,
      "subscribersLost": 1
    },
    {
      "date": "2024-11-02",
      "views": 980,
      "estimatedMinutesWatched": 2650,
      "averageViewDuration": 162,
      "likes": 67,
      "dislikes": 1,
      "comments": 8,
      "shares": 5,
      "subscribersGained": 3,
      "subscribersLost": 0
    }
  ],
  "totals": {
    "views": 2230,
    "estimatedMinutesWatched": 6050,
    "likes": 156,
    "comments": 20,
    "shares": 13,
    "netSubscribers": 7
  }
}
```

**Limitations:**
- Data is available with a 48-72 hour delay from YouTube's reporting API.
- Maximum date range of 90 days per request.
- Only available for YouTube accounts connected through Late.

---

### Get Follower Stats
**GET** `/v1/accounts/follower-stats`

Returns follower count history and growth statistics for connected accounts. (Also documented in the Accounts API reference.)

**Query Parameters:**
- `accountId` (string, optional): Filter by specific account ID
- `profileId` (string, optional): Filter by profile ID
- `startDate` (string, optional): Start date in ISO 8601 format
- `endDate` (string, optional): End date in ISO 8601 format
- `granularity` (string, optional): Data granularity - `daily`, `weekly`, `monthly` (default: `daily`)

**Response (200):**
```json
{
  "stats": [
    {
      "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
      "platform": "twitter",
      "platformUsername": "@exampleuser",
      "currentFollowers": 5200,
      "followerHistory": [
        {
          "date": "2024-10-01",
          "followers": 4800
        },
        {
          "date": "2024-10-15",
          "followers": 5000
        },
        {
          "date": "2024-11-01",
          "followers": 5200
        }
      ],
      "growth": {
        "absolute": 400,
        "percentage": 8.33
      }
    }
  ]
}
```

---

### Get LinkedIn Aggregate Analytics
**GET** `/v1/accounts/{accountId}/linkedin-aggregate-analytics`

Returns aggregate analytics for a LinkedIn account, including total followers, impressions, and engagement over a time period.

**Path Parameters:**
- `accountId` (string, required): The LinkedIn account ID

**Query Parameters:**
- `startDate` (string, required): Start date in ISO 8601 format
- `endDate` (string, required): End date in ISO 8601 format
- `granularity` (string, optional): Data granularity - `daily`, `monthly` (default: `daily`)

**Response (200):**
```json
{
  "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
  "platform": "linkedin",
  "organizationUrn": "urn:li:organization:12345678",
  "aggregateMetrics": {
    "followers": {
      "organic": 4500,
      "paid": 200,
      "total": 4700
    },
    "pageViews": {
      "total": 8900,
      "uniqueVisitors": 5200
    },
    "impressions": {
      "organic": 125000,
      "paid": 35000,
      "total": 160000
    },
    "engagements": {
      "likes": 2300,
      "comments": 450,
      "shares": 320,
      "clicks": 1800,
      "total": 4870
    }
  },
  "timeSeries": [
    {
      "date": "2024-11-01",
      "impressions": 5200,
      "engagements": 180,
      "followers": 4650,
      "followerGrowth": 12
    },
    {
      "date": "2024-11-02",
      "impressions": 4800,
      "engagements": 165,
      "followers": 4662,
      "followerGrowth": 12
    }
  ]
}
```

**Limitations:**
- Only available for LinkedIn Organization pages (not personal profiles).
- Data is available with a 24-48 hour delay.
- Maximum date range of 365 days per request.
- Requires `r_organization_social` scope on the connected account.

---

### Get LinkedIn Post Analytics
**GET** `/v1/accounts/{accountId}/linkedin-post-analytics`

Returns per-post analytics for content published on LinkedIn through a specific account.

**Path Parameters:**
- `accountId` (string, required): The LinkedIn account ID

**Query Parameters:**
- `startDate` (string, optional): Filter posts published after this date (ISO 8601)
- `endDate` (string, optional): Filter posts published before this date (ISO 8601)
- `postId` (string, optional): Filter by specific Late post ID
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `25`, max: `100`)
- `sort` (string, optional): Sort by metric - `impressions`, `engagements`, `clicks`, `publishedAt` (default: `publishedAt`)
- `order` (string, optional): Sort order - `asc`, `desc` (default: `desc`)

**Response (200):**
```json
{
  "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
  "platform": "linkedin",
  "posts": [
    {
      "postId": "65a0b1c2d3e4f5a6b7c8d9e0",
      "platformPostUrn": "urn:li:share:7654321098765432",
      "publishedAt": "2024-11-15T10:00:00Z",
      "text": "Exciting announcement about our latest product...",
      "metrics": {
        "impressions": 8500,
        "uniqueImpressions": 7200,
        "clicks": 420,
        "likes": 180,
        "comments": 35,
        "shares": 22,
        "engagementRate": 7.73,
        "clickThroughRate": 4.94
      }
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 25,
    "total": 12,
    "totalPages": 1
  }
}
```

**Limitations:**
- Available for both personal profiles and organization pages.
- Metrics for personal profiles may be limited compared to organization pages.
- Data is refreshed every 24 hours.

---

## Error Responses

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters (e.g., invalid date format, unknown metric) |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Insufficient permissions for the requested account or profile |
| **404** | Account or post not found |
| **429** | Rate limit exceeded |
| **500** | Internal server error |

---

## Notes

- Analytics data is collected asynchronously and may not be immediately available after a post is published.
- Engagement rate is calculated as: `(total engagements / impressions) * 100`.
- Not all metrics are available for every platform. Unavailable metrics will return `0` or `null`.
- For real-time monitoring, consider using webhooks to receive notifications when analytics data is updated.

---
