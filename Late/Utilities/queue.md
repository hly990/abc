# Queue API

Manage queue schedules and time slots for scheduling posts.

## Endpoints

| Method   | Endpoint                | Description                                  |
|----------|-------------------------|----------------------------------------------|
| `GET`    | `/v1/queue/slots`       | Get queue schedules and configured time slots |
| `POST`   | `/v1/queue/slots`       | Create new queue time slots                   |
| `DELETE` | `/v1/queue/slots`       | Delete queue time slots                       |
| `PUT`    | `/v1/queue/slots`       | Update existing queue time slots              |
| `GET`    | `/v1/queue/preview`     | Preview the upcoming queue schedule           |
| `GET`    | `/v1/queue/next-slot`   | Get the next available queue time slot        |

---

## Get Queue Schedules

```
GET /v1/queue/slots
```

Retrieves the configured queue schedules including all time slots.

### Query Parameters

| Parameter   | Type   | Required | Description                                                                 |
|-------------|--------|----------|-----------------------------------------------------------------------------|
| `accountId` | string | Yes      | The unique identifier of the account                                        |
| `timezone`  | string | No       | IANA timezone string (e.g., `America/New_York`). Defaults to account timezone. |

### Response Example

```json
{
  "timezone": "America/New_York",
  "schedules": [
    {
      "day": "MONDAY",
      "slots": [
        { "time": "09:00", "enabled": true },
        { "time": "12:30", "enabled": true },
        { "time": "17:00", "enabled": true }
      ]
    },
    {
      "day": "TUESDAY",
      "slots": [
        { "time": "09:00", "enabled": true },
        { "time": "12:30", "enabled": true },
        { "time": "17:00", "enabled": false }
      ]
    },
    {
      "day": "WEDNESDAY",
      "slots": [
        { "time": "10:00", "enabled": true },
        { "time": "14:00", "enabled": true }
      ]
    },
    {
      "day": "THURSDAY",
      "slots": [
        { "time": "09:00", "enabled": true },
        { "time": "12:30", "enabled": true },
        { "time": "17:00", "enabled": true }
      ]
    },
    {
      "day": "FRIDAY",
      "slots": [
        { "time": "09:00", "enabled": true },
        { "time": "12:00", "enabled": true }
      ]
    },
    {
      "day": "SATURDAY",
      "slots": [
        { "time": "10:00", "enabled": true }
      ]
    },
    {
      "day": "SUNDAY",
      "slots": []
    }
  ]
}
```

---

## Create Queue Slots

```
POST /v1/queue/slots
```

Creates new time slots in the queue schedule.

### Request Body

| Field               | Type    | Required | Description                                                                                          |
|---------------------|---------|----------|------------------------------------------------------------------------------------------------------|
| `accountId`         | string  | Yes      | The unique identifier of the account                                                                 |
| `timezone`          | string  | No       | IANA timezone string. Defaults to account timezone.                                                  |
| `slots`             | array   | Yes      | Array of slot configuration objects to create                                                        |
| `reshuffleExisting` | boolean | No       | If `true`, existing queued posts are reshuffled to accommodate the new slots. Defaults to `false`.    |

### Slot Configuration Object

| Field   | Type    | Required | Description                                                     |
|---------|---------|----------|-----------------------------------------------------------------|
| `day`   | string  | Yes      | Day of the week: `MONDAY` through `SUNDAY`                      |
| `time`  | string  | Yes      | Time in 24-hour format (e.g., `"09:00"`, `"14:30"`)            |
| `enabled`| boolean| No       | Whether the slot is active. Defaults to `true`.                 |

### Request Example

```bash
curl -X POST \
  "https://api.example.com/v1/queue/slots" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "accountId": "123456",
    "timezone": "America/New_York",
    "slots": [
      { "day": "MONDAY", "time": "08:00", "enabled": true },
      { "day": "MONDAY", "time": "18:00", "enabled": true },
      { "day": "WEDNESDAY", "time": "08:00", "enabled": true }
    ],
    "reshuffleExisting": true
  }'
```

