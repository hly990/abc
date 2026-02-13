# Late API Authentication Documentation

## Overview

The Late API requires authentication using API keys for all requests. This documentation covers obtaining, formatting, and using API keys securely.

## Getting Your API Key

To obtain an API key:

1. Log in at [getlate.dev](https://getlate.dev)
2. Navigate to **Settings → API Keys**
3. Click **Create API Key**
4. Name your key (e.g., "My App" or "CI/CD Pipeline")
5. Copy immediately—the key displays only once

## API Key Format

| Component | Details |
|-----------|---------|
| **Prefix** | `sk_` (3 characters) |
| **Body** | 32 random bytes as hex (64 characters) |
| **Total Length** | 67 characters |

**Example:** `sk_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v`

**Dashboard Preview:** `sk_a1b2c...d0e1f2`

### Key Security Notes

- Keys appear only at creation time
- Stored as SHA-256 hashes (never in plain text)
- Limited to 10 active keys per user

## Making Authenticated Requests

Include your API key in the `Authorization` header as a Bearer token:

```bash
curl https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### List Posts Example

```bash
curl https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer sk_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v"
```

### Create Post Example

```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer sk_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Hello from the API!",
    "platforms": [
      {"platform": "twitter", "accountId": "acc_123"}
    ]
  }'
```

## Environment Variables

Never hardcode API keys. Use environment variables instead:

```bash
export LATE_API_KEY="sk_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v"

curl https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer $LATE_API_KEY"
```

### Node.js Implementation

```javascript
const apiKey = process.env.LATE_API_KEY;

const response = await fetch('https://getlate.dev/api/v1/posts', {
  headers: {
    'Authorization': `Bearer ${apiKey}`,
    'Content-Type': 'application/json'
  }
});
```

### Python Implementation

```python
import os
import requests

api_key = os.environ.get('LATE_API_KEY')

response = requests.get(
    'https://getlate.dev/api/v1/posts',
    headers={'Authorization': f'Bearer {api_key}'}
)
```

## Error Responses

Failed authentication returns a `401 Unauthorized` response:

```json
{
  "error": "Invalid or missing API key"
}
```

Common causes:
- Missing `Authorization` header
- Typo in API key
- Deleted or expired key
- Using `API_KEY` instead of `Bearer API_KEY`

## Security Best Practices

- Never share API keys publicly or commit to version control
- Store keys in environment variables
- Create separate keys for different applications
- Delete unused keys from your dashboard
- Rotate keys periodically

## Programmatic Key Management

| Endpoint | Purpose |
|----------|---------|
| `GET /v1/api-keys` | List your API keys |
| `POST /v1/api-keys` | Create a new API key |
| `DELETE /v1/api-keys/{keyId}` | Delete an API key |

See the [API Keys reference](/management/api-keys) for complete details.

## Next Steps

After authentication setup, proceed to the [Quickstart guide](/quickstart) to schedule your first social media post.
