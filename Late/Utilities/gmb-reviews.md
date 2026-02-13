# GMB Reviews API Reference

Retrieve and manage Google My Business reviews for a given account.

## Endpoint

```
GET /v1/accounts/{accountId}/gmb-reviews
```

### Path Parameters

| Parameter   | Type   | Required | Description                          |
|-------------|--------|----------|--------------------------------------|
| `accountId` | string | Yes      | The unique identifier of the account |

### Query Parameters

| Parameter   | Type    | Required | Description                                                                 |
|-------------|---------|----------|-----------------------------------------------------------------------------|
| `pageSize`  | integer | No       | Maximum number of reviews to return per page. Defaults to 20. Max is 100.   |
| `pageToken` | string  | No       | Token for retrieving the next page of results from a previous response.     |

## Response

### Success Response (200)

```json
{
  "reviews": [
    {
      "id": "accounts/123456/reviews/AbCdEfGhIj",
      "reviewer": {
        "displayName": "Jane Doe",
        "profilePhotoUrl": "https://lh3.googleusercontent.com/a/photo-url"
      },
      "rating": 5,
      "starRating": "FIVE",
      "comment": "Excellent service and great food! Highly recommend.",
      "createTime": "2025-08-15T14:30:00Z",
      "reviewReply": {
        "comment": "Thank you for your kind words, Jane!",
        "updateTime": "2025-08-16T09:00:00Z"
      }
    }
  ],
  "nextPageToken": "eyJwYWdlIjoyLCJsaW1pdCI6MjB9",
  "totalReviewCount": 142,
  "averageRating": 4.3
}
```

## Error Codes

| Status Code | Description                                                                 |
|-------------|-----------------------------------------------------------------------------|
| `200`       | Success. Returns the list of reviews.                                       |
| `400`       | Bad Request. Invalid query parameters.                                      |
| `401`       | Unauthorized. Missing or invalid authentication token.                      |
| `403`       | Forbidden. The authenticated user does not have access to this account.     |
| `404`       | Not Found. The specified `accountId` does not exist.                        |
| `500`       | Internal Server Error.                                                      |

## Notes

- Reviews are returned in reverse chronological order (newest first).
- The `reviewReply` field is `null` when the business owner has not replied to a review.
- Rate limits apply. See the general API rate limiting documentation for details.
