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
    },
    {
      "id": "accounts/123456/reviews/KlMnOpQrSt",
      "reviewer": {
        "displayName": "John Smith",
        "profilePhotoUrl": "https://lh3.googleusercontent.com/a/another-photo"
      },
      "rating": 3,
      "starRating": "THREE",
      "comment": "Decent experience but the wait was too long.",
      "createTime": "2025-07-20T18:45:00Z",
      "reviewReply": null
    }
  ],
  "nextPageToken": "eyJwYWdlIjoyLCJsaW1pdCI6MjB9",
  "totalReviewCount": 142,
  "averageRating": 4.3
}
```

### Response Fields

| Field                        | Type    | Description                                                        |
|------------------------------|---------|--------------------------------------------------------------------|
| `reviews`                    | array   | Array of review objects                                            |
| `reviews[].id`               | string  | Unique identifier of the review                                    |
| `reviews[].reviewer`         | object  | Information about the person who left the review                   |
| `reviews[].reviewer.displayName` | string | Display name of the reviewer                                   |
| `reviews[].reviewer.profilePhotoUrl` | string | URL of the reviewer's profile photo                        |
| `reviews[].rating`           | integer | Numeric rating from 1 to 5                                        |
| `reviews[].starRating`       | string  | Star rating enum: `ONE`, `TWO`, `THREE`, `FOUR`, `FIVE`           |
| `reviews[].comment`          | string  | The text content of the review. May be empty.                     |
| `reviews[].createTime`       | string  | ISO 8601 timestamp when the review was created                    |
| `reviews[].reviewReply`      | object  | The owner's reply to the review, or `null` if no reply exists     |
| `reviews[].reviewReply.comment`    | string | Text content of the reply                                   |
| `reviews[].reviewReply.updateTime` | string | ISO 8601 timestamp when the reply was last updated          |
| `nextPageToken`              | string  | Token to retrieve the next page of results. Absent on last page.  |
| `totalReviewCount`           | integer | Total number of reviews for this account                          |
| `averageRating`              | number  | Average star rating across all reviews                            |

## Error Codes

| Status Code | Description                                                                 |
|-------------|-----------------------------------------------------------------------------|
| `200`       | Success. Returns the list of reviews.                                       |
| `400`       | Bad Request. Invalid query parameters (e.g., `pageSize` exceeds maximum).   |
| `401`       | Unauthorized. Missing or invalid authentication token.                      |
| `403`       | Forbidden. The authenticated user does not have access to this account.     |
| `404`       | Not Found. The specified `accountId` does not exist.                        |
| `500`       | Internal Server Error. An unexpected error occurred on the server.          |

## Example

### cURL

```bash
curl -X GET \
  "https://api.example.com/v1/accounts/123456/gmb-reviews?pageSize=10&pageToken=eyJwYWdlIjoxfQ" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json"
```

### Pagination

To paginate through all reviews, use the `nextPageToken` returned in each response as the `pageToken` query parameter in the subsequent request. When `nextPageToken` is absent from the response, you have reached the last page.

```bash
# First request
curl -X GET \
  "https://api.example.com/v1/accounts/123456/gmb-reviews?pageSize=20" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"

# Subsequent request using nextPageToken
curl -X GET \
  "https://api.example.com/v1/accounts/123456/gmb-reviews?pageSize=20&pageToken=eyJwYWdlIjoyLCJsaW1pdCI6MjB9" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Notes

- Reviews are returned in reverse chronological order (newest first).
- The `reviewReply` field is `null` when the business owner has not replied to a review.
- The `comment` field on a review may be an empty string if the reviewer only left a star rating without text.
- Rate limits apply. See the general API rate limiting documentation for details.
