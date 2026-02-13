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
| `thread` | array of objects | Array of tweet objects composing a thread |
| `thread[].text` | string | Text content for the individual tweet (max 280 characters) |
| `thread[].media` | array | Optional media attachments for the individual tweet |

---

## Facebook

Settings for Facebook Page posts.

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `story` | boolean | When `true`, publishes as a Facebook Story. Default: `false` |
| `firstComment` | string | Text to post as the first comment |
| `pageId` | string | The Facebook Page ID to post to |

---

## Instagram

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `story` | boolean | When `true`, publishes as an Instagram Story. Default: `false` |
| `collaborators` | array of strings | Instagram usernames to invite as collaborators (max 3) |
| `userTags` | array of objects | Tag users in photo posts |
| `thumbOffset` | integer | Thumbnail offset in milliseconds for video/reel posts |

---

## LinkedIn

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `organizationUrn` | string | LinkedIn Organization URN |
| `firstComment` | string | Text for an automatic first comment |
| `disableLinkPreview` | boolean | Prevents LinkedIn from generating a link preview card |

---

## Reddit

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `subreddit` | string | **Required.** The subreddit name to post to |
| `title` | string | **Required.** The submission title (max 300 characters) |
| `flairId` | string | Optional flair ID to apply |

---

## Pinterest

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `title` | string | The Pin title (max 100 characters) |
| `boardId` | string | **Required.** The Pinterest Board ID |
| `link` | string | Destination URL when users click the Pin |

---

## YouTube

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `title` | string | **Required.** The video title (max 100 characters) |
| `visibility` | string | `public`, `unlisted`, `private`. Default: `public` |
| `madeForKids` | boolean | **Required.** COPPA compliance flag |
| `categoryId` | string | YouTube video category ID |

---

## TikTok

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `tiktokSettings.privacyLevel` | string | **Required.** Video privacy level |
| `tiktokSettings.disableDuet` | boolean | Disable duet |
| `tiktokSettings.disableStitch` | boolean | Disable stitch |
| `tiktokSettings.disableComment` | boolean | Disable comments |
| `tiktokSettings.brandContentToggle` | boolean | Paid partnership flag |
| `tiktokSettings.brandOrganicToggle` | boolean | Own brand promotion flag |
| `tiktokSettings.videoCoverTimestamp` | number | Cover frame timestamp in seconds |
| `tiktokSettings.title` | string | Video description/caption (max 2200 characters) |

---

## Google Business

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `locationId` | string | **Required.** Google Business location identifier |
| `callToAction` | object | Optional call-to-action button |
| `callToAction.actionType` | string | `BOOK`, `ORDER`, `SHOP`, `LEARN_MORE`, `SIGN_UP`, `CALL` |
| `callToAction.url` | string | Destination URL for the CTA button |

---

## Telegram

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `parseMode` | string | `HTML`, `MarkdownV2`, `Markdown`. Default: `HTML` |
| `disableNotification` | boolean | Send silently. Default: `false` |
| `protectContent` | boolean | Prevent forwarding/saving. Default: `false` |

---

## Snapchat

**Fields:**
| Field | Type | Description |
|-------|------|-------------|
| `contentType` | string | `STORY` or `SPOTLIGHT`. Default: `STORY` |

---

## Bluesky

Bluesky posts use the top-level `text` and `media` fields. No Bluesky-specific platform settings required.

---

## Notes

- The `platformSpecificData` object is entirely optional. When omitted, default platform behavior applies.
- Platform-specific fields only apply when posting to the corresponding platform.
