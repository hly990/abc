# Reviews - API Reference

## Overview
The Reviews API provides endpoints for managing reviews on connected business accounts. Currently supports Google Business Profile reviews. You can list reviews, post replies, and delete replies.

---

## Endpoints

### List Reviews
**GET** `/v1/inbox/reviews`

Returns a paginated list of reviews across connected business accounts.

### Reply to Review
**POST** `/v1/inbox/reviews/{reviewId}/reply`

Posts a reply to a specific review. If a reply already exists, it will be replaced.

### Delete Reply
**DELETE** `/v1/inbox/reviews/{reviewId}/reply`

Deletes the reply from a specific review.

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
- The `rating` field uses a 1-5 integer scale.
- Review deletion is not supported -- only reply management is available through the API.
