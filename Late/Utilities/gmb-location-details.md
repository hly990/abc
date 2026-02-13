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

## Supported Fields

- `regularHours` - The regular operating hours
- `specialHours` - Special hours that override regular hours
- `profile` - Business profile information
- `websiteUri` - Business website URL
- `phoneNumbers` - Primary and additional phone numbers
- `address` - Physical address
- `categories` - Business categories
- `serviceArea` - Geographic area served
- `openInfo` - Open/closed status
- `latlng` - Latitude and longitude

## Error Responses

| Status Code | Description                                                                     |
|-------------|---------------------------------------------------------------------------------|
| `200`       | Success. Returns the location details object.                                   |
| `400`       | Bad Request. Invalid field values or unsupported mask.                           |
| `401`       | Unauthorized. Missing or invalid authentication token.                          |
| `403`       | Forbidden.                                                                      |
| `404`       | Not Found. The specified `accountId` does not exist.                            |
| `422`       | Unprocessable Entity. Validation errors.                                        |
| `500`       | Internal Server Error.                                                          |

## Notes

- Days of the week are enums: `MONDAY`, `TUESDAY`, `WEDNESDAY`, `THURSDAY`, `FRIDAY`, `SATURDAY`, `SUNDAY`.
- Times are in 24-hour format (e.g., `"09:00"`, `"21:00"`).
- When using `updateMask`, only the specified fields are modified. All other fields remain unchanged.
