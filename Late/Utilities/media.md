# Media API

Upload media files using presigned URLs. Supports files up to 5 GB in size.

## Endpoint

```
POST /v1/media/presign
```

Generates a presigned URL for uploading a media file directly to cloud storage.

## Request Body

| Field         | Type   | Required | Description                                                             |
|---------------|--------|----------|-------------------------------------------------------------------------|
| `fileName`    | string | Yes      | The name of the file to upload, including the file extension.           |
| `contentType` | string | Yes      | The MIME type of the file. Must be a supported content type.            |
| `fileSize`    | integer| No       | The size of the file in bytes. Used for validation against the 5 GB limit. |

## Supported Content Types

### Images

| MIME Type         | Extension | Description         |
|-------------------|-----------|---------------------|
| `image/jpeg`      | .jpg/.jpeg| JPEG image          |
| `image/png`       | .png      | PNG image           |
| `image/gif`       | .gif      | GIF image           |
| `image/webp`      | .webp     | WebP image          |
| `image/svg+xml`   | .svg      | SVG image           |
| `image/bmp`       | .bmp      | BMP image           |
| `image/tiff`      | .tiff     | TIFF image          |

### Videos

| MIME Type         | Extension | Description         |
|-------------------|-----------|---------------------|
| `video/mp4`       | .mp4      | MP4 video           |
| `video/quicktime` | .mov      | QuickTime video     |
| `video/x-msvideo` | .avi      | AVI video           |
| `video/webm`      | .webm     | WebM video          |
| `video/mpeg`      | .mpeg     | MPEG video          |

### Documents

| MIME Type           | Extension | Description       |
|---------------------|-----------|-------------------|
| `application/pdf`   | .pdf      | PDF document      |

## Response Fields

| Field       | Type   | Description                                                                       |
|-------------|--------|-----------------------------------------------------------------------------------|
| `uploadUrl` | string | The presigned URL to which the file should be uploaded via HTTP PUT.               |
| `publicUrl` | string | The public URL where the file will be accessible after the upload is complete.     |
| `key`       | string | The unique storage key assigned to the uploaded file.                              |
| `type`      | string | The detected type category: `image`, `video`, or `document`.                      |
| `expiresIn` | integer| The number of seconds until the presigned `uploadUrl` expires. Typically 3600 (1 hour). |

## 3-Step Upload Flow

Uploading media follows a three-step process:

### Step 1: Request a Presigned URL

Send a POST request to `/v1/media/presign` with the file metadata to receive a presigned upload URL.

```bash
curl -X POST \
  "https://api.example.com/v1/media/presign" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "fileName": "product-photo.jpg",
    "contentType": "image/jpeg",
    "fileSize": 2458624
  }'
```

Response:
```json
{
  "uploadUrl": "https://storage.example.com/uploads/abc123?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=...&X-Amz-Signature=...",
  "publicUrl": "https://cdn.example.com/media/abc123/product-photo.jpg",
  "key": "media/abc123/product-photo.jpg",
  "type": "image",
  "expiresIn": 3600
}
```

### Step 2: Upload the File

Use the `uploadUrl` from the response to upload the actual file via an HTTP PUT request. The `Content-Type` header must match the `contentType` specified in step 1.

```bash
curl -X PUT \
  "https://storage.example.com/uploads/abc123?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=...&X-Amz-Signature=..." \
  -H "Content-Type: image/jpeg" \
  --data-binary @product-photo.jpg
```

A successful upload returns an HTTP `200` response with no body.

### Step 3: Use the Public URL

After the upload is complete, use the `publicUrl` returned in step 1 to reference the media in other API calls (e.g., creating a post, updating a profile photo, or setting a GMB media item).

```json
{
  "mediaUrl": "https://cdn.example.com/media/abc123/product-photo.jpg"
}
```

## JavaScript Example

```javascript
async function uploadMedia(file, accessToken) {
  // Step 1: Get presigned URL
  const presignResponse = await fetch('https://api.example.com/v1/media/presign', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${accessToken}`,
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      fileName: file.name,
      contentType: file.type,
      fileSize: file.size,
    }),
  });

  if (!presignResponse.ok) {
    throw new Error(`Presign failed: ${presignResponse.status}`);
  }

  const { uploadUrl, publicUrl, key, type } = await presignResponse.json();

  // Step 2: Upload file to presigned URL
  const uploadResponse = await fetch(uploadUrl, {
    method: 'PUT',
    headers: {
      'Content-Type': file.type,
    },
    body: file,
  });

  if (!uploadResponse.ok) {
    throw new Error(`Upload failed: ${uploadResponse.status}`);
  }

  // Step 3: Return the public URL for use in other API calls
  return {
    publicUrl,
    key,
    type,
  };
}

// Usage
const fileInput = document.getElementById('file-input');
const file = fileInput.files[0];

uploadMedia(file, 'YOUR_ACCESS_TOKEN')
  .then(({ publicUrl, key, type }) => {
    console.log('Upload successful!');
    console.log('Public URL:', publicUrl);
    console.log('Storage Key:', key);
    console.log('File Type:', type);
  })
  .catch((error) => {
    console.error('Upload error:', error.message);
  });
```

## Error Responses

| Status Code | Description                                                                           |
|-------------|---------------------------------------------------------------------------------------|
| `200`       | Success. Returns the presigned URL and file metadata.                                 |
| `400`       | Bad Request. Missing required fields, unsupported content type, or invalid file name. |
| `401`       | Unauthorized. Missing or invalid authentication token.                                |
| `403`       | Forbidden. The authenticated user does not have permission to upload media.            |
| `413`       | Payload Too Large. The specified `fileSize` exceeds the 5 GB limit.                   |
| `415`       | Unsupported Media Type. The specified `contentType` is not supported.                 |
| `429`       | Too Many Requests. Upload rate limit exceeded.                                        |
| `500`       | Internal Server Error. An unexpected error occurred on the server.                    |

## Notes

- The presigned `uploadUrl` expires after the duration specified in `expiresIn` (typically 1 hour). If the URL expires before the upload completes, you must request a new presigned URL.
- The maximum file size is 5 GB. Files larger than this will be rejected.
- The `Content-Type` header in the PUT request to the `uploadUrl` must exactly match the `contentType` specified in the presign request. A mismatch will cause the upload to fail.
- The `publicUrl` is available immediately after a successful upload. There is no additional processing delay for images. Videos may require a brief processing period before they are fully available.
- Uploaded files are stored indefinitely unless explicitly deleted through other API endpoints.
- The `key` field can be used to reference the file in subsequent API calls or for tracking purposes.
- Multiple files can be uploaded in parallel by requesting multiple presigned URLs.
