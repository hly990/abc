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

## Path Parameters

| Parameter   | Type   | Required | Description                          |
|-------------|--------|----------|--------------------------------------|
| `accountId` | string | Yes      | The unique identifier of the account |

## Query Parameters (GET)

| Parameter       | Type   | Required | Description                                                                      |
|-----------------|--------|----------|----------------------------------------------------------------------------------|
| `attributeMask` | string | No       | Comma-separated list of attribute names to include in the response. If omitted, all attributes are returned. |

## Body Parameters (PUT)

| Parameter       | Type   | Required | Description                                                                              |
|-----------------|--------|----------|------------------------------------------------------------------------------------------|
| `attributeMask` | string | No       | Comma-separated list of attribute names to update. If omitted, all provided attributes replace the existing set. |
| `attributes`    | array  | Yes      | Array of attribute objects to set.                                                       |

## Common Attributes

| Attribute Name                   | Type    | Description                                              |
|----------------------------------|---------|----------------------------------------------------------|
| `has_dine_in`                    | boolean | Whether the business offers dine-in service              |
| `has_takeout`                    | boolean | Whether the business offers takeout service              |
| `has_delivery`                   | boolean | Whether the business offers delivery service             |
| `has_wifi`                       | boolean | Whether the business offers free Wi-Fi                   |
| `has_outdoor_seating`            | boolean | Whether the business has outdoor seating                 |
| `pay_credit_card_types_accepted` | array   | List of accepted credit card types                       |
| `has_wheelchair_accessible_entrance` | boolean | Whether the entrance is wheelchair accessible        |
| `has_wheelchair_accessible_seating` | boolean | Whether there is wheelchair accessible seating        |
| `has_restroom`                   | boolean | Whether the business has a restroom                      |
| `has_kids_menu`                  | boolean | Whether the business offers a kids menu                  |
| `has_happy_hour`                 | boolean | Whether the business has a happy hour                    |
| `has_live_music`                 | boolean | Whether the business features live music                 |
| `has_parking`                    | boolean | Whether parking is available                             |
| `requires_reservations`          | boolean | Whether reservations are required                        |
| `serves_alcohol`                 | boolean | Whether the business serves alcohol                      |
| `serves_breakfast`               | boolean | Whether the business serves breakfast                    |
| `serves_lunch`                   | boolean | Whether the business serves lunch                        |
| `serves_dinner`                  | boolean | Whether the business serves dinner                       |
| `serves_brunch`                  | boolean | Whether the business serves brunch                       |
| `serves_vegetarian_food`         | boolean | Whether vegetarian food options are available            |

### Credit Card Types for `pay_credit_card_types_accepted`

| Value              | Description           |
|--------------------|-----------------------|
| `VISA`             | Visa cards            |
| `MASTERCARD`       | Mastercard cards      |
| `AMERICAN_EXPRESS` | American Express cards|
| `DISCOVER`         | Discover cards        |
| `JCB`              | JCB cards             |
| `DINERS_CLUB`      | Diners Club cards     |
| `UNIONPAY`         | UnionPay cards        |

## Request Examples

### Get All Attributes

```bash
curl -X GET \
  "https://api.example.com/v1/accounts/123456/gmb-attributes" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Get Specific Attributes with `attributeMask`

```bash
curl -X GET \
  "https://api.example.com/v1/accounts/123456/gmb-attributes?attributeMask=has_dine_in,has_takeout,has_delivery,has_wifi" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Update Specific Attributes with `attributeMask`

```bash
curl -X PUT \
  "https://api.example.com/v1/accounts/123456/gmb-attributes" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "attributeMask": "has_dine_in,has_takeout,has_delivery,has_wifi,has_outdoor_seating",
    "attributes": [
      {
        "name": "has_dine_in",
        "values": [true]
      },
      {
        "name": "has_takeout",
        "values": [true]
      },
      {
        "name": "has_delivery",
        "values": [false]
      },
      {
        "name": "has_wifi",
        "values": [true]
      },
      {
        "name": "has_outdoor_seating",
        "values": [true]
      }
    ]
  }'
```

