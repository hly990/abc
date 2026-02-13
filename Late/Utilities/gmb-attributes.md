# GMB Attributes API

Retrieve and update Google My Business attributes for a given account.

## Endpoints

### Get Attributes

```
GET /v1/accounts/{accountId}/gmb-attributes
```

Retrieves the current attributes for the specified account.

### Update Attributes

```
PUT /v1/accounts/{accountId}/gmb-attributes
```

Updates the attributes for the specified account.

## Common Attributes

| Attribute Name                   | Type    | Description                                              |
|----------------------------------|---------|----------------------------------------------------------|
| `has_dine_in`                    | boolean | Whether the business offers dine-in service              |
| `has_takeout`                    | boolean | Whether the business offers takeout service              |
| `has_delivery`                   | boolean | Whether the business offers delivery service             |
| `has_wifi`                       | boolean | Whether the business offers free Wi-Fi                   |
| `has_outdoor_seating`            | boolean | Whether the business has outdoor seating                 |
| `has_wheelchair_accessible_entrance` | boolean | Whether the entrance is wheelchair accessible        |
| `has_restroom`                   | boolean | Whether the business has a restroom                      |
| `serves_breakfast`               | boolean | Whether the business serves breakfast                    |
| `serves_lunch`                   | boolean | Whether the business serves lunch                        |
| `serves_dinner`                  | boolean | Whether the business serves dinner                       |

## Error Responses

| Status Code | Description                                                                           |
|-------------|---------------------------------------------------------------------------------------|
| `200`       | Success. Returns the attributes.                                                      |
| `400`       | Bad Request. Invalid attribute names or unsupported values.                            |
| `401`       | Unauthorized.                                                                         |
| `403`       | Forbidden.                                                                            |
| `404`       | Not Found.                                                                            |
| `422`       | Unprocessable Entity. Attribute validation errors.                                    |
| `500`       | Internal Server Error.                                                                |

## Notes

- Boolean attributes accept `[true]` or `[false]` as values.
- Available attributes vary depending on the business category.
- Attribute names are case-sensitive and must match exactly as documented.
