# Reviews - API Reference

## Overview
The Reviews API provides endpoints for managing reviews on connected business accounts. Currently supports Google Business Profile reviews. You can list reviews, post replies, and delete replies.

---

## Endpoints

### List Reviews
**GET** `/v1/inbox/reviews`

Returns a paginated list of reviews across connected business accounts.

**Query Parameters:**
- `profileId` (string, optional): Filter by profile ID
- `accountId` (string, optional): Filter by specific account ID
- `platform` (string, optional): Filter by platform (e.g., `google-business`)
- `rating` (integer, optional): Filter by star rating (1-5)
- `hasReply` (boolean, optional): When `true`, returns only reviews with replies; when `false`, returns only reviews without replies
- `startDate` (string, optional): Filter reviews created after this date (ISO 8601)
- `endDate` (string, optional): Filter reviews created before this date (ISO 8601)
- `page` (integer, optional): Page number (default: `1`)
- `limit` (integer, optional): Results per page (default: `25`, max: `100`)
- `sort` (string, optional): Sort field - `createdAt`, `rating` (default: `createdAt`)
- `order` (string, optional): Sort order - `asc`, `desc` (default: `desc`)

**Response (200):**
```json
{
  "reviews": [
    {
      "_id": "rev_a1b2c3d4e5f6",
      "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
      "platform": "google-business",
      "platformReviewId": "AbCdEfGhIjKlMnOp",
      "reviewer": {
        "displayName": "John Smith",
        "avatarUrl": "https://lh3.googleusercontent.com/.../photo.jpg",
        "isAnonymous": false
      },
      "rating": 5,
      "text": "Excellent service! The staff was very friendly and helpful. Highly recommend this place.",
      "reply": {
        "text": "Thank you, John! We're glad you had a great experience.",
        "repliedAt": "2024-11-16T09:00:00Z"
      },
      "createdAt": "2024-11-15T14:30:00Z",
      "updatedAt": "2024-11-15T14:30:00Z"
    },
    {
      "_id": "rev_b2c3d4e5f6g7",
      "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
      "platform": "google-business",
      "platformReviewId": "QrStUvWxYzAbCdEf",
      "reviewer": {
        "displayName": "Jane Doe",
        "avatarUrl": "https://lh3.googleusercontent.com/.../photo.jpg",
        "isAnonymous": false
      },
      "rating": 2,
      "text": "The wait was too long and the food was cold.",
      "reply": null,
      "createdAt": "2024-11-14T18:00:00Z",
      "updatedAt": "2024-11-14T18:00:00Z"
    }
  ],
  "summary": {
    "totalReviews": 156,
    "averageRating": 4.3,
    "ratingDistribution": {
      "1": 5,
      "2": 8,
      "3": 15,
      "4": 42,
      "5": 86
    },
    "repliedCount": 120,
    "unrepliedCount": 36
  },
  "pagination": {
    "page": 1,
    "limit": 25,
    "total": 156,
    "totalPages": 7
  }
}
```

---

### Reply to Review
**POST** `/v1/inbox/reviews/{reviewId}/reply`

Posts a reply to a specific review. If a reply already exists, it will be replaced.

**Path Parameters:**
- `reviewId` (string, required): The review ID

**Request Body:**
- `text` (string, required): Reply text content (max 4096 characters for Google Business)

**Request Example:**
```json
{
  "text": "Thank you for your feedback, Jane. We're sorry about your experience. We've addressed the issue with our team and would love the opportunity to make it right. Please contact us directly at support@example.com."
}
```

**Response (201):**
```json
{
  "message": "Reply posted successfully",
  "review": {
    "_id": "rev_b2c3d4e5f6g7",
    "platformReviewId": "QrStUvWxYzAbCdEf",
    "reply": {
      "text": "Thank you for your feedback, Jane. We're sorry about your experience. We've addressed the issue with our team and would love the opportunity to make it right. Please contact us directly at support@example.com.",
      "repliedAt": "2024-11-16T10:00:00Z"
    }
  }
}
```

**Error Responses:**
- **400:** Invalid request body or empty text
- **401:** Unauthorized access
- **403:** Insufficient permissions to reply to reviews for this account
- **404:** Review not found
- **422:** Reply text exceeds platform character limit
- **429:** Rate limit exceeded

---

### Delete Reply
**DELETE** `/v1/inbox/reviews/{reviewId}/reply`

Deletes the reply from a specific review.

**Path Parameters:**
- `reviewId` (string, required): The review ID

**Response (200):**
```json
{
  "message": "Reply deleted successfully",
  "review": {
    "_id": "rev_b2c3d4e5f6g7",
    "platformReviewId": "QrStUvWxYzAbCdEf",
    "reply": null
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **403:** Insufficient permissions to manage replies for this account
- **404:** Review not found or no reply exists

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters or body |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Insufficient permissions |
| **404** | Review not found or no reply to delete |
| **422** | Content exceeds platform constraints |
| **429** | Rate limit exceeded |
| **500** | Internal server error |

---

## Notes

- Reviews are synced periodically from connected platforms. New reviews may take up to 15 minutes to appear.
- Replying to a review that already has a reply will overwrite the existing reply.
- The `rating` field uses a 1-5 integer scale where 1 is the lowest and 5 is the highest.
- Anonymous reviews will have `isAnonymous: true` and a generic display name.
- Review deletion is not supported -- only reply management is available through the API.

---
