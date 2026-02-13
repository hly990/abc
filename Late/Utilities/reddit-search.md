# Reddit Search API

Search for Reddit posts and browse subreddit feeds.

## Endpoints

| Method | Endpoint             | Description                              |
|--------|----------------------|------------------------------------------|
| `GET`  | `/v1/reddit/search`  | Search for Reddit posts by query         |
| `GET`  | `/v1/reddit/feed`    | Browse a subreddit feed                  |

---

## Search Reddit Posts

```
GET /v1/reddit/search
```

Search for Reddit posts matching a query string.

### Query Parameters

| Parameter   | Type    | Required | Description                                                                        |
|-------------|---------|----------|------------------------------------------------------------------------------------|
| `accountId` | string  | Yes      | The unique identifier of the account                                               |
| `q`         | string  | Yes      | The search query string                                                            |
| `subreddit` | string  | No       | Limit search to a specific subreddit (without the `r/` prefix)                     |
| `sort`      | string  | No       | Sort order: `relevance`, `hot`, `top`, `new`, `comments`. Defaults to `relevance`. |
| `limit`     | integer | No       | Maximum number of results to return. Defaults to 25. Max is 100.                   |
| `after`     | string  | No       | Fullname of a post to use as an anchor for pagination (e.g., `t3_abc123`)          |

### Request Example

```bash
curl -X GET \
  "https://api.example.com/v1/reddit/search?accountId=123456&q=social+media+marketing&subreddit=marketing&sort=relevance&limit=10" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Response Example

```json
{
  "posts": [
    {
      "id": "t3_xyz789",
      "subreddit": "marketing",
      "subredditPrefixed": "r/marketing",
      "title": "What social media marketing strategies are working in 2025?",
      "author": "marketingpro42",
      "selfText": "I've been running campaigns across multiple platforms and wanted to share some insights...",
      "url": "https://www.reddit.com/r/marketing/comments/xyz789/what_social_media_marketing_strategies_are/",
      "permalink": "/r/marketing/comments/xyz789/what_social_media_marketing_strategies_are/",
      "score": 342,
      "upvoteRatio": 0.94,
      "numComments": 87,
      "created": "2025-07-15T14:30:00Z",
      "isNsfw": false,
      "isSpoiler": false,
      "flair": "Discussion",
      "thumbnail": "self",
      "mediaUrl": null,
      "linkUrl": null,
      "postType": "self"
    },
    {
      "id": "t3_abc456",
      "subreddit": "marketing",
      "subredditPrefixed": "r/marketing",
      "title": "Case Study: How we grew our social media presence by 400% in 6 months",
      "author": "growthHacker99",
      "selfText": "",
      "url": "https://www.reddit.com/r/marketing/comments/abc456/case_study_how_we_grew/",
      "permalink": "/r/marketing/comments/abc456/case_study_how_we_grew/",
      "score": 1205,
      "upvoteRatio": 0.97,
      "numComments": 203,
      "created": "2025-06-28T09:15:00Z",
      "isNsfw": false,
      "isSpoiler": false,
      "flair": "Case Study",
      "thumbnail": "https://b.thumbs.redditmedia.com/example-thumb.jpg",
      "mediaUrl": "https://i.redd.it/example-image.jpg",
      "linkUrl": null,
      "postType": "image"
    }
  ],
  "after": "t3_abc456",
  "before": null,
  "totalEstimate": 1542
}
```

---

## Browse Subreddit Feed

```
GET /v1/reddit/feed
```

Retrieves posts from a subreddit feed.

### Query Parameters

| Parameter   | Type    | Required | Description                                                                                    |
|-------------|---------|----------|------------------------------------------------------------------------------------------------|
| `accountId` | string  | Yes      | The unique identifier of the account                                                           |
| `subreddit` | string  | Yes      | The subreddit name (without the `r/` prefix)                                                   |
| `sort`      | string  | No       | Sort order: `hot`, `new`, `top`, `rising`, `controversial`. Defaults to `hot`.                 |
| `limit`     | integer | No       | Maximum number of posts to return. Defaults to 25. Max is 100.                                 |
| `after`     | string  | No       | Fullname of a post to use as an anchor for pagination (e.g., `t3_abc123`)                      |
| `t`         | string  | No       | Time filter for `top` and `controversial` sorts: `hour`, `day`, `week`, `month`, `year`, `all`. Defaults to `day`. |

### Request Example

```bash
curl -X GET \
  "https://api.example.com/v1/reddit/feed?accountId=123456&subreddit=technology&sort=top&limit=5&t=week" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Response Example

