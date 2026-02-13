# GMB Media API

Manage Google My Business media (photos and videos) for a given account.

## Endpoints

### List Media

```
GET /v1/accounts/{accountId}/gmb-media
```

Retrieves all media items for the specified account.

### Upload Media

```
POST /v1/accounts/{accountId}/gmb-media
```

Uploads a new media item to the specified account.

### Delete Media

```
DELETE /v1/accounts/{accountId}/gmb-media/{mediaId}
```

Deletes a specific media item from the specified account.

## Path Parameters

| Parameter   | Type   | Required | Description                                      |
|-------------|--------|----------|--------------------------------------------------|
| `accountId` | string | Yes      | The unique identifier of the account              |
| `mediaId`   | string | Yes      | The unique identifier of the media item (DELETE only) |

## Query Parameters (GET)

| Parameter   | Type    | Required | Description                                                            |
|-------------|---------|----------|------------------------------------------------------------------------|
| `pageSize`  | integer | No       | Maximum number of media items to return per page. Defaults to 50.      |
| `pageToken` | string  | No       | Token for retrieving the next page of results from a previous response.|
| `category`  | string  | No       | Filter media by category. See photo categories below.                  |

## Photo Categories

| Category          | Description                                                      |
|-------------------|------------------------------------------------------------------|
| `COVER`           | Cover photo displayed prominently on the business listing        |
| `PROFILE`         | Profile photo representing the business                          |
| `LOGO`            | Business logo                                                    |
| `EXTERIOR`        | Photos of the exterior of the business                           |
| `INTERIOR`        | Photos of the interior of the business                           |
| `FOOD_AND_DRINK`  | Photos of food and beverages served by the business              |
| `MENU`            | Photos of the physical menu                                      |
| `PRODUCT`         | Photos of products offered by the business                       |
| `TEAMS`           | Photos of the team and staff                                     |
| `ADDITIONAL`      | Additional photos that do not fit into any other category        |

## Request Body (POST - Upload)

| Parameter    | Type   | Required | Description                                                |
|--------------|--------|----------|------------------------------------------------------------|
| `sourceUrl`  | string | Yes      | The publicly accessible URL of the media to upload         |
| `description`| string | No       | A description of the media item. Max 1000 characters.      |
| `category`   | string | Yes      | The category for the media. Must be a valid photo category.|

### Upload Request Example

```bash
curl -X POST \
  "https://api.example.com/v1/accounts/123456/gmb-media" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "sourceUrl": "https://cdn.example.com/photos/restaurant-exterior.jpg",
    "description": "Front entrance of our restaurant with outdoor patio seating",
    "category": "EXTERIOR"
  }'
```

## Response Examples

### List Media Response (GET)

```json
{
  "mediaItems": [
    {
      "mediaId": "media-abc-001",
      "sourceUrl": "https://cdn.example.com/photos/restaurant-exterior.jpg",
      "thumbnailUrl": "https://cdn.example.com/thumbnails/restaurant-exterior-thumb.jpg",
      "description": "Front entrance of our restaurant with outdoor patio seating",
      "category": "EXTERIOR",
      "mediaFormat": "PHOTO",
      "dimensions": {
        "widthPixels": 1920,
        "heightPixels": 1080
      },
      "sizeBytes": 2458624,
      "createTime": "2025-06-15T10:30:00Z",
      "insights": {
        "viewCount": 1542
      }
    },
    {
      "mediaId": "media-abc-002",
      "sourceUrl": "https://cdn.example.com/photos/restaurant-interior.jpg",
      "thumbnailUrl": "https://cdn.example.com/thumbnails/restaurant-interior-thumb.jpg",
      "description": "Dining area with warm lighting and rustic decor",
      "category": "INTERIOR",
      "mediaFormat": "PHOTO",
      "dimensions": {
        "widthPixels": 2400,
        "heightPixels": 1600
      },
      "sizeBytes": 3145728,
      "createTime": "2025-06-10T14:15:00Z",
      "insights": {
        "viewCount": 987
      }
    },
    {
      "mediaId": "media-abc-003",
      "sourceUrl": "https://cdn.example.com/photos/logo.png",
      "thumbnailUrl": "https://cdn.example.com/thumbnails/logo-thumb.png",
      "description": "Official business logo",
      "category": "LOGO",
      "mediaFormat": "PHOTO",
      "dimensions": {
        "widthPixels": 512,
        "heightPixels": 512
      },
      "sizeBytes": 102400,
      "createTime": "2025-01-05T08:00:00Z",
      "insights": {
        "viewCount": 5230
      }
    },
    {
      "mediaId": "media-abc-004",
      "sourceUrl": "https://cdn.example.com/videos/kitchen-tour.mp4",
      "thumbnailUrl": "https://cdn.example.com/thumbnails/kitchen-tour-thumb.jpg",
      "description": "Behind the scenes kitchen tour with our head chef",
      "category": "INTERIOR",
      "mediaFormat": "VIDEO",
      "dimensions": {
        "widthPixels": 1920,
        "heightPixels": 1080
      },
      "sizeBytes": 52428800,
      "createTime": "2025-07-20T12:00:00Z",
      "insights": {
        "viewCount": 2103
      }
    }
  ],
  "nextPageToken": "eyJwYWdlIjoyLCJsaW1pdCI6NTB9",
  "totalMediaCount": 24
}
```