### Response Example

```json
{
  "created": 3,
  "reshuffled": true,
  "slots": [
    { "day": "MONDAY", "time": "08:00", "enabled": true },
    { "day": "MONDAY", "time": "18:00", "enabled": true },
    { "day": "WEDNESDAY", "time": "08:00", "enabled": true }
  ]
}
```

---

## Delete Queue Slots

```
DELETE /v1/queue/slots
```

Deletes specific time slots from the queue schedule.

### Request Body

| Field               | Type    | Required | Description                                                                                           |
|---------------------|---------|----------|-------------------------------------------------------------------------------------------------------|
| `accountId`         | string  | Yes      | The unique identifier of the account                                                                  |
| `slots`             | array   | Yes      | Array of slot identifiers to delete (each with `day` and `time`)                                      |
| `reshuffleExisting` | boolean | No       | If `true`, existing queued posts are reshuffled after slots are removed. Defaults to `false`.          |

### Request Example

```bash
curl -X DELETE \
  "https://api.example.com/v1/queue/slots" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "accountId": "123456",
    "slots": [
      { "day": "MONDAY", "time": "18:00" },
      { "day": "SATURDAY", "time": "10:00" }
    ],
    "reshuffleExisting": true
  }'
```

### Response Example

```json
{
  "deleted": 2,
  "reshuffled": true
}
```

---

## Update Queue Slots

```
PUT /v1/queue/slots
```

Updates existing time slots in the queue schedule. Can be used to change the time, day, or enabled status of slots.

### Request Body

| Field               | Type    | Required | Description                                                                                    |
|---------------------|---------|----------|------------------------------------------------------------------------------------------------|
| `accountId`         | string  | Yes      | The unique identifier of the account                                                           |
| `timezone`          | string  | No       | IANA timezone string. Defaults to account timezone.                                            |
| `updates`           | array   | Yes      | Array of update objects, each with the original slot and the new configuration                  |
| `reshuffleExisting` | boolean | No       | If `true`, existing queued posts are reshuffled to accommodate changes. Defaults to `false`.    |

### Update Object

| Field     | Type   | Required | Description                                          |
|-----------|--------|----------|------------------------------------------------------|
| `original`| object | Yes      | The original slot to update (with `day` and `time`)  |
| `updated` | object | Yes      | The new slot configuration (with `day`, `time`, and optionally `enabled`) |

### Request Example

```bash
curl -X PUT \
  "https://api.example.com/v1/queue/slots" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "accountId": "123456",
    "timezone": "America/New_York",
    "updates": [
      {
        "original": { "day": "MONDAY", "time": "09:00" },
        "updated": { "day": "MONDAY", "time": "08:30", "enabled": true }
      },
      {
        "original": { "day": "TUESDAY", "time": "17:00" },
        "updated": { "day": "TUESDAY", "time": "17:00", "enabled": false }
      }
    ],
    "reshuffleExisting": true
  }'
```

### Response Example

```json
{
  "updated": 2,
  "reshuffled": true,
  "slots": [
    { "day": "MONDAY", "time": "08:30", "enabled": true },
    { "day": "TUESDAY", "time": "17:00", "enabled": false }
  ]
}
```

---

## Preview Queue Schedule

```
GET /v1/queue/preview
```

Returns a preview of the upcoming queue showing when posts will be published.

### Query Parameters

| Parameter   | Type    | Required | Description                                                            |
|-------------|---------|----------|------------------------------------------------------------------------|
| `accountId` | string  | Yes      | The unique identifier of the account                                   |
| `days`      | integer | No       | Number of days to preview. Defaults to 7. Max is 30.                   |
| `timezone`  | string  | No       | IANA timezone string. Defaults to account timezone.                    |

### Request Example

