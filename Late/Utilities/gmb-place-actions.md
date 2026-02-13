# GMB Place Actions API

Manage Google My Business place action links for a given account. Place actions are links that appear on a business listing to drive specific user actions.

## Endpoints

### List Place Actions

```
GET /v1/accounts/{accountId}/gmb-place-actions
```

Retrieves all place action links for the specified account.

### Create Place Action

```
POST /v1/accounts/{accountId}/gmb-place-actions
```

Creates a new place action link for the specified account.

### Delete Place Action

```
DELETE /v1/accounts/{accountId}/gmb-place-actions/{actionId}
```

Deletes a specific place action link from the specified account.

## Path Parameters

| Parameter   | Type   | Required | Description                                              |
|-------------|--------|----------|----------------------------------------------------------|
| `accountId` | string | Yes      | The unique identifier of the account                     |
| `actionId`  | string | Yes      | The unique identifier of the place action (DELETE only)  |

## Action Types

| Action Type           | Description                                                          |
|-----------------------|----------------------------------------------------------------------|
| `APPOINTMENT`         | Link for booking a general appointment                               |
| `ONLINE_APPOINTMENT`  | Link for booking an online/virtual appointment                       |
| `DINING_RESERVATION`  | Link for making a dining reservation                                 |
| `FOOD_ORDERING`       | Link for ordering food (general, covers both delivery and takeout)   |
| `FOOD_DELIVERY`       | Link specifically for ordering food delivery                         |
| `FOOD_TAKEOUT`        | Link specifically for ordering food for takeout/pickup               |
| `SHOP_ONLINE`         | Link for shopping online from the business                           |

## Request Body (POST - Create)

| Field        | Type   | Required | Description                                                   |
|--------------|--------|----------|---------------------------------------------------------------|
| `actionType` | string | Yes      | The type of place action. Must be a valid action type.        |
| `uri`        | string | Yes      | The URL that the action links to. Must be a valid HTTPS URL.  |
| `provider`   | string | No       | The name of the provider for this action (e.g., "OpenTable"). |

## Request Examples

### List Place Actions

```bash
curl -X GET \
  "https://api.example.com/v1/accounts/123456/gmb-place-actions" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Create a Dining Reservation Action

```bash
curl -X POST \
  "https://api.example.com/v1/accounts/123456/gmb-place-actions" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "actionType": "DINING_RESERVATION",
    "uri": "https://www.opentable.com/r/example-restaurant",
    "provider": "OpenTable"
  }'
```

### Create a Food Delivery Action

```bash
curl -X POST \
  "https://api.example.com/v1/accounts/123456/gmb-place-actions" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "actionType": "FOOD_DELIVERY",
    "uri": "https://www.doordash.com/store/example-restaurant",
    "provider": "DoorDash"
  }'
```

### Create a Food Takeout Action

```bash
curl -X POST \
  "https://api.example.com/v1/accounts/123456/gmb-place-actions" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "actionType": "FOOD_TAKEOUT",
    "uri": "https://order.example-restaurant.com/takeout",
    "provider": "Direct"
  }'
```

### Create an Online Appointment Action

```bash
curl -X POST \
  "https://api.example.com/v1/accounts/123456/gmb-place-actions" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "actionType": "ONLINE_APPOINTMENT",
    "uri": "https://calendly.com/example-business/consultation"
  }'
```

### Create a Shop Online Action

```bash
curl -X POST \
  "https://api.example.com/v1/accounts/123456/gmb-place-actions" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "actionType": "SHOP_ONLINE",
    "uri": "https://shop.example-business.com"
  }'
```

### Delete a Place Action

```bash
curl -X DELETE \
  "https://api.example.com/v1/accounts/123456/gmb-place-actions/action-789" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Response Examples

### List Place Actions Response (GET)