### Upload Media Response (POST)

```json
{
  "mediaId": "media-abc-005",
  "sourceUrl": "https://cdn.example.com/photos/restaurant-exterior.jpg",
  "thumbnailUrl": "https://cdn.example.com/thumbnails/restaurant-exterior-thumb.jpg",
  "description": "Front entrance of our restaurant with outdoor patio seating",
  "category": "EXTERIOR",
  "mediaFormat": "PHOTO",
  "dimensions": {
    "widthPixels": 1920,
    "heightPixels": 1080
  },
  "sizeBytes": 2458624,
  "createTime": "2025-08-01T09:45:00Z",
  "insights": {
    "viewCount": 0
  }
}
```

### Delete Media Response (DELETE)

```bash
curl -X DELETE \
  "https://api.example.com/v1/accounts/123456/gmb-media/media-abc-005" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

Response:
```json
{
  "success": true,
  "message": "Media item media-abc-005 has been deleted."
}
```

## Response Fields

| Field                       | Type    | Description                                        |
|-----------------------------|---------|----------------------------------------------------|
| `mediaItems`                | array   | Array of media item objects (GET only)              |
| `mediaItems[].mediaId`      | string  | Unique identifier of the media item                 |
| `mediaItems[].sourceUrl`    | string  | The source URL of the media                         |
| `mediaItems[].thumbnailUrl` | string  | URL of the generated thumbnail                      |
| `mediaItems[].description`  | string  | Description of the media item                       |
| `mediaItems[].category`     | string  | The photo category of the media item                |
| `mediaItems[].mediaFormat`  | string  | Format type: `PHOTO` or `VIDEO`                     |
| `mediaItems[].dimensions`   | object  | Width and height in pixels                          |
| `mediaItems[].sizeBytes`    | integer | File size in bytes                                  |
| `mediaItems[].createTime`   | string  | ISO 8601 timestamp when the media was uploaded      |
| `mediaItems[].insights`     | object  | Insights data for the media item                    |
| `mediaItems[].insights.viewCount` | integer | Total number of views                         |
| `nextPageToken`             | string  | Token for the next page of results                  |
| `totalMediaCount`           | integer | Total number of media items for the account         |

## Error Responses

| Status Code | Description                                                                       |
|-------------|-----------------------------------------------------------------------------------|
| `200`       | Success. Returns the media item(s).                                               |
| `201`       | Created. The media item was successfully uploaded (POST).                          |
| `204`       | No Content. The media item was successfully deleted (DELETE).                      |
| `400`       | Bad Request. Invalid parameters, unsupported media format, or invalid `sourceUrl`. |
| `401`       | Unauthorized. Missing or invalid authentication token.                            |
| `403`       | Forbidden. The authenticated user does not have access to this account.           |
| `404`       | Not Found. The specified `accountId` or `mediaId` does not exist.                 |
| `413`       | Payload Too Large. The media file exceeds the maximum allowed size.               |
| `415`       | Unsupported Media Type. The file format is not supported.                         |
| `500`       | Internal Server Error. An unexpected error occurred on the server.                |

## Notes

- Supported photo formats: JPEG, PNG, GIF, BMP, TIFF, WEBP.
- Supported video formats: MP4, MOV, AVI.
- Maximum photo file size: 5 MB.
- Maximum video file size: 75 MB. Video duration must not exceed 30 seconds.
- Minimum photo dimensions: 250 x 250 pixels.
- Recommended photo dimensions: 720 x 720 pixels or larger.
- Each account can have a maximum of 1 `COVER`, 1 `PROFILE`, and 1 `LOGO` photo. Uploading a new one in these categories replaces the existing one.
- Categories `EXTERIOR`, `INTERIOR`, `FOOD_AND_DRINK`, `MENU`, `PRODUCT`, `TEAMS`, and `ADDITIONAL` support multiple media items.
- The `insights.viewCount` is updated periodically and may not reflect real-time data.
- The `sourceUrl` must be a publicly accessible HTTPS URL when uploading media.