```bash
curl -X GET \
  "https://api.example.com/v1/queue/preview?accountId=123456&days=7&timezone=America/New_York" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Response Example

```json
{
  "timezone": "America/New_York",
  "preview": [
    {
      "date": "2025-08-04",
      "day": "MONDAY",
      "slots": [
        { "time": "09:00", "status": "FILLED", "postId": "post-abc-001" },
        { "time": "12:30", "status": "FILLED", "postId": "post-abc-002" },
        { "time": "17:00", "status": "EMPTY", "postId": null }
      ]
    },
    {
      "date": "2025-08-05",
      "day": "TUESDAY",
      "slots": [
        { "time": "09:00", "status": "EMPTY", "postId": null },
        { "time": "12:30", "status": "EMPTY", "postId": null }
      ]
    }
  ],
  "totalSlots": 5,
  "filledSlots": 2,
  "emptySlots": 3
}
```

---

## Get Next Available Slot

```
GET /v1/queue/next-slot
```

Returns the next available (empty) time slot in the queue.

### Query Parameters

| Parameter   | Type   | Required | Description                                                            |
|-------------|--------|----------|------------------------------------------------------------------------|
| `accountId` | string | Yes      | The unique identifier of the account                                   |
| `timezone`  | string | No       | IANA timezone string. Defaults to account timezone.                    |

### Request Example

```bash
curl -X GET \
  "https://api.example.com/v1/queue/next-slot?accountId=123456" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Response Example

```json
{
  "timezone": "America/New_York",
  "nextSlot": {
    "date": "2025-08-04",
    "day": "MONDAY",
    "time": "17:00",
    "dateTime": "2025-08-04T17:00:00-04:00"
  }
}
```

### No Available Slots

```json
{
  "timezone": "America/New_York",
  "nextSlot": null,
  "message": "No available queue slots found. Configure queue slots first."
}
```

---

## The `reshuffleExisting` Parameter

The `reshuffleExisting` parameter is available on the Create, Delete, and Update endpoints. When set to `true`, any posts already queued are redistributed across the updated slot schedule.

**Behavior:**

- **`reshuffleExisting: false` (default):** Existing queued posts retain their scheduled times. If a slot is deleted or moved, any post assigned to that slot will remain at its originally scheduled time (even if the slot no longer exists).
- **`reshuffleExisting: true`:** All queued posts are reassigned to the current set of active slots in chronological order. This ensures an even distribution across the updated schedule.

**Example scenario:**

1. You have 6 posts queued across 3 slots per day (Mon-Wed).
2. You add 2 new slots per day with `reshuffleExisting: true`.
3. All 6 posts are redistributed evenly across the 5 slots per day.

## Timezone Handling

- All times in requests and responses use the specified timezone or the account's default timezone.
- Timezones must be valid IANA timezone identifiers (e.g., `America/New_York`, `Europe/London`, `Asia/Tokyo`).
- If an invalid timezone is provided, the API returns a `400` error.
- Daylight saving time transitions are handled automatically.

## Error Responses

| Status Code | Description                                                                         |
|-------------|-------------------------------------------------------------------------------------|
| `200`       | Success. Returns the requested data.                                                |
| `201`       | Created. New slots were successfully created (POST).                                |
| `400`       | Bad Request. Invalid parameters, invalid timezone, invalid time format, or duplicate slot. |
| `401`       | Unauthorized. Missing or invalid authentication token.                              |
| `403`       | Forbidden. The authenticated user does not have access to this account.             |
| `404`       | Not Found. The specified `accountId` does not exist, or the slot to update/delete was not found. |
| `409`       | Conflict. A slot already exists for the specified day and time (POST).              |
| `500`       | Internal Server Error. An unexpected error occurred on the server.                  |

## Notes

- Times are in 24-hour format and must be in 30-minute increments (e.g., `"09:00"`, `"09:30"`, `"10:00"`).
- Each day can have a maximum of 25 time slots.
- Days with an empty `slots` array have no scheduled posting times.
- The `enabled` field on a slot allows temporarily disabling a slot without deleting it.
- Disabled slots (`enabled: false`) are skipped when assigning posts to the queue.
