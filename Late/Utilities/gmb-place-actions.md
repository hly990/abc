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

## Action Types

| Action Type           | Description                                                          |
|-----------------------|----------------------------------------------------------------------|
| `APPOINTMENT`         | Link for booking a general appointment                               |
| `ONLINE_APPOINTMENT`  | Link for booking an online/virtual appointment                       |
| `DINING_RESERVATION`  | Link for making a dining reservation                                 |
| `FOOD_ORDERING`       | Link for ordering food                                               |
| `FOOD_DELIVERY`       | Link specifically for ordering food delivery                         |
| `FOOD_TAKEOUT`        | Link specifically for ordering food for takeout/pickup               |
| `SHOP_ONLINE`         | Link for shopping online from the business                           |

## Error Responses

| Status Code | Description                                                                          |
|-------------|--------------------------------------------------------------------------------------|
| `200`       | Success. Returns the list of place actions.                                          |
| `201`       | Created. The place action was successfully created.                                  |
| `204`       | No Content. The place action was successfully deleted.                               |
| `400`       | Bad Request. Invalid action type or malformed URL.                                   |
| `401`       | Unauthorized.                                                                        |
| `403`       | Forbidden.                                                                           |
| `404`       | Not Found.                                                                           |
| `409`       | Conflict. A place action with the same `actionType` and `uri` already exists.        |
| `500`       | Internal Server Error.                                                               |

## Notes

- Each account can have multiple place actions, but duplicate combinations of `actionType` and `uri` are not allowed.
- The `uri` must be a valid HTTPS URL.
- There is no update (PATCH/PUT) endpoint for individual place actions. To modify a place action, delete the existing one and create a new one.
