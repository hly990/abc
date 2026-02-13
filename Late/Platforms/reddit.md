# Reddit API - Late Documentation

## Overview

The Late API enables posting to Reddit with support for text posts, link posts, and image posts. Video uploads are not supported via the API due to Reddit's limitations.

## Quick Start

**Create a text post to Reddit:**

```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "What is your favorite programming language and why?\n\nI have been using Python for years but considering learning Rust.",
    "platforms": [
      {"platform": "reddit", "accountId": "YOUR_ACCOUNT_ID"}
    ],
    "publishNow": true
  }'
```

## Supported Post Types

| Type | Status |
|------|--------|
| Text posts | ✅ Supported |
| Link posts | ✅ Supported |
| Image posts (single) | ✅ Supported |
| Video posts | ❌ Not supported via API |

## Image Requirements

| Property | Specification |
|----------|---------------|
| Maximum images per post | 1 |
| Accepted formats | JPEG, PNG, GIF |
| Maximum file size | 20 MB |
| Recommended dimensions | 1200 × 628 px |

### Aspect Ratio Guidelines

Supported ratios include 16:9 (landscape), 4:3 (classic), 1:1 (square), and 9:16 (mobile screenshots).

## Image Post Example

```javascript
const response = await fetch('https://getlate.dev/api/v1/posts', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_API_KEY',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    content: 'Check out this view from my hike!',
    mediaItems: [
      { type: 'image', url: 'https://example.com/hiking-photo.jpg' }
    ],
    platforms: [
      { platform: 'reddit', accountId: 'YOUR_ACCOUNT_ID' }
    ],
    publishNow: true
  })
});

const { post } = await response.json();
console.log('Posted to Reddit!', post._id);
```

## Link Posts

Share URLs by setting the link field:

```json
{
  "content": "Interesting article about AI",
  "link": "https://example.com/article",
  "platforms": [
    { "platform": "reddit", "accountId": "acc_123" }
  ]
}
```

The content becomes the post title when a link is provided.

## Subreddit Selection

Posts submit to the subreddit configured on the connected Reddit account by default. To target a specific subreddit per post, use `platformSpecificData.subreddit` without the `r/` prefix.

## Post Flairs

Some subreddits require post flairs. Retrieve available flairs:

```bash
curl -X GET "https://getlate.dev/api/v1/accounts/YOUR_ACCOUNT_ID/reddit-flairs?subreddit=socialmedia" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

Then create a post with `flairId`:

```json
{
  "content": "What is your favorite programming language and why?",
  "platforms": [
    {
      "platform": "reddit",
      "accountId": "YOUR_ACCOUNT_ID",
      "platformSpecificData": {
        "subreddit": "socialmedia",
        "flairId": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
      }
    }
  ],
  "publishNow": true
}
```

If a flair is required but not provided, the API attempts to use the first available option.

## GIF Support

Reddit accepts animated GIFs with these specifications:

- Display as static until clicked
- Maximum 20 MB file size
- May be converted to video format internally
- Keep under 10 MB for optimal performance

## Video Limitations

Reddit's API does not support video uploads for third-party applications. To post videos, upload to a video hosting service and create a link post with the URL.

## Markdown Formatting

Text posts support Markdown:

```
# Heading
**Bold text**
*Italic text*
- Bullet points
1. Numbered lists
[Link text](https://example.com)
> Block quotes
`inline code`
```

## Common Issues

**Post rejected by subreddit:** Check subreddit rules for karma requirements, flair requirements, or image restrictions. Use the flairs endpoint to identify required flair IDs.

**Rate limited:** Reddit enforces strict limits (~10 posts/day for new accounts, higher for established accounts). Space requests appropriately.

**Image not displaying:** Verify file size (≤20 MB), format validity (JPEG, PNG, GIF), and URL accessibility.

**Video not supported:** Reddit's API limitation prevents video uploads. Use link posts with video URLs instead.

**Post removed:** Moderators may remove posts for rule violations, spam detection, missing flairs, or low account karma.

## Inbox Management

*Requires Inbox add-on ($1/social set/month)*

### Direct Messages

| Feature | Support |
|---------|---------|
| List conversations | ✅ |
| Fetch messages | ✅ |
| Send text messages | ✅ |
| Send attachments | ❌ (API limitation) |
| Archive/unarchive | ✅ |

### Comments

| Feature | Support |
|---------|---------|
| List post comments | ✅ |
| Reply to comments | ✅ |
| Delete comments | ✅ |
| Upvote/downvote | ✅ |
| Remove votes | ✅ |

### Limitations

DM attachments are unsupported due to Reddit's API constraints. Comment replies require specifying the subreddit parameter.

## Related API Endpoints

- Connect Reddit Account (OAuth flow)
- Create Post (scheduling and creation)
- Upload Media (image uploads)
- Reddit Search (content discovery)
- Messages and Comments (unified inbox)
