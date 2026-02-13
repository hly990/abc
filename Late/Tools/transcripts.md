# Transcripts - Late API Documentation

## Overview
The Transcripts tool enables extraction of transcripts and captions from YouTube videos through the Late API.

## Endpoint
**GET** `/v1/tools/youtube/transcript`

### Rate Limits
- Build: 50/day
- Accelerate: 500/day
- Unlimited: Unlimited requests

### Query Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | Yes | YouTube video URL or video ID |
| `lang` | string | No | Language code for transcript (default: `"en"`) |

### Response (200 Success)
```json
{
  "success": true,
  "videoId": "string",
  "language": "string",
  "fullText": "string",
  "segments": [
    {
      "text": "string",
      "start": 0,
      "duration": 0
    }
  ]
}
```

### Example cURL
```bash
curl -X GET "https://getlate.dev/api/v1/tools/youtube/transcript?url=VIDEO_URL" \
  -H "Authorization: Bearer YOUR_API_KEY"
```
