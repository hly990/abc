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

### Query Parameters

| Parameter   | Type    | Required | Description                                                                        |
|-------------|---------|----------|------------------------------------------------------------------------------------|
| `accountId` | string  | Yes      | The unique identifier of the account                                               |
| `q`         | string  | Yes      | The search query string                                                            |
| `subreddit` | string  | No       | Limit search to a specific subreddit                                               |
| `sort`      | string  | No       | Sort order: `relevance`, `hot`, `top`, `new`, `comments`. Default: `relevance`     |
| `limit`     | integer | No       | Maximum number of results. Default: 25. Max: 100.                                  |
| `after`     | string  | No       | Fullname for pagination (e.g., `t3_abc123`)                                        |

---

## Browse Subreddit Feed

```
GET /v1/reddit/feed
```

### Query Parameters

| Parameter   | Type    | Required | Description                                                                                    |
|-------------|---------|----------|------------------------------------------------------------------------------------------------|
| `accountId` | string  | Yes      | The unique identifier of the account                                                           |
| `subreddit` | string  | Yes      | The subreddit name (without the `r/` prefix)                                                   |
| `sort`      | string  | No       | Sort order: `hot`, `new`, `top`, `rising`, `controversial`. Default: `hot`                     |
| `limit`     | integer | No       | Maximum number of posts. Default: 25. Max: 100.                                                |
| `t`         | string  | No       | Time filter for `top`/`controversial`: `hour`, `day`, `week`, `month`, `year`, `all`           |

## Error Responses

| Status Code | Description                                                                     |
|-------------|---------------------------------------------------------------------------------|
| `200`       | Success. Returns the search results or feed posts.                              |
| `400`       | Bad Request. Missing required parameters.                                       |
| `401`       | Unauthorized.                                                                   |
| `403`       | Forbidden.                                                                      |
| `404`       | Not Found.                                                                      |
| `429`       | Too Many Requests. Reddit API rate limit exceeded.                              |
| `500`       | Internal Server Error.                                                          |

## Notes

- The `t` time filter is only applicable when `sort` is `top` or `controversial`.
- Subreddit names are case-insensitive.
- Reddit rate limits may apply. If you receive a `429` response, wait before retrying.
