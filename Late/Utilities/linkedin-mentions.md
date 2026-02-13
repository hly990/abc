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
| `displayName` | string | No       | Display name to search for.                                                         |

At least one of `url` or `displayName` must be provided.

## Mention Types

### Person Mentions
Person mentions reference individual LinkedIn users. URN format: `urn:li:person:{id}`. Requires organization admin access.

### Organization Mentions
Organization mentions reference LinkedIn company pages. URN format: `urn:li:organization:{id}`.

## Using Mentions in Posts

Insert the `mentionFormat` value directly into the text body:
```
Excited to announce our partnership with @[Acme Corporation](urn:li:organization:12345678)!
```

## Error Responses

| Status Code | Description                                                                                          |
|-------------|------------------------------------------------------------------------------------------------------|
| `200`       | Success. Returns the list of matching mentions.                                                      |
| `400`       | Bad Request. Neither `url` nor `displayName` was provided.                                           |
| `401`       | Unauthorized.                                                                                        |
| `403`       | Forbidden. For person mentions, org admin access is required.                                        |
| `404`       | Not Found. The specified `accountId` does not exist.                                                 |
| `429`       | Too Many Requests. LinkedIn API rate limit exceeded.                                                 |
| `500`       | Internal Server Error.                                                                               |

## Notes

- The `displayName` search performs a case-insensitive prefix match.
- Search results are limited to a maximum of 25 results per request.
- URN identifiers are stable and do not change even if the user or organization updates their vanity name or display name.
