# Migrating from Ayrshare to Late

## Overview
This guide walks you through migrating your social media integration from Ayrshare to the Late API. Whether you use the Ayrshare SDK or make direct API calls, Late provides equivalent functionality with a straightforward migration path.

## Migration Paths

### Path 1: Drop-in SDK Replacement
If you are using the Ayrshare Node.js SDK, the fastest migration is swapping the SDK package.

**Step 1**: Uninstall the Ayrshare SDK and install the Late SDK.

```bash
npm uninstall social-post-api
npm install @getlate/sdk
```

**Step 2**: Update your import and initialization.

```javascript
// Before (Ayrshare)
const SocialPost = require('social-post-api');
const social = new SocialPost('AYRSHARE_API_KEY');

// After (Late)
const Late = require('@getlate/sdk');
const late = new Late('LATE_API_KEY');
```

The Late SDK mirrors the Ayrshare SDK method signatures where possible, so most calls require minimal changes.

### Path 2: Full API Migration
If you make direct HTTP calls to the Ayrshare REST API, follow the step-by-step migration below to update your endpoints, request bodies, and authentication.

---

## Quick Reference: Ayrshare vs Late

| Feature | Ayrshare | Late |
|---------|----------|------|
| **Base URL** | `https://app.ayrshare.com/api` | `https://getlate.dev/api/v1` |
| **Content field** | `post` | `content` |
| **Platforms format** | `["twitter", "facebook"]` | `["twitter", "facebook"]` |
| **Media format** | `mediaUrls: ["url"]` | `media: [{ url: "url" }]` |
| **Schedule** | `scheduleDate: "ISO8601"` | `scheduledAt: "ISO8601"` |
| **Publish now** | `POST /post` | `POST /posts/publish` |
| **Multi-user** | Profile keys | Profile IDs via `profileId` header |

---

## Step-by-Step Migration

### Step 1: Create Your Late Account and Profiles

1. Sign up at [getlate.dev](https://getlate.dev) and choose a plan (Build, Accelerate, or Unlimited).
2. Generate an API key from **Settings > API Keys**.
3. Create profiles to match your Ayrshare profile keys. Each profile in Late corresponds to a set of connected social accounts.

```bash
curl -X POST "https://getlate.dev/api/v1/profiles" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"name": "My Brand Profile"}'
```

### Step 2: Connect Social Accounts

For each profile, connect the same social accounts you had in Ayrshare.

```bash
curl -X POST "https://getlate.dev/api/v1/profiles/PROFILE_ID/accounts" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"platform": "twitter", "callbackUrl": "https://yourdomain.com/callback"}'
```

This returns an OAuth URL. Open it in a browser to authorize the account.

### Step 3: Retrieve Connected Account IDs

After connecting, retrieve the account IDs you will use in post calls.

```bash
curl -X GET "https://getlate.dev/api/v1/profiles/PROFILE_ID/accounts" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

Response:
```json
{
  "success": true,
  "accounts": [
    {
      "id": "acc_abc123",
      "platform": "twitter",
      "username": "@yourbrand",
      "status": "connected"
    }
  ]
}
```

### Step 4: Update Post Calls

Replace your Ayrshare post calls with Late equivalents.

**Ayrshare (before)**:
```javascript
const response = await social.post({
  post: "Check out our new product!",
  platforms: ["twitter", "facebook", "instagram"],
  mediaUrls: ["https://example.com/image.jpg"],
  scheduleDate: "2025-03-01T10:00:00Z",
  profileKey: "PROFILE_KEY"
});
```

**Late (after)**:
```javascript
const response = await late.posts.create({
  content: "Check out our new product!",
  platforms: ["twitter", "facebook", "instagram"],
  media: [{ url: "https://example.com/image.jpg" }],
  scheduledAt: "2025-03-01T10:00:00Z",
  profileId: "PROFILE_ID"
});
```

**cURL equivalent**:
```bash
curl -X POST "https://getlate.dev/api/v1/posts" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Check out our new product!",
    "platforms": ["twitter", "facebook", "instagram"],
    "media": [{ "url": "https://example.com/image.jpg" }],
    "scheduledAt": "2025-03-01T10:00:00Z",
    "profileId": "PROFILE_ID"
  }'
