# GMB Location Details API

Retrieve and update Google My Business location details for a given account.

## Endpoints

### Get Location Details

```
GET /v1/accounts/{accountId}/gmb-location-details
```

Retrieves the location details for the specified account.

### Update Location Details

```
PUT /v1/accounts/{accountId}/gmb-location-details
```

Updates the location details for the specified account.

## Path Parameters

| Parameter   | Type   | Required | Description                          |
|-------------|--------|----------|--------------------------------------|
| `accountId` | string | Yes      | The unique identifier of the account |

## Query Parameters (GET)

| Parameter  | Type   | Required | Description                                                                                              |
|------------|--------|----------|----------------------------------------------------------------------------------------------------------|
| `readMask` | string | No       | Comma-separated list of fields to include in the response. If omitted, all fields are returned.          |

### Supported `readMask` Values

- `regularHours`
- `specialHours`
- `profile`
- `websiteUri`
- `phoneNumbers`
- `address`
- `categories`
- `serviceArea`
- `openInfo`
- `latlng`

## Body Parameters (PUT)

| Parameter    | Type   | Required | Description                                                                                      |
|--------------|--------|----------|--------------------------------------------------------------------------------------------------|
| `updateMask` | string | No       | Comma-separated list of fields to update. If omitted, the entire location details object is replaced. |

### Supported `updateMask` Values

- `regularHours`
- `specialHours`
- `profile`
- `websiteUri`
- `phoneNumbers`
- `address`
- `categories`

## Response Fields

| Field           | Type   | Description                                                              |
|-----------------|--------|--------------------------------------------------------------------------|
| `regularHours`  | object | The regular operating hours for the business                             |
| `specialHours`  | object | Special hours that override regular hours (e.g., holidays)               |
| `profile`       | object | Business profile information including description                       |
| `websiteUri`    | string | The URL of the business website                                          |
| `phoneNumbers`  | object | Primary and additional phone numbers for the business                    |
| `address`       | object | The physical address of the business                                     |
| `categories`    | object | Primary and additional business categories                               |
| `serviceArea`   | object | The geographic area served by the business (for service-area businesses) |
| `openInfo`      | object | Information about whether the business is open, closed, or temporarily closed |
| `latlng`        | object | The latitude and longitude of the business location                      |

## Response Examples

### Full Response

```json
{
  "regularHours": {
    "periods": [
      {
        "openDay": "MONDAY",
        "openTime": "09:00",
        "closeDay": "MONDAY",
        "closeTime": "17:00"
      },
      {
        "openDay": "TUESDAY",
        "openTime": "09:00",
        "closeDay": "TUESDAY",
        "closeTime": "17:00"
      },
      {
        "openDay": "WEDNESDAY",
        "openTime": "09:00",
        "closeDay": "WEDNESDAY",
        "closeTime": "17:00"
      },
      {
        "openDay": "THURSDAY",
        "openTime": "09:00",
        "closeDay": "THURSDAY",
        "closeTime": "21:00"
      },
      {
        "openDay": "FRIDAY",
        "openTime": "09:00",
        "closeDay": "FRIDAY",
        "closeTime": "21:00"
      },
      {
        "openDay": "SATURDAY",
        "openTime": "10:00",
        "closeDay": "SATURDAY",
        "closeTime": "18:00"
      }
    ]
  },
  "specialHours": {
    "specialHourPeriods": [
      {
        "startDate": {
          "year": 2025,
          "month": 12,
          "day": 25
        },
        "isClosed": true
      },
      {
        "startDate": {
          "year": 2025,
          "month": 12,
          "day": 31
        },
        "openTime": "10:00",
        "closeTime": "15:00",
        "isClosed": false
      }
    ]
  },
  "profile": {
    "description": "A family-owned Italian restaurant serving authentic Neapolitan cuisine since 1985. We use only the freshest locally sourced ingredients."
  },
  "websiteUri": "https://www.example-restaurant.com",
  "phoneNumbers": {
    "primaryPhone": "+1-555-123-4567",
    "additionalPhones": [
      "+1-555-123-4568",
      "+1-555-123-4569"
    ]
  },
  "address": {
    "regionCode": "US",
    "languageCode": "en",
    "postalCode": "90210",
    "administrativeArea": "CA",
    "locality": "Beverly Hills",
    "addressLines": [
      "123 Main Street",
      "Suite 100"
    ]
  },
  "categories": {
    "primaryCategory": {
      "categoryId": "gcid:italian_restaurant",
      "displayName": "Italian Restaurant"
    },
    "additionalCategories": [
      {
        "categoryId": "gcid:pizza_restaurant",
        "displayName": "Pizza Restaurant"
      }
    ]
  },
  "openInfo": {
    "status": "OPEN",
    "canReopen": true
  },
  "latlng": {
    "latitude": 34.0901,
    "longitude": -118.4065
  }
}
```

