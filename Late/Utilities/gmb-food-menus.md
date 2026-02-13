# GMB Food Menus API

Retrieve and update Google My Business food menus for a given account.

## Endpoints

### Get Food Menu

```
GET /v1/accounts/{accountId}/gmb-food-menus
```

Retrieves the current food menu for the specified account.

### Update Food Menu

```
PUT /v1/accounts/{accountId}/gmb-food-menus
```

Updates the food menu for the specified account. Use the `updateMask` parameter to perform partial updates.

## Path Parameters

| Parameter   | Type   | Required | Description                          |
|-------------|--------|----------|--------------------------------------|
| `accountId` | string | Yes      | The unique identifier of the account |

## Query Parameters (PUT)

| Parameter    | Type   | Required | Description                                                                                      |
|--------------|--------|----------|--------------------------------------------------------------------------------------------------|
| `updateMask` | string | No       | Comma-separated list of fields to update. If omitted, the entire menu object is replaced.        |

## Error Responses

| Status Code | Description                                                                      |
|-------------|----------------------------------------------------------------------------------|
| `200`       | Success. Returns the menu object.                                                |
| `400`       | Bad Request. Invalid menu structure or malformed JSON.                            |
| `401`       | Unauthorized. Missing or invalid authentication token.                           |
| `403`       | Forbidden.                                                                       |
| `404`       | Not Found. The specified `accountId` does not exist.                             |
| `422`       | Unprocessable Entity. Validation errors in menu data.                            |
| `500`       | Internal Server Error.                                                           |

## Notes

- When using `updateMask`, only the specified fields are modified.
- Price is represented using `units` and `nanos`. For example, $9.99 is `units: 9, nanos: 990000000`.
- Labels support multiple languages via the `languageCode` field.
