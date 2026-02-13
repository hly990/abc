# Telegram API Documentation

## Overview

The Telegram API enables posting to channels and groups with support for text, images, videos, and media albums. Posts can be scheduled and sent immediately.

## Quick Start

**Basic Post Request:**
```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Hello from Late API!",
    "platforms": [
      {"platform": "telegram", "accountId": "YOUR_ACCOUNT_ID"}
    ],
    "publishNow": true
  }'
```

## Connection Methods

### Option 1: Access Code Flow (Recommended)

**Step 1 - Generate Access Code:**
```bash
curl -X GET "https://getlate.dev/api/v1/connect/telegram?profileId=YOUR_PROFILE_ID" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

**Response includes:**
- Unique access code (15-minute expiration)
- Bot username to message (@LateScheduleBot)
- Setup instructions

**Step 2 - Add Bot to Channel/Group:**
- Add @LateScheduleBot as administrator
- Grant "Post Messages" permission for channels
- Make administrator for groups

**Step 3 - Send Access Code:**
- Open private chat with @LateScheduleBot
- Send: `LATE-ABC123 @yourchannel`

**Step 4 - Poll Connection Status:**
```bash
curl -X PATCH "https://getlate.dev/api/v1/connect/telegram?code=LATE-ABC123" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Option 2: Direct Connection

For users with known chat IDs:
```bash
curl -X POST https://getlate.dev/api/v1/connect/telegram \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "profileId": "YOUR_PROFILE_ID",
    "chatId": "-1001234567890"
  }'
```

## Capabilities

| Feature | Support | Details |
|---------|---------|---------|
| Text Posts | ✅ | Up to 4,096 characters |
| Media Captions | ✅ | Up to 1,024 characters |
| Images | ✅ | 10 per album max |
| Videos | ✅ | 10 per album max |
| Mixed Media | ✅ | Images and videos together |
| Scheduling | ✅ | Yes |
| Analytics | ❌ | Platform limitation |

## Media Requirements

**Images:**
- Max 10 per album
- Formats: JPEG, PNG, GIF, WebP
- Max size: 10 MB
- Max resolution: 10000 × 10000 px

**Videos:**
- Max 10 per album
- Formats: MP4, MOV
- Max size: 50 MB
- H.264 codec recommended

## Platform-Specific Options

```json
{
  "content": "<b>Important Update!</b>",
  "platforms": [{
    "platform": "telegram",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {
      "parseMode": "HTML",
      "disableWebPagePreview": false,
      "disableNotification": false,
      "protectContent": false
    }
  }],
  "publishNow": true
}
```

**Available Options:**
- `parseMode`: HTML, Markdown, or MarkdownV2 (default: HTML)
- `disableWebPagePreview`: Prevent link previews
- `disableNotification`: Silent delivery
- `protectContent`: Prevent forwarding/saving

## Text Formatting

**HTML Mode (Default):**
```html
<b>bold</b>
<i>italic</i>
<u>underline</u>
<s>strikethrough</s>
<a href="https://example.com">link</a>
<code>inline code</code>
<pre>code block</pre>
```

**Markdown Mode:**
```markdown
*bold*
_italic_
[link](https://example.com)
`inline code`
```

**MarkdownV2 Mode:**
```markdown
*bold*
_italic_
__underline__
~strikethrough~
||spoiler||
```

## Posting with Media

**Single Image:**
```json
{
  "content": "Check out this photo!",
  "mediaItems": [
    {"type": "image", "url": "https://example.com/image.jpg"}
  ],
  "platforms": [{"platform": "telegram", "accountId": "YOUR_ACCOUNT_ID"}],
  "publishNow": true
}
```

**Media Album:**
```json
{
  "content": "Gallery!",
  "mediaItems": [
    {"type": "image", "url": "https://example.com/image1.jpg"},
    {"type": "image", "url": "https://example.com/image2.jpg"},
    {"type": "video", "url": "https://example.com/video.mp4"}
  ],
  "platforms": [{"platform": "telegram", "accountId": "YOUR_ACCOUNT_ID"}],
  "publishNow": true
}
```

## Channel vs Group Posts

- **Channels**: Post displays channel name and logo
- **Groups**: Post shows as from LateScheduleBot

## Character Limits

| Content Type | Limit |
|-------------|-------|
| Text-only messages | 4,096 characters |
| Media captions | 1,024 characters |

## Special Features

**Silent Messages:**
```json
{
  "content": "Late night update",
  "platforms": [{
    "platform": "telegram",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {"disableNotification": true}
  }]
}
```

**Protected Content:**
```json
{
  "content": "Exclusive content",
  "platforms": [{
    "platform": "telegram",
    "accountId": "YOUR_ACCOUNT_ID",
    "platformSpecificData": {"protectContent": true}
  }]
}
```

## Analytics Limitation

Telegram analytics are not available via API. The Telegram Bot API does not expose message analytics (views, forwards, reactions).

View counts are visible only to channel administrators in the Telegram app. Message IDs are returned for tracking purposes.

## Inbox Management

Requires Inbox add-on ($1/social set/month):

| Feature | Support |
|---------|---------|
| List conversations | ✅ |
| Fetch messages | ✅ |
| Send text | ✅ |
| Send attachments | ✅ |
| Edit messages | ✅ |
| Inline keyboards | ✅ |
| Reply keyboards | ✅ |
| Archive/unarchive | ✅ |

**Attachment Support:**
- Images: 10 MB max
- Videos: 50 MB max
- Documents: 50 MB max

## Common Issues

| Error | Resolution |
|-------|-----------|
| Bot not a member | Add @LateScheduleBot as administrator |
| Message too long | Split content or reduce to limits |
| Invalid file identifier | Verify publicly accessible HTTPS URLs |
| Parse error | Check HTML/Markdown syntax |
| Media not displaying | Confirm format and file size |
| Expired access code | Generate new code (15-minute window) |

## Related Endpoints

- Connect Telegram Account
- Create Post
- Upload Media
- Messages API
- Account Settings (bot commands)
