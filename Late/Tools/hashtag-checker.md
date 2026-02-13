# Hashtag Checker - Late API Documentation

## Overview
The Hashtag Checker validates Instagram hashtags for bans and restrictions before posting.

## Endpoint
**POST** `/v1/tools/instagram/hashtag-checker`

### Request Body
- `hashtags` (array of strings, required) - Maximum 20 items per request

### Response (200)
```json
{
  "success": true,
  "results": [
    {
      "hashtag": "string",
      "status": "banned|restricted|safe",
      "reason": "string",
      "confidence": 0
    }
  ],
  "summary": {
    "banned": 0,
    "restricted": 0,
    "safe": 0
  }
}
```

### Rate Limits
- Build: 50/day
- Accelerate: 500/day
- Unlimited: Unlimited

### Example
```bash
curl -X POST "https://getlate.dev/api/v1/tools/instagram/hashtag-checker" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"hashtags": ["travel", "followforfollow", "fitness"]}'
```
