# Platform Settings - Reference

## Overview
The `platformSpecificData` object is used within the Posts API to configure platform-specific options for each social media platform. This document details all available settings per platform.

The `platformSpecificData` object is included in the request body when creating or updating a post via `POST /v1/posts` or `PUT /v1/posts/{postId}`.

---

## Twitter / X

Settings specific to Twitter/X posts and threads.

```json
{
  "platformSpecificData": {
    "twitter": {
      "thread": [
        {
          "text": "This is the first tweet in the thread.",
          "media": []
        },
        {
          "text": "This is the second tweet in the thread.",
          "media": [
            {
              "type": "image",
              "url": "https://example.com/image.jpg",
              "altText": "Description of image"
            }
          ]
        },
        {
          "text": "Final tweet in the thread. #thread"
        }
      ]
    }
  }
}
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `thread` | array of objects | Array of tweet objects composing a thread. Each object contains `text` (string) and optional `media` (array). When provided, the top-level `text` field is ignored. |
| `thread[].text` | string | Text content for the individual tweet (max 280 characters) |
| `thread[].media` | array | Optional media attachments for the individual tweet |

---

## Facebook

Settings for Facebook Page posts.

```json
{
  "platformSpecificData": {
    "facebook": {
      "story": true,
      "firstComment": "Follow us for more content!",
      "pageId": "123456789"
    }
  }
}
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `story` | boolean | When `true`, publishes the post as a Facebook Story instead of a feed post. Stories require a single image or video. Default: `false` |
| `firstComment` | string | Text to post as the first comment immediately after publishing. Useful for adding links or hashtags without cluttering the main post. |
| `pageId` | string | The Facebook Page ID to post to. Required when the connected account manages multiple pages. |

---

## Instagram

Settings for Instagram posts, stories, reels, and collaborations.

```json
{
  "platformSpecificData": {
    "instagram": {
      "story": true,
      "collaborators": ["username1", "username2"],
      "userTags": [
        {
          "username": "taggeduser",
          "x": 0.5,
          "y": 0.5
        }
      ],
      "thumbOffset": 5000
    }
  }
}
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `story` | boolean | When `true`, publishes as an Instagram Story. Requires a single image or video. Default: `false` |
| `collaborators` | array of strings | Instagram usernames to invite as collaborators on the post. Maximum 3 collaborators. The collaborators must approve the invitation for their name to appear. |
| `userTags` | array of objects | Tag users in photo posts. Each object requires `username` (string), `x` (number, 0-1), and `y` (number, 0-1) representing the tag position as a fraction of image dimensions. |
| `userTags[].username` | string | Instagram username to tag (without @ prefix) |
| `userTags[].x` | number | Horizontal position of the tag (0.0 = left, 1.0 = right) |
| `userTags[].y` | number | Vertical position of the tag (0.0 = top, 1.0 = bottom) |
| `thumbOffset` | integer | Thumbnail offset in milliseconds for video/reel posts. Determines which frame is used as the cover image. Default: `0` |

---

## LinkedIn

Settings for LinkedIn personal and organization posts.

```json
{
  "platformSpecificData": {
    "linkedin": {
      "organizationUrn": "urn:li:organization:12345678",
      "firstComment": "What are your thoughts? Share below!",
      "disableLinkPreview": true
    }
  }
}
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `organizationUrn` | string | LinkedIn Organization URN to post on behalf of. Format: `urn:li:organization:{id}`. When omitted, posts to the personal profile. |
| `firstComment` | string | Text for an automatic first comment posted immediately after publishing. |
| `disableLinkPreview` | boolean | When `true`, prevents LinkedIn from generating a link preview card for URLs in the post text. Default: `false` |

---

## Reddit

Settings for Reddit submissions.

```json
{
  "platformSpecificData": {
    "reddit": {
      "subreddit": "marketing",
      "title": "Check out our latest marketing strategies",
      "flairId": "abcd1234-ef56-7890"
    }
  }
}
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `subreddit` | string | **Required.** The subreddit name to post to (without `r/` prefix). The connected Reddit account must have posting permissions for this subreddit. |
| `title` | string | **Required.** The submission title. Reddit requires a title for all posts. Maximum 300 characters. |
| `flairId` | string | Optional flair ID to apply to the submission. Flair IDs are subreddit-specific. Use the Reddit API or subreddit settings to find available flair IDs. |

---

## Pinterest

Settings for Pinterest Pins.

```json
{
  "platformSpecificData": {
    "pinterest": {
      "title": "10 Marketing Tips for 2025",
      "boardId": "board-123456",
      "link": "https://example.com/blog/marketing-tips"
    }
  }
}
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `title` | string | The Pin title displayed above the description. Maximum 100 characters. |
| `boardId` | string | **Required.** The Pinterest Board ID to pin to. Must be a board owned by the connected account. |
| `link` | string | Destination URL when users click the Pin. Must be a valid URL. |

---

## YouTube

Settings for YouTube video uploads.

