# Tools API Overview

## Overview
The Tools API provides media download and utility features for working with social media content. These endpoints help you download videos, extract transcripts, check hashtags, and generate AI captions.

**Important**: Tools API endpoints are available exclusively to Build, Accelerate, and Unlimited paid plans.

## Rate Limiting

| Plan | Daily Limit |
|------|------------|
| Build | 50 requests/day |
| Accelerate | 500 requests/day |
| Unlimited | Unlimited |

Rate limit information appears in response headers:
- `X-RateLimit-Limit` - Your daily request allocation
- `X-RateLimit-Remaining` - Available requests remaining today
- `X-RateLimit-Reset` - Unix timestamp indicating when your limit resets

## Available Tools

### Media Downloads
Download videos from multiple platforms including YouTube, Instagram, TikTok, Twitter/X, Facebook, LinkedIn, and Bluesky.

### Transcripts
Extract transcripts and captions from YouTube video content.

### Hashtag Checker
Validate Instagram hashtags for bans and usage restrictions before posting.

## Error Responses

| Status | Description |
|--------|------------|
| 401 | Unauthorized — Missing or invalid API credentials |
| 403 | Forbidden — Your plan lacks Tools API access |
| 429 | Rate limit exceeded — Wait until reset timestamp |
| 404 | Not found — Requested content unavailable or URL invalid |
