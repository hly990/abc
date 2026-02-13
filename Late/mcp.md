# Social Media MCP - Late API Documentation

## Overview
The Late Social Media MCP (Model Context Protocol) server enables Claude Desktop to manage your social media accounts directly through natural conversation. With the MCP integration, you can create posts, manage profiles, upload media, and publish content across 13 platforms without leaving Claude Desktop.

## Prerequisites
- A Late API key (available at [getlate.dev](https://getlate.dev))
- Claude Desktop installed
- `uv` package manager installed

## Setup Instructions

### Step 1: Install uv

If you do not already have `uv` installed, install it via the official installer:

**macOS / Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows:**
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### Step 2: Get Your Late API Key

1. Log in to your Late dashboard at [getlate.dev](https://getlate.dev).
2. Navigate to **Settings > API Keys**.
3. Click **Generate New Key** and copy the key.

### Step 3: Configure Claude Desktop

Open your Claude Desktop configuration file and add the Late MCP server.

**Configuration file location:**
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

Add the following to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "late-social": {
      "command": "uvx",
      "args": ["late-mcp-server"],
      "env": {
        "LATE_API_KEY": "YOUR_API_KEY"
      }
    }
  }
}
```

### Step 4: Restart Claude Desktop

After saving the configuration, restart Claude Desktop. You should see the Late tools appear in the tools menu (hammer icon).

---

## Available Commands

| Command | Description |
|---------|-------------|
| `accounts_list` | List all connected social media accounts |
| `profiles_list` | List all profiles |
| `profiles_create` | Create a new profile |
| `profiles_update` | Update an existing profile |
| `profiles_delete` | Delete a profile |
| `posts_list` | List posts with optional filters |
| `posts_create` | Create a new draft post |
| `posts_publish_now` | Publish a post immediately |
| `posts_cross_post` | Cross-post content to multiple platforms |
| `posts_update` | Update a draft or scheduled post |
| `posts_delete` | Delete a post |
| `posts_retry` | Retry a failed post |
| `media_generate_upload_link` | Generate a presigned URL for media upload |
| `media_check_upload_status` | Check the processing status of an uploaded file |

---

## Core Tools Reference

### Accounts

#### accounts_list
Lists all connected social media accounts across all profiles.

**Parameters**: None

**Returns**: Array of account objects with `id`, `platform`, `username`, `profileId`, and `status`.

---

### Profiles

#### profiles_list
Retrieve all profiles in your workspace.

**Parameters**: None

#### profiles_create
Create a new profile to group social accounts.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Display name for the profile |
| `description` | string | No | Optional description |

#### profiles_update
Update an existing profile's name or description.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `profileId` | string | Yes | The profile ID to update |
| `name` | string | No | New display name |
| `description` | string | No | New description |

#### profiles_delete
Permanently delete a profile and disconnect its accounts.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `profileId` | string | Yes | The profile ID to delete |

---

### Posts

#### posts_list
List posts with optional filtering by status, platform, or date range.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `profileId` | string | No | Filter by profile |
| `status` | string | No | Filter by status: `draft`, `scheduled`, `published`, `failed` |
| `platform` | string | No | Filter by platform |
| `limit` | number | No | Number of results (default: 20, max: 100) |
| `offset` | number | No | Pagination offset |

#### posts_create
Create a new post in draft mode.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `content` | string | Yes | Post text content |
| `platforms` | array | Yes | Target platforms (e.g., `["twitter", "instagram"]`) |
| `profileId` | string | Yes | Profile to post from |
| `media` | array | No | Media attachments `[{ url: "..." }]` |
| `scheduledAt` | string | No | ISO 8601 datetime for scheduled publishing |

#### posts_publish_now
Immediately publish a draft post.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `postId` | string | Yes | The post ID to publish |

#### posts_cross_post
Publish the same content across multiple platforms simultaneously.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `content` | string | Yes | Post text content |
| `platforms` | array | Yes | Target platforms |
| `profileId` | string | Yes | Profile to post from |
| `media` | array | No | Media attachments |

#### posts_update
Update a draft or scheduled post before it is published.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `postId` | string | Yes | The post ID to update |
| `content` | string | No | Updated text content |
| `platforms` | array | No | Updated target platforms |
| `scheduledAt` | string | No | Updated schedule time |
| `media` | array | No | Updated media attachments |

#### posts_delete
Delete a draft, scheduled, or published post.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `postId` | string | Yes | The post ID to delete |

#### posts_retry
Retry publishing a post that previously failed.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `postId` | string | Yes | The failed post ID to retry |

---

### Media

#### media_generate_upload_link
Generate a presigned URL for uploading media files.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fileName` | string | Yes | Name of the file to upload |
| `contentType` | string | Yes | MIME type (e.g., `image/jpeg`, `video/mp4`) |

**Returns**: `{ uploadUrl: "...", mediaUrl: "..." }`

#### media_check_upload_status
Check whether a previously uploaded media file has finished processing.

**Parameters**:
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mediaUrl` | string | Yes | The media URL returned from the upload link |

**Returns**: `{ status: "processing|ready|failed", url: "..." }`

---

## Post Creation Modes

The MCP supports three modes for creating posts:

### Draft Mode
Create a post without publishing. Review and publish later.

> "Create a draft post for Twitter saying 'Exciting news coming next week!' on my brand profile."

### Immediate Mode
Create and publish a post right away.

> "Publish a post to Instagram and Twitter right now: 'Our summer collection is live! Shop now at example.com'"

### Scheduled Mode
Create a post scheduled for a future date and time.

> "Schedule a LinkedIn post for next Monday at 9 AM EST: 'We are hiring! Check out our open positions.'"

---

## Media Upload Flow

Uploading media follows a three-step process:

### Step 1: Generate Upload Link
Ask Claude to generate an upload link for your file.

> "Generate an upload link for a JPEG image called product-hero.jpg."

The MCP calls `media_generate_upload_link` and returns a presigned URL.

### Step 2: Upload the File
Upload your file to the presigned URL using any HTTP client.

```bash
curl -X PUT "PRESIGNED_UPLOAD_URL" \
  -H "Content-Type: image/jpeg" \
  --data-binary @product-hero.jpg
```

### Step 3: Use in a Post
Reference the returned media URL when creating your post.

> "Create a Twitter post with the image I just uploaded saying 'New product reveal!'"

---

## Example Conversations

### Example 1: Creating and Publishing a Post

**You**: Create a post for my brand profile on Twitter and LinkedIn saying "We just launched v2.0 of our API! Check out the new features at docs.example.com"

**Claude**: I'll create that post for you on Twitter and LinkedIn.
*(Uses `posts_create` with the content and platforms)*
Post created as a draft. Would you like me to publish it now or schedule it for later?

**You**: Publish it now.

**Claude**: *(Uses `posts_publish_now`)*
Done! Your post has been published to Twitter and LinkedIn.

### Example 2: Checking Connected Accounts

**You**: What social media accounts do I have connected?

**Claude**: *(Uses `accounts_list`)*
You have the following accounts connected:
- Twitter: @yourbrand (Brand Profile)
- Instagram: @yourbrand (Brand Profile)
- LinkedIn: Your Company (Brand Profile)
- Facebook: Your Brand Page (Brand Profile)

### Example 3: Scheduling a Week of Content

**You**: Schedule these posts for my brand profile on Instagram:
- Monday 9 AM: "Monday motivation! Start your week strong."
- Wednesday 12 PM: "Mid-week tip: Always test your API integrations."
- Friday 5 PM: "Happy Friday! What did you ship this week?"

**Claude**: *(Uses `posts_create` three times with appropriate `scheduledAt` values)*
All three posts have been scheduled:
1. Monday at 9:00 AM EST - "Monday motivation..."
2. Wednesday at 12:00 PM EST - "Mid-week tip..."
3. Friday at 5:00 PM EST - "Happy Friday..."

---

## Troubleshooting

### MCP server not appearing in Claude Desktop
- Verify your `claude_desktop_config.json` syntax is valid JSON.
- Ensure `uv` is installed and accessible from your system PATH.
- Restart Claude Desktop after saving the configuration.

### "Invalid API key" errors
- Double-check that the `LATE_API_KEY` in your config matches your key from the Late dashboard.
- Ensure the key has not been revoked or expired.

### Tools not responding
- Check your internet connection.
- Verify the Late API status at [status.getlate.dev](https://status.getlate.dev).
- Try restarting Claude Desktop.

### Posts failing to publish
- Confirm the target social account is still connected and authorized via `accounts_list`.
- Check that your content meets platform-specific requirements (character limits, media formats).
- Use `posts_retry` to attempt publishing again after resolving the issue.

### Media upload issues
- Ensure the `contentType` matches the actual file type.
- Files must be under the platform-specific size limits.
- Use `media_check_upload_status` to verify the file has finished processing before including it in a post.