```json
{
  "subreddit": "technology",
  "subredditPrefixed": "r/technology",
  "sort": "top",
  "timeFilter": "week",
  "posts": [
    {
      "id": "t3_def001",
      "subreddit": "technology",
      "subredditPrefixed": "r/technology",
      "title": "New breakthrough in quantum computing achieves 1000-qubit milestone",
      "author": "techReporter",
      "selfText": "",
      "url": "https://www.reddit.com/r/technology/comments/def001/new_breakthrough_in_quantum/",
      "permalink": "/r/technology/comments/def001/new_breakthrough_in_quantum/",
      "score": 45230,
      "upvoteRatio": 0.96,
      "numComments": 2841,
      "created": "2025-07-29T16:00:00Z",
      "isNsfw": false,
      "isSpoiler": false,
      "flair": "Hardware",
      "thumbnail": "https://b.thumbs.redditmedia.com/quantum-thumb.jpg",
      "mediaUrl": null,
      "linkUrl": "https://techpublication.com/quantum-computing-breakthrough",
      "postType": "link"
    },
    {
      "id": "t3_def002",
      "subreddit": "technology",
      "subredditPrefixed": "r/technology",
      "title": "Open source AI model outperforms proprietary alternatives in new benchmark",
      "author": "openSourceFan",
      "selfText": "A new open source model has just been released that shows remarkable performance...",
      "url": "https://www.reddit.com/r/technology/comments/def002/open_source_ai_model/",
      "permalink": "/r/technology/comments/def002/open_source_ai_model/",
      "score": 32150,
      "upvoteRatio": 0.93,
      "numComments": 1567,
      "created": "2025-07-28T11:30:00Z",
      "isNsfw": false,
      "isSpoiler": false,
      "flair": "AI",
      "thumbnail": "self",
      "mediaUrl": null,
      "linkUrl": null,
      "postType": "self"
    },
    {
      "id": "t3_def003",
      "subreddit": "technology",
      "subredditPrefixed": "r/technology",
      "title": "Video: Behind the scenes at the world's largest data center",
      "author": "datacenterGeek",
      "selfText": "",
      "url": "https://www.reddit.com/r/technology/comments/def003/video_behind_the_scenes/",
      "permalink": "/r/technology/comments/def003/video_behind_the_scenes/",
      "score": 18900,
      "upvoteRatio": 0.91,
      "numComments": 743,
      "created": "2025-07-27T20:45:00Z",
      "isNsfw": false,
      "isSpoiler": false,
      "flair": "Infrastructure",
      "thumbnail": "https://b.thumbs.redditmedia.com/datacenter-thumb.jpg",
      "mediaUrl": "https://v.redd.it/example-video",
      "linkUrl": null,
      "postType": "video"
    }
  ],
  "after": "t3_def003",
  "before": null
}
```

---

## Post Object Fields

| Field               | Type    | Description                                                           |
|---------------------|---------|-----------------------------------------------------------------------|
| `id`                | string  | The fullname identifier of the post (e.g., `t3_abc123`)              |
| `subreddit`         | string  | The subreddit name (without prefix)                                   |
| `subredditPrefixed` | string  | The subreddit name with `r/` prefix                                   |
| `title`             | string  | The title of the post                                                 |
| `author`            | string  | The username of the post author                                       |
| `selfText`          | string  | The body text of a self post. Empty string for link/image/video posts.|
| `url`               | string  | The full Reddit URL of the post                                       |
| `permalink`         | string  | The relative permalink of the post                                    |
| `score`             | integer | The net score (upvotes minus downvotes)                               |
| `upvoteRatio`       | number  | The ratio of upvotes to total votes (0.0 to 1.0)                     |
| `numComments`       | integer | The total number of comments on the post                              |
| `created`           | string  | ISO 8601 timestamp when the post was created                          |
| `isNsfw`            | boolean | Whether the post is marked as NSFW                                    |
| `isSpoiler`         | boolean | Whether the post is marked as a spoiler                               |
| `flair`             | string  | The post flair text, or `null` if no flair is set                     |
| `thumbnail`         | string  | Thumbnail URL, `"self"` for text posts, or `"default"` if none       |
| `mediaUrl`          | string  | Direct URL to the media (image/video), or `null`                     |
| `linkUrl`           | string  | External link URL for link posts, or `null`                          |
| `postType`          | string  | The type of post: `self`, `link`, `image`, `video`, `gallery`        |

## Pagination

Both endpoints support cursor-based pagination using the `after` parameter:

1. Make an initial request without the `after` parameter.
2. Use the `after` value from the response as the `after` query parameter in the next request.
3. Continue until `after` is `null`, indicating no more results.

```bash
# Page 1
curl -X GET \
  "https://api.example.com/v1/reddit/search?accountId=123456&q=marketing&limit=25" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"

# Page 2 (using 'after' from page 1 response)
curl -X GET \
  "https://api.example.com/v1/reddit/search?accountId=123456&q=marketing&limit=25&after=t3_abc456" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Error Responses

| Status Code | Description                                                                     |
|-------------|---------------------------------------------------------------------------------|
| `200`       | Success. Returns the search results or feed posts.                              |
| `400`       | Bad Request. Missing required parameters, invalid sort value, or invalid limit. |
| `401`       | Unauthorized. Missing or invalid authentication token.                          |
| `403`       | Forbidden. The authenticated user does not have access to this account.         |
| `404`       | Not Found. The specified `accountId` or subreddit does not exist.               |
| `429`       | Too Many Requests. Reddit API rate limit exceeded.                              |
| `500`       | Internal Server Error. An unexpected error occurred on the server.              |

## Notes

- The `t` (time filter) parameter is only applicable when `sort` is `top` or `controversial`. It is ignored for other sort values.
- Subreddit names are case-insensitive.
- The `totalEstimate` field in search results is an approximate count and may not be exact.
- NSFW posts are included by default. Filter on the `isNsfw` field client-side if you wish to exclude them.
- The `after` cursor value is the fullname of the last post in the current page (e.g., `t3_abc123`).
- Reddit rate limits may apply. If you receive a `429` response, wait before retrying.