### Update Credit Card Types

```bash
curl -X PUT \
  "https://api.example.com/v1/accounts/123456/gmb-attributes" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "attributeMask": "pay_credit_card_types_accepted",
    "attributes": [
      {
        "name": "pay_credit_card_types_accepted",
        "values": ["VISA", "MASTERCARD", "AMERICAN_EXPRESS", "DISCOVER"]
      }
    ]
  }'
```

### Full Replacement (No `attributeMask`)

```bash
curl -X PUT \
  "https://api.example.com/v1/accounts/123456/gmb-attributes" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "attributes": [
      {
        "name": "has_dine_in",
        "values": [true]
      },
      {
        "name": "has_takeout",
        "values": [true]
      },
      {
        "name": "has_delivery",
        "values": [true]
      },
      {
        "name": "has_wifi",
        "values": [true]
      },
      {
        "name": "has_outdoor_seating",
        "values": [false]
      },
      {
        "name": "pay_credit_card_types_accepted",
        "values": ["VISA", "MASTERCARD"]
      }
    ]
  }'
```

## Response Examples

### Full Response (GET)

```json
{
  "attributes": [
    {
      "name": "has_dine_in",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "has_takeout",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "has_delivery",
      "values": [false],
      "valueType": "BOOL"
    },
    {
      "name": "has_wifi",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "has_outdoor_seating",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "pay_credit_card_types_accepted",
      "values": ["VISA", "MASTERCARD", "AMERICAN_EXPRESS", "DISCOVER"],
      "valueType": "ENUM"
    },
    {
      "name": "has_wheelchair_accessible_entrance",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "has_restroom",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "serves_breakfast",
      "values": [false],
      "valueType": "BOOL"
    },
    {
      "name": "serves_lunch",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "serves_dinner",
      "values": [true],
      "valueType": "BOOL"
    }
  ]
}
```

### Filtered Response with `attributeMask`

```json
{
  "attributes": [
    {
      "name": "has_dine_in",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "has_takeout",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "has_delivery",
      "values": [false],
      "valueType": "BOOL"
    },
    {
      "name": "has_wifi",
      "values": [true],
      "valueType": "BOOL"
    }
  ]
}
```

### Update Response (PUT)

```json
{
  "attributes": [
    {
      "name": "has_dine_in",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "has_takeout",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "has_delivery",
      "values": [false],
      "valueType": "BOOL"
    },
    {
      "name": "has_wifi",
      "values": [true],
      "valueType": "BOOL"
    },
    {
      "name": "has_outdoor_seating",
      "values": [true],
      "valueType": "BOOL"
    }
  ]
}
```

## Error Responses

| Status Code | Description                                                                           |
|-------------|---------------------------------------------------------------------------------------|
| `200`       | Success. Returns the attributes.                                                      |
| `400`       | Bad Request. Invalid attribute names, unsupported values, or malformed JSON.           |
| `401`       | Unauthorized. Missing or invalid authentication token.                                |
| `403`       | Forbidden. The authenticated user does not have access to this account.               |
| `404`       | Not Found. The specified `accountId` does not exist.                                  |
| `422`       | Unprocessable Entity. Attribute validation errors (e.g., wrong value type).           |
| `500`       | Internal Server Error. An unexpected error occurred on the server.                    |

## Notes

- Boolean attributes accept `[true]` or `[false]` as values.
- Enum attributes (like `pay_credit_card_types_accepted`) accept an array of valid enum strings.
- When using `attributeMask` on a PUT request, only the specified attributes are modified. Attributes not listed in the mask remain unchanged.
- Without `attributeMask`, a PUT request replaces the entire attribute set. Any attributes not included in the request body are removed.
- Available attributes vary depending on the business category. Not all attributes listed here may be applicable to every business type.
- Attribute names are case-sensitive and must match exactly as documented.