### Partial Response with `readMask`

Request:
```bash
curl -X GET \
  "https://api.example.com/v1/accounts/123456/gmb-location-details?readMask=regularHours,phoneNumbers,websiteUri" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

Response:
```json
{
  "regularHours": {
    "periods": [
      {
        "openDay": "MONDAY",
        "openTime": "09:00",
        "closeDay": "MONDAY",
        "closeTime": "17:00"
      },
      {
        "openDay": "TUESDAY",
        "openTime": "09:00",
        "closeDay": "TUESDAY",
        "closeTime": "17:00"
      }
    ]
  },
  "phoneNumbers": {
    "primaryPhone": "+1-555-123-4567",
    "additionalPhones": [
      "+1-555-123-4568"
    ]
  },
  "websiteUri": "https://www.example-restaurant.com"
}
```

## Request Examples

### Update Regular Hours

```bash
curl -X PUT \
  "https://api.example.com/v1/accounts/123456/gmb-location-details" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "updateMask": "regularHours",
    "regularHours": {
      "periods": [
        {
          "openDay": "MONDAY",
          "openTime": "08:00",
          "closeDay": "MONDAY",
          "closeTime": "18:00"
        },
        {
          "openDay": "TUESDAY",
          "openTime": "08:00",
          "closeDay": "TUESDAY",
          "closeTime": "18:00"
        }
      ]
    }
  }'
```

### Update Special Hours

```bash
curl -X PUT \
  "https://api.example.com/v1/accounts/123456/gmb-location-details" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "updateMask": "specialHours",
    "specialHours": {
      "specialHourPeriods": [
        {
          "startDate": {
            "year": 2025,
            "month": 12,
            "day": 25
          },
          "isClosed": true
        }
      ]
    }
  }'
```

### Update Profile and Website

```bash
curl -X PUT \
  "https://api.example.com/v1/accounts/123456/gmb-location-details" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "updateMask": "profile,websiteUri",
    "profile": {
      "description": "Updated business description with new details."
    },
    "websiteUri": "https://www.new-website.com"
  }'
```

### Update Phone Numbers

```bash
curl -X PUT \
  "https://api.example.com/v1/accounts/123456/gmb-location-details" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "updateMask": "phoneNumbers",
    "phoneNumbers": {
      "primaryPhone": "+1-555-999-0000",
      "additionalPhones": [
        "+1-555-999-0001"
      ]
    }
  }'
```

## Error Responses

| Status Code | Description                                                                     |
|-------------|---------------------------------------------------------------------------------|
| `200`       | Success. Returns the location details object.                                   |
| `400`       | Bad Request. Invalid field values, unsupported `readMask`/`updateMask`, or malformed JSON. |
| `401`       | Unauthorized. Missing or invalid authentication token.                          |
| `403`       | Forbidden. The authenticated user does not have access to this account.         |
| `404`       | Not Found. The specified `accountId` does not exist.                            |
| `422`       | Unprocessable Entity. Validation errors (e.g., invalid phone number format).    |
| `500`       | Internal Server Error. An unexpected error occurred on the server.              |

## Notes

- Days of the week are represented as enums: `MONDAY`, `TUESDAY`, `WEDNESDAY`, `THURSDAY`, `FRIDAY`, `SATURDAY`, `SUNDAY`.
- Times are in 24-hour format (e.g., `"09:00"`, `"21:00"`).
- A business that is open 24 hours should set `openTime` to `"00:00"` and `closeTime` to `"24:00"`.
- For businesses closed on a specific day, simply omit that day from the `periods` array.
- The `openInfo.status` field can be `OPEN`, `CLOSED_PERMANENTLY`, or `CLOSED_TEMPORARILY`.
- When using `readMask`, only the specified fields are returned. Unspecified fields are omitted from the response entirely.
- When using `updateMask`, only the specified fields are modified. All other fields remain unchanged.