# Late API - Platforms Overview

## Summary
Late API supports 13 major social media platforms with unified endpoints for posting, scheduling, analytics, and inbox management. This documentation covers connection methods, content capabilities, and platform-specific features.

## Supported Platforms

| Platform | Auth | Post Types | Analytics | Media |
|----------|------|-----------|-----------|-------|
| Twitter/X | OAuth 2.0 | Text, Images, Videos, Threads | Yes | View |
| Instagram | OAuth 2.0 | Feed, Stories, Reels, Carousels | Yes | View |
| Facebook | OAuth 2.0 | Text, Images, Videos, Reels | Yes | View |
| LinkedIn | OAuth 2.0 | Text, Images, Videos, Documents | Yes | View |
| TikTok | OAuth 2.0 | Videos | Yes | View |
| YouTube | OAuth 2.0 | Videos, Shorts | Yes | View |
| Pinterest | OAuth 2.0 | Pins (Image/Video) | Yes | View |
| Reddit | OAuth 2.0 | Text, Images, Videos, Links | Limited | View |
| Bluesky | App Password | Text, Images, Videos | Limited | View |
| Threads | OAuth 2.0 | Text, Images, Videos | Yes | View |
| Google Business | OAuth 2.0 | Updates, Photos | Yes | View |
| Telegram | Bot Token | Text, Images, Videos, Albums | No | View |
| Snapchat | OAuth 2.0 | Stories, Saved Stories, Spotlight | Yes | View |

## Getting Started

### 1. Connect Account
Replace `{platform}` with platform identifier (twitter, instagram, facebook, linkedin, tiktok, youtube, pinterest, reddit, bluesky, threads, googlebusiness, telegram, snapchat):

```bash
curl "https://getlate.dev/api/v1/connect/{platform}?profileId=YOUR_PROFILE_ID" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 2. Create Post
```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Hello from Late API!",
    "platforms": [
      {"platform": "twitter", "accountId": "YOUR_ACCOUNT_ID"}
    ],
    "publishNow": true
  }'
```

### 3. Cross-Post
```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Cross-posting to all platforms!",
    "platforms": [
      {"platform": "twitter", "accountId": "acc_twitter"},
      {"platform": "linkedin", "accountId": "acc_linkedin"},
      {"platform": "bluesky", "accountId": "acc_bluesky"}
    ],
    "publishNow": true
  }'
```

## Platform-Specific Features

- **Twitter/X**: Threads, polls, scheduled spaces
- **Instagram**: Stories, Reels, Carousels, Collaborators
- **Facebook**: Reels, Stories, Page posts
- **LinkedIn**: PDF documents, Company pages, Personal profiles
- **TikTok**: Privacy settings, duet/stitch controls
- **YouTube**: Shorts, playlists, visibility settings
- **Pinterest**: Boards, Rich pins
- **Reddit**: Subreddits, flairs, NSFW tags
- **Bluesky**: Custom feeds, app passwords
- **Threads**: Reply controls
- **Google Business**: Location posts, offers, events
- **Telegram**: Channels, groups, silent messages, protected content
- **Snapchat**: Stories, Saved Stories, Spotlight, Public Profiles

## Analytics KPIs by Platform

Instagram provides the most comprehensive metrics (impressions, reach, likes, comments, shares, saves, views), while Telegram and Bluesky offer minimal analytics. LinkedIn offers all standard metrics plus views for video posts. TikTok lacks impressions/reach data but tracks engagement.

## Inbox Features (Requires Add-on)

### DM Support
Facebook and Instagram offer complete DM functionality including attachments and quick replies. Twitter/X supports basic messaging. Bluesky and Reddit have limited capabilities.

### Comments Support
Facebook enables posting and liking comments. Instagram and Threads support replies only. YouTube allows full comment management without likes.

### Reviews Support
Facebook and Google Business profiles can reply to and manage customer reviews.

### Account Settings
Configure platform-specific features:
- Facebook: Persistent menu
- Instagram: Ice breakers
- Telegram: Bot commands

## API Reference

**Core endpoints:**
- Profiles, Connect, Accounts, Posts, Platform Settings, Analytics, Messages, Comments, Reviews, Webhooks, Logs

**Management:** Users, Account Groups, API Keys, Invites

**Utilities:** GMB (Reviews, Menus, Location, Media, Attributes, Actions), LinkedIn Mentions, Media Upload, Queue, Reddit Search, Usage

**Tools:** Media Downloads, Transcripts, Hashtag Checker
