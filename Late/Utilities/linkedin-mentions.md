# LinkedIn Mentions API

Search for and retrieve LinkedIn person and organization mentions to use when composing posts or comments.

## Endpoint

```
GET /v1/accounts/{accountId}/linkedin-mentions
```

### Path Parameters

| Parameter   | Type   | Required | Description                          |
|-------------|--------|----------|--------------------------------------|
| `accountId` | string | Yes      | The unique identifier of the account |

### Query Parameters

| Parameter     | Type   | Required | Description                                                                         |
|---------------|--------|----------|-------------------------------------------------------------------------------------|
| `url`         | string | No       | LinkedIn profile or organization URL to look up directly.                           |
| `displayName` | string | No       | Display name to search for. Searches across both persons and organizations.         |

At least one of `url` or `displayName` must be provided.

## Mention Types

The API supports two types of mentions:

### Person Mentions

Person mentions reference individual LinkedIn users. They use the URN format `urn:li:person:{id}`.

**Important:** Person mentions require that the authenticated user has organization admin access. Without org admin access, person mention lookups will fail with a `403` error. This is a LinkedIn platform restriction.

### Organization Mentions

Organization mentions reference LinkedIn company pages. They use the URN format `urn:li:organization:{id}`.

Organization mentions do not require special admin access beyond standard account authentication.

## Response Fields

| Field                        | Type   | Description                                                      |
|------------------------------|--------|------------------------------------------------------------------|
| `mentions`                   | array  | Array of mention objects matching the search criteria             |
| `mentions[].urn`             | string | The LinkedIn URN identifier for the person or organization       |
| `mentions[].type`            | string | The type of mention: `PERSON` or `ORGANIZATION`                  |
| `mentions[].displayName`     | string | The display name of the person or organization                   |
| `mentions[].vanityName`      | string | The LinkedIn vanity name (URL slug)                              |
| `mentions[].profileUrl`      | string | Full LinkedIn profile or page URL                                |
| `mentions[].profileImageUrl` | string | URL of the profile image, or `null` if not available             |
| `mentions[].mentionFormat`   | string | The formatted mention string to use in post content              |
| `totalResults`               | integer| Total number of matching results                                 |

### The `mentionFormat` Field

The `mentionFormat` field contains the properly formatted mention string that should be used when including the mention in a LinkedIn post or comment body. This format is required by LinkedIn for mentions to render correctly as clickable links.

Format: `@[{displayName}](urn:li:{type}:{id})`

Examples:
- Person: `@[Jane Doe](urn:li:person:abc123)`
- Organization: `@[Acme Corp](urn:li:organization:12345678)`

## Request Examples

### Search by Display Name

```bash
curl -X GET \
  "https://api.example.com/v1/accounts/123456/linkedin-mentions?displayName=Acme" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Search by LinkedIn URL

```bash
curl -X GET \
  "https://api.example.com/v1/accounts/123456/linkedin-mentions?url=https://www.linkedin.com/company/acme-corp" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Search for a Person by URL

```bash
curl -X GET \
  "https://api.example.com/v1/accounts/123456/linkedin-mentions?url=https://www.linkedin.com/in/janedoe" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Response Examples

### Organization Search Results

```json
{
  "mentions": [
    {
      "urn": "urn:li:organization:12345678",
      "type": "ORGANIZATION",
      "displayName": "Acme Corporation",
      "vanityName": "acme-corp",
      "profileUrl": "https://www.linkedin.com/company/acme-corp",
      "profileImageUrl": "https://media.licdn.com/dms/image/C4E0BAQExample/company-logo.jpg",
      "mentionFormat": "@[Acme Corporation](urn:li:organization:12345678)"
    },
    {
      "urn": "urn:li:organization:87654321",
      "type": "ORGANIZATION",
      "displayName": "Acme Technologies Inc.",
      "vanityName": "acme-technologies",
      "profileUrl": "https://www.linkedin.com/company/acme-technologies",
      "profileImageUrl": "https://media.licdn.com/dms/image/C4E0BAQExample2/company-logo.jpg",
      "mentionFormat": "@[Acme Technologies Inc.](urn:li:organization:87654321)"
    }
  ],
  "totalResults": 2
}
```

### Person Search Result

```json
{
  "mentions": [
    {
      "urn": "urn:li:person:abc123def456",
      "type": "PERSON",
      "displayName": "Jane Doe",
      "vanityName": "janedoe",
      "profileUrl": "https://www.linkedin.com/in/janedoe",
      "profileImageUrl": "https://media.licdn.com/dms/image/C4D03AQExample/profile.jpg",
      "mentionFormat": "@[Jane Doe](urn:li:person:abc123def456)"
    }
  ],
  "totalResults": 1
}
```

### No Results

```json
{
  "mentions": [],
  "totalResults": 0
}
```

## Using Mentions in Posts

When composing a LinkedIn post or comment that includes a mention, insert the `mentionFormat` value directly into the text body:

```
Excited to announce our partnership with @[Acme Corporation](urn:li:organization:12345678)!
Looking forward to working with @[Jane Doe](urn:li:person:abc123def456) and the team.
```

The mention will render as a clickable link when the post is published on LinkedIn.

## Error Responses

| Status Code | Description                                                                                          |
|-------------|------------------------------------------------------------------------------------------------------|
| `200`       | Success. Returns the list of matching mentions.                                                      |
| `400`       | Bad Request. Neither `url` nor `displayName` was provided, or the parameters are malformed.          |
| `401`       | Unauthorized. Missing or invalid authentication token.                                               |
| `403`       | Forbidden. The authenticated user does not have the required access. For person mentions, org admin access is required. |
| `404`       | Not Found. The specified `accountId` does not exist.                                                 |
| `422`       | Unprocessable Entity. The provided URL is not a valid LinkedIn profile or company page URL.           |
| `429`       | Too Many Requests. LinkedIn API rate limit exceeded. Retry after the specified delay.                 |
| `500`       | Internal Server Error. An unexpected error occurred on the server.                                   |

## Notes

- The `displayName` search performs a case-insensitive prefix match.
- When using `url`, the API attempts an exact match against the LinkedIn profile or company page URL.
- Person mentions require org admin access on the authenticated LinkedIn account. This is enforced by the LinkedIn platform, not by this API.
- The `mentionFormat` string must be used exactly as provided. Modifying the format may cause the mention to not render correctly on LinkedIn.
- Search results are limited to a maximum of 25 results per request.
- The `profileImageUrl` may be `null` if the person or organization has not set a profile image, or if the image is not accessible.
- URN identifiers are stable and do not change even if the user or organization updates their vanity name or display name.