```

### Step 5: Media Uploads with Presigned URLs

Late uses presigned URLs for media uploads instead of accepting direct URLs in some cases. This is the recommended approach for large files and ensures reliable delivery.

**Step 5a**: Request a presigned upload URL.

```bash
curl -X POST "https://getlate.dev/api/v1/media/upload-link" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"fileName": "product-photo.jpg", "contentType": "image/jpeg"}'
```

**Step 5b**: Upload the file to the presigned URL.

```bash
curl -X PUT "PRESIGNED_URL" \
  -H "Content-Type: image/jpeg" \
  --data-binary @product-photo.jpg
```

**Step 5c**: Use the returned media URL in your post.

```bash
curl -X POST "https://getlate.dev/api/v1/posts" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "New product launch!",
    "platforms": ["instagram"],
    "media": [{ "url": "RETURNED_MEDIA_URL" }],
    "profileId": "PROFILE_ID"
  }'
```

### Step 6: Migrate Scheduled Posts

If you have future-scheduled posts in Ayrshare, export them and recreate them in Late.

1. **Export from Ayrshare**: Use the Ayrshare `GET /post` endpoint with a date filter to retrieve all pending scheduled posts.
2. **Transform**: Map each post to the Late format using the field mapping in the quick reference table above.
3. **Import to Late**: Create each post via `POST /v1/posts` with the `scheduledAt` field set to the original schedule time.

```javascript
// Node.js migration script example
const Late = require('@getlate/sdk');
const late = new Late('LATE_API_KEY');

const ayrshareScheduledPosts = [
  // ... exported posts from Ayrshare
];

async function migratePosts(posts) {
  for (const post of posts) {
    try {
      const result = await late.posts.create({
        content: post.post,                   // Ayrshare "post" -> Late "content"
        platforms: post.platforms,
        media: (post.mediaUrls || []).map(url => ({ url })),  // Transform media format
        scheduledAt: post.scheduleDate,       // Ayrshare "scheduleDate" -> Late "scheduledAt"
        profileId: 'LATE_PROFILE_ID'
      });
      console.log(`Migrated: ${result.id}`);
    } catch (err) {
      console.error(`Failed to migrate post: ${err.message}`);
    }
  }
}

migratePosts(ayrshareScheduledPosts);
```

---

## Cutover Strategy

A phased approach minimizes risk during the migration.

### Phase 1: Prep (Week 1)
- Create your Late account and profiles.
- Connect all social accounts.
- Set up API keys and configure your environment.
- Run test posts to a staging/test social account.

### Phase 2: Pilot (Week 2)
- Route 10-20% of new posts through Late while keeping Ayrshare active.
- Compare delivery and formatting across both systems.
- Verify webhooks, scheduling, and media uploads work as expected.

### Phase 3: Rollout (Week 3)
- Route all new posts through Late.
- Keep Ayrshare active in read-only mode for any remaining scheduled posts to finish publishing.
- Monitor error rates and delivery metrics.

### Phase 4: Cutoff (Week 4)
- Confirm all Ayrshare scheduled posts have been published or migrated.
- Remove Ayrshare SDK and API key references from your codebase.
- Cancel your Ayrshare subscription.

---

## Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| `401 Unauthorized` | Invalid or missing API key | Verify your Late API key in the Authorization header |
| `400 Bad Request` on posts | Incorrect field names | Ensure you use `content` instead of `post`, `scheduledAt` instead of `scheduleDate` |
| Media not appearing | Direct URL not accessible | Use the presigned URL upload flow (Step 5) for reliable media delivery |
| `404 Profile not found` | Wrong profile ID | Retrieve profile IDs via `GET /v1/profiles` and verify the ID |
| Posts publishing to wrong accounts | Profile mismatch | Verify connected accounts under the correct profile with `GET /v1/profiles/PROFILE_ID/accounts` |
| `429 Rate Limit` | Too many requests | Check your plan limits; upgrade or implement request throttling |
| Schedule time in the past | Timezone mismatch | Always use UTC ISO 8601 format for `scheduledAt` values |
| Webhook not firing | Webhook not configured | Register your webhook URL via `POST /v1/webhooks` |

---

## Additional Resources

- [Late API Documentation](https://getlate.dev/docs)
- [Late SDKs](https://getlate.dev/docs/sdks)
- [Late API Status](https://status.getlate.dev)
- [Support](mailto:support@getlate.dev)