```json
{
  "placeActions": [
    {
      "actionId": "action-001",
      "actionType": "DINING_RESERVATION",
      "uri": "https://www.opentable.com/r/example-restaurant",
      "provider": "OpenTable",
      "createTime": "2025-03-10T08:00:00Z",
      "updateTime": "2025-03-10T08:00:00Z"
    },
    {
      "actionId": "action-002",
      "actionType": "FOOD_DELIVERY",
      "uri": "https://www.doordash.com/store/example-restaurant",
      "provider": "DoorDash",
      "createTime": "2025-04-15T12:30:00Z",
      "updateTime": "2025-04-15T12:30:00Z"
    },
    {
      "actionId": "action-003",
      "actionType": "FOOD_TAKEOUT",
      "uri": "https://order.example-restaurant.com/takeout",
      "provider": "Direct",
      "createTime": "2025-04-15T12:35:00Z",
      "updateTime": "2025-04-15T12:35:00Z"
    },
    {
      "actionId": "action-004",
      "actionType": "SHOP_ONLINE",
      "uri": "https://shop.example-business.com",
      "provider": null,
      "createTime": "2025-05-20T09:00:00Z",
      "updateTime": "2025-05-20T09:00:00Z"
    }
  ],
  "totalCount": 4
}
```

### Create Place Action Response (POST)

```json
{
  "actionId": "action-005",
  "actionType": "APPOINTMENT",
  "uri": "https://booking.example-business.com",
  "provider": null,
  "createTime": "2025-08-01T14:00:00Z",
  "updateTime": "2025-08-01T14:00:00Z"
}
```

### Delete Place Action Response (DELETE)

```json
{
  "success": true,
  "message": "Place action action-005 has been deleted."
}
```

## Response Fields

| Field                          | Type   | Description                                          |
|--------------------------------|--------|------------------------------------------------------|
| `placeActions`                 | array  | Array of place action objects (GET only)             |
| `placeActions[].actionId`      | string | Unique identifier of the place action                |
| `placeActions[].actionType`    | string | The type of place action                             |
| `placeActions[].uri`           | string | The URL that the action links to                     |
| `placeActions[].provider`      | string | The provider name, or `null` if not specified        |
| `placeActions[].createTime`    | string | ISO 8601 timestamp when the action was created       |
| `placeActions[].updateTime`    | string | ISO 8601 timestamp when the action was last updated  |
| `totalCount`                   | integer| Total number of place actions for the account        |

## Error Responses

| Status Code | Description                                                                          |
|-------------|--------------------------------------------------------------------------------------|
| `200`       | Success. Returns the list of place actions (GET).                                    |
| `201`       | Created. The place action was successfully created (POST).                           |
| `204`       | No Content. The place action was successfully deleted (DELETE).                      |
| `400`       | Bad Request. Invalid action type, malformed URL, or malformed JSON.                  |
| `401`       | Unauthorized. Missing or invalid authentication token.                               |
| `403`       | Forbidden. The authenticated user does not have access to this account.              |
| `404`       | Not Found. The specified `accountId` or `actionId` does not exist.                   |
| `409`       | Conflict. A place action with the same `actionType` and `uri` already exists.        |
| `422`       | Unprocessable Entity. Validation errors (e.g., the URI is not a valid HTTPS URL).    |
| `500`       | Internal Server Error. An unexpected error occurred on the server.                   |

## Notes

- Each account can have multiple place actions, but duplicate combinations of `actionType` and `uri` are not allowed.
- The `uri` must be a valid HTTPS URL. HTTP URLs are not accepted.
- The `provider` field is optional and is used to display the name of the third-party service (e.g., "OpenTable", "DoorDash", "Uber Eats").
- Place actions are displayed on the business listing in Google Search and Google Maps.
- Not all action types are available for all business categories. For example, `DINING_RESERVATION` is only available for food-service businesses.
- Deleting a place action immediately removes it from the business listing.
- There is no update (PATCH/PUT) endpoint for individual place actions. To modify a place action, delete the existing one and create a new one.
