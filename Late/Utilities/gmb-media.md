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

## Photo Categories

| Category          | Description                                                      |
|-------------------|------------------------------------------------------------------|
| `COVER`           | Cover photo displayed prominently on the business listing        |
| `PROFILE`         | Profile photo representing the business                          |
| `LOGO`            | Business logo                                                    |
| `EXTERIOR`        | Photos of the exterior of the business                           |
| `INTERIOR`        | Photos of the interior of the business                           |
| `FOOD_AND_DRINK`  | Photos of food and beverages                                     |
| `MENU`            | Photos of the physical menu                                      |
| `PRODUCT`         | Photos of products                                               |
| `TEAMS`           | Photos of the team and staff                                     |
| `ADDITIONAL`      | Additional photos                                                |

## Error Responses

| Status Code | Description                                                                       |
|-------------|-----------------------------------------------------------------------------------|
| `200`       | Success. Returns the media item(s).                                               |
| `201`       | Created. The media item was successfully uploaded.                                |
| `204`       | No Content. The media item was successfully deleted.                              |
| `400`       | Bad Request. Invalid parameters or unsupported media format.                      |
| `401`       | Unauthorized.                                                                     |
| `403`       | Forbidden.                                                                        |
| `404`       | Not Found.                                                                        |
| `413`       | Payload Too Large. The media file exceeds the maximum allowed size.               |
| `500`       | Internal Server Error.                                                            |

## Notes

- Supported photo formats: JPEG, PNG, GIF, BMP, TIFF, WEBP.
- Supported video formats: MP4, MOV, AVI.
- Maximum photo file size: 5 MB. Maximum video file size: 75 MB.
- The `sourceUrl` must be a publicly accessible HTTPS URL when uploading media.
