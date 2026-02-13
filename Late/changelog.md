# Late API Changelog

All notable changes to the Late API are documented here.

---

## February 12, 2025

### Reddit Flairs Support
- Added support for Reddit flairs when creating posts.
- New optional `flairId` and `flairText` fields in the post creation body for Reddit posts.
- Use `GET /v1/accounts/{accountId}/reddit/flairs?subreddit=SUBREDDIT` to retrieve available flairs for a subreddit.

---

## February 11, 2025

### Instagram Profile Context
- Instagram posts now support profile context tagging.
- Added `profileContext` field for Instagram-specific post metadata.

### Interactive Messaging
- Introduced interactive messaging support for direct messages.
- New message types: buttons, quick replies, and card carousels.

### Account Settings
- New `GET /v1/accounts/{accountId}/settings` endpoint to retrieve platform-specific account settings.
- New `PATCH /v1/accounts/{accountId}/settings` endpoint to update account configuration.

---

## February 10, 2025

### New Logging Endpoints
- Added `GET /v1/logs` endpoint to retrieve API request logs.
- Added `GET /v1/logs/{logId}` endpoint to retrieve a specific log entry.
- Logs include request method, path, status code, response time, and timestamp.
- Filter logs by date range, status code, and endpoint path.

---

## February 5, 2025

### Google Business Profile Endpoints
- Added full support for Google Business Profile (GBP) as a platform.
- New endpoints:
  - `POST /v1/posts` now accepts `"google_business"` as a platform.
  - `GET /v1/accounts/{accountId}/google-business/locations` to list business locations.
  - `GET /v1/accounts/{accountId}/google-business/reviews` to retrieve reviews.
- Supports local posts, events, offers, and product updates.

---

## February 4, 2025

### Twitter/X Inbox
- Added inbox support for Twitter/X direct messages.
- New endpoints:
  - `GET /v1/inbox/twitter/messages` to list DM conversations.
  - `POST /v1/inbox/twitter/messages` to send a direct message.
  - `GET /v1/inbox/twitter/messages/{conversationId}` to retrieve a conversation thread.

### Saves Tracking
- Post analytics now include save/bookmark counts where supported by the platform.
- New `saves` field in the post analytics response for Instagram and Facebook.

---

## February 3, 2025

### Analytics Source Filtering
- Analytics endpoints now support a `source` query parameter to filter metrics by traffic source.
- Supported sources: `organic`, `paid`, `viral`, `direct`.
- Applies to `GET /v1/analytics/posts` and `GET /v1/analytics/accounts`.

---

## February 2, 2025

### Enhanced Error Responses
- All error responses now include a machine-readable `code` field alongside the human-readable `message`.
- New error codes: `INVALID_MEDIA_FORMAT`, `PLATFORM_RATE_LIMITED`, `CONTENT_TOO_LONG`, `DUPLICATE_POST`, `ACCOUNT_DISCONNECTED`.
- Error responses now include a `details` array for validation errors with field-level specificity.

### YouTube categoryId
- Added support for `categoryId` when creating YouTube video posts.
- New optional `categoryId` field in the post creation body for YouTube uploads.
- Use `GET /v1/accounts/{accountId}/youtube/categories` to retrieve the list of available video categories.

---

## January 30, 2025

### Multi-Page and Multi-Location Posting
- Added support for posting to multiple Facebook Pages and Google Business locations in a single API call.
- New `accountIds` array field in the post creation body allows targeting multiple accounts on the same platform.
- Responses include per-account status for each target.

---

## January 25, 2025

### Duplicate Detection (409 Conflict)
- The API now detects and rejects duplicate posts with a `409 Conflict` status code.
- Duplicate detection compares content, platforms, and scheduled time within a configurable window.
- Response includes the `existingPostId` of the detected duplicate for reference.

### Rate Limit Headers
- All API responses now include standard rate limit headers:
  - `X-RateLimit-Limit` - Maximum requests allowed per window.
  - `X-RateLimit-Remaining` - Requests remaining in the current window.
  - `X-RateLimit-Reset` - Unix timestamp when the rate limit window resets.
- Rate limits are enforced per API key at the per-minute level based on your plan tier.

---

## January 22, 2025

### LinkedIn OAuth Update
- Updated LinkedIn OAuth flow to use the latest OpenID Connect scopes.
- New required scopes: `openid`, `profile`, `email`, `w_member_social`.
- Existing LinkedIn connections will continue to work; re-authorization is only required when tokens expire.
- Added `GET /v1/accounts/{accountId}/linkedin/permissions` to check current OAuth scope status.