```json
{
  "platformSpecificData": {
    "youtube": {
      "title": "Marketing Strategies for 2025",
      "visibility": "public",
      "madeForKids": false,
      "categoryId": "22"
    }
  }
}
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `title` | string | **Required.** The video title. Maximum 100 characters. |
| `visibility` | string | Video privacy status. Values: `public`, `unlisted`, `private`. Default: `public` |
| `madeForKids` | boolean | **Required.** Indicates whether the video is made for children, as required by COPPA. Default: `false` |
| `categoryId` | string | YouTube video category ID. Common values: `1` (Film & Animation), `2` (Autos & Vehicles), `10` (Music), `15` (Pets & Animals), `17` (Sports), `20` (Gaming), `22` (People & Blogs), `23` (Comedy), `24` (Entertainment), `25` (News & Politics), `26` (Howto & Style), `27` (Education), `28` (Science & Technology). Default: `22` |

---

## TikTok

Settings for TikTok video posts. TikTok requires specific disclosure and interaction settings.

```json
{
  "platformSpecificData": {
    "tiktok": {
      "tiktokSettings": {
        "privacyLevel": "PUBLIC_TO_EVERYONE",
        "disableDuet": false,
        "disableStitch": false,
        "disableComment": false,
        "brandContentToggle": false,
        "brandOrganicToggle": false,
        "videoCoverTimestamp": 1.5,
        "title": "Check this out! #fyp"
      }
    }
  }
}
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `tiktokSettings.privacyLevel` | string | **Required.** Video privacy level. Values: `PUBLIC_TO_EVERYONE`, `MUTUAL_FOLLOW_FRIENDS`, `FOLLOWER_OF_CREATOR`, `SELF_ONLY`. Default: `PUBLIC_TO_EVERYONE` |
| `tiktokSettings.disableDuet` | boolean | Disable duet for this video. Default: `false` |
| `tiktokSettings.disableStitch` | boolean | Disable stitch for this video. Default: `false` |
| `tiktokSettings.disableComment` | boolean | Disable comments for this video. Default: `false` |
| `tiktokSettings.brandContentToggle` | boolean | **Required if applicable.** Must be `true` if the video promotes a third-party brand (paid partnership). When `true`, `brandOrganicToggle` must be `false`. |
| `tiktokSettings.brandOrganicToggle` | boolean | **Required if applicable.** Must be `true` if the video promotes the creator's own brand/business. When `true`, `brandContentToggle` must be `false`. |
| `tiktokSettings.videoCoverTimestamp` | number | Timestamp in seconds for the video cover frame. Default: `0` |
| `tiktokSettings.title` | string | Video description/caption. Hashtags can be included inline. Maximum 2200 characters. |

**Important TikTok Constraints:**
- At least one of `brandContentToggle` or `brandOrganicToggle` must be set if the content is promotional.
- Both `brandContentToggle` and `brandOrganicToggle` cannot be `true` simultaneously.
- Video must be between 1 second and 10 minutes long.
- Supported formats: MP4, WebM.

---

## Google Business

Settings for Google Business Profile posts.

```json
{
  "platformSpecificData": {
    "googleBusiness": {
      "locationId": "locations/12345678901234567890",
      "callToAction": {
        "actionType": "LEARN_MORE",
        "url": "https://example.com/promo"
      }
    }
  }
}
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `locationId` | string | **Required.** The Google Business location identifier. Format: `locations/{locationId}`. |
| `callToAction` | object | Optional call-to-action button displayed on the post. |
| `callToAction.actionType` | string | Button type. Values: `BOOK`, `ORDER`, `SHOP`, `LEARN_MORE`, `SIGN_UP`, `CALL`. |
| `callToAction.url` | string | Destination URL for the CTA button. Required for all action types except `CALL`. |

---

## Telegram

Settings for Telegram bot messages.

```json
{
  "platformSpecificData": {
    "telegram": {
      "parseMode": "HTML",
      "disableNotification": false,
      "protectContent": true
    }
  }
}
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `parseMode` | string | Message formatting mode. Values: `HTML`, `MarkdownV2`, `Markdown`. Default: `HTML`. Determines how the post text is parsed for formatting. |
| `disableNotification` | boolean | When `true`, sends the message silently without triggering a notification for channel subscribers. Default: `false` |
| `protectContent` | boolean | When `true`, prevents message forwarding and saving. Default: `false` |

**Telegram Parse Mode Examples:**
- **HTML:** `<b>bold</b>`, `<i>italic</i>`, `<a href="url">link</a>`, `<code>code</code>`
- **MarkdownV2:** `*bold*`, `_italic_`, `[link](url)`, `` `code` ``

---

## Snapchat

Settings for Snapchat content.

```json
{
  "platformSpecificData": {
    "snapchat": {
      "contentType": "STORY"
    }
  }
}
```

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `contentType` | string | The type of Snapchat content. Values: `STORY`, `SPOTLIGHT`. Default: `STORY` |

---

## Bluesky

Bluesky posts use the top-level `text` and `media` fields. There are currently no Bluesky-specific platform settings required.

```json
{
  "platformSpecificData": {
    "bluesky": {}
  }
}
```

Bluesky posts support:
- Text content up to 300 characters
- Up to 4 images per post
- Link cards are automatically generated from URLs in the text
- Mentions using `@handle.bsky.social` format
- Hashtags using `#tag` format

---

## Notes

- The `platformSpecificData` object is entirely optional. When omitted, default platform behavior applies.
- Platform-specific fields only apply when posting to the corresponding platform. Fields for other platforms are ignored.
- When posting to multiple platforms simultaneously, you can include settings for each platform within the same `platformSpecificData` object.
- Refer to each platform's official documentation for the most current constraints and limitations.
