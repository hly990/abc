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

### Supported `updateMask` Values

- `menus` - Update all menus
- `menus.labels` - Update menu labels only
- `menus.sections` - Update all sections within menus
- `menus.sections.items` - Update items within sections
- `menus.sections.items.dietaryRestrictions` - Update dietary restriction data only
- `menus.sections.items.allergenData` - Update allergen data only

## Menu Object Structure

```json
{
  "menus": [
    {
      "menuId": "menu-001",
      "labels": [
        {
          "displayName": "Lunch Menu",
          "description": "Served daily from 11 AM to 3 PM",
          "languageCode": "en"
        }
      ],
      "sections": [
        {
          "sectionId": "section-appetizers",
          "labels": [
            {
              "displayName": "Appetizers",
              "description": "Start your meal right",
              "languageCode": "en"
            }
          ],
          "items": [
            {
              "itemId": "item-001",
              "labels": [
                {
                  "displayName": "Bruschetta",
                  "description": "Toasted bread topped with fresh tomatoes, basil, and olive oil",
                  "languageCode": "en"
                }
              ],
              "price": {
                "currencyCode": "USD",
                "units": 9,
                "nanos": 990000000
              },
              "dietaryRestrictions": [
                "VEGETARIAN",
                "VEGAN"
              ],
              "allergenData": {
                "allergens": [
                  {
                    "allergenType": "GLUTEN",
                    "isPresent": true
                  },
                  {
                    "allergenType": "DAIRY",
                    "isPresent": false
                  },
                  {
                    "allergenType": "NUTS",
                    "isPresent": false
                  },
                  {
                    "allergenType": "SOY",
                    "isPresent": false
                  }
                ]
              }
            },
            {
              "itemId": "item-002",
              "labels": [
                {
                  "displayName": "Caesar Salad",
                  "description": "Romaine lettuce, parmesan, croutons, and house-made Caesar dressing",
                  "languageCode": "en"
                }
              ],
              "price": {
                "currencyCode": "USD",
                "units": 12,
                "nanos": 500000000
              },
              "dietaryRestrictions": [
                "VEGETARIAN"
              ],
              "allergenData": {
                "allergens": [
                  {
                    "allergenType": "GLUTEN",
                    "isPresent": true
                  },
                  {
                    "allergenType": "DAIRY",
                    "isPresent": true
                  },
                  {
                    "allergenType": "EGGS",
                    "isPresent": true
                  }
                ]
              }
            }
          ]
        },
        {
          "sectionId": "section-mains",
          "labels": [
            {
              "displayName": "Main Courses",
              "description": "Chef's specialties",
              "languageCode": "en"
            }
          ],
          "items": [
            {
              "itemId": "item-003",
              "labels": [
                {
                  "displayName": "Grilled Salmon",
                  "description": "Atlantic salmon with lemon butter sauce, served with seasonal vegetables",
                  "languageCode": "en"
                }
              ],
              "price": {
                "currencyCode": "USD",
                "units": 24,
                "nanos": 0
              },
              "dietaryRestrictions": [
                "GLUTEN_FREE"
              ],
              "allergenData": {
                "allergens": [
                  {
                    "allergenType": "FISH",
                    "isPresent": true
                  },
                  {
                    "allergenType": "DAIRY",
                    "isPresent": true
                  }
                ]
              }
            }
          ]
        }
      ]
    }
  ]
}
```

### Labels Object

| Field          | Type   | Description                                              |
|----------------|--------|----------------------------------------------------------|
| `displayName`  | string | The display name of the menu, section, or item           |
| `description`  | string | A description of the menu, section, or item              |
| `languageCode` | string | BCP 47 language code (e.g., `en`, `es`, `fr`)           |

### Item Object

| Field                 | Type   | Description                                                   |
|-----------------------|--------|---------------------------------------------------------------|
| `itemId`              | string | Unique identifier for the menu item                           |
| `labels`              | array  | Array of label objects with localized names and descriptions  |
| `price`               | object | Price object with `currencyCode`, `units`, and `nanos`        |
| `dietaryRestrictions` | array  | Array of dietary restriction enums                            |
| `allergenData`        | object | Object containing allergen information                        |

### Dietary Restriction Values

| Value            | Description                            |
|------------------|----------------------------------------|
| `VEGETARIAN`     | Suitable for vegetarians               |
| `VEGAN`          | Suitable for vegans                    |
| `GLUTEN_FREE`    | Does not contain gluten                |
| `HALAL`          | Prepared according to halal standards  |
| `KOSHER`         | Prepared according to kosher standards |
| `ORGANIC`        | Made with organic ingredients          |
| `DAIRY_FREE`     | Does not contain dairy products        |
| `NUT_FREE`       | Does not contain nuts                  |

### Allergen Types

| Value        | Description          |
|--------------|----------------------|
| `GLUTEN`     | Contains gluten      |
| `DAIRY`      | Contains dairy       |
| `EGGS`       | Contains eggs        |
| `NUTS`       | Contains tree nuts   |
| `PEANUTS`    | Contains peanuts     |
| `SOY`        | Contains soy         |
| `FISH`       | Contains fish        |
| `SHELLFISH`  | Contains shellfish   |
| `SESAME`     | Contains sesame      |
| `SULFITES`   | Contains sulfites    |

## Request Example (PUT)

### Partial Update with `updateMask`

```bash
curl -X PUT \
  "https://api.example.com/v1/accounts/123456/gmb-food-menus?updateMask=menus.sections.items" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "menus": [
      {
        "menuId": "menu-001",
        "sections": [
          {
            "sectionId": "section-appetizers",
            "items": [
              {
                "itemId": "item-001",
                "labels": [
                  {
                    "displayName": "Bruschetta",
                    "description": "Updated description with new ingredients",
                    "languageCode": "en"
                  }
                ],
                "price": {
                  "currencyCode": "USD",
                  "units": 10,
                  "nanos": 990000000
                }
              }
            ]
          }
        ]
      }
    ]
  }'
```

### Full Replacement (No `updateMask`)

```bash
curl -X PUT \
  "https://api.example.com/v1/accounts/123456/gmb-food-menus" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "menus": [
      {
        "menuId": "menu-001",
        "labels": [
          {
            "displayName": "Dinner Menu",
            "description": "Served daily from 5 PM to 10 PM",
            "languageCode": "en"
          }
        ],
        "sections": []
      }
    ]
  }'
```

## Response Example (GET)

```bash
curl -X GET \
  "https://api.example.com/v1/accounts/123456/gmb-food-menus" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

Returns the full menu object structure as described above.

## Error Responses

| Status Code | Description                                                                      |
|-------------|----------------------------------------------------------------------------------|
| `200`       | Success. Returns the menu object.                                                |
| `400`       | Bad Request. Invalid menu structure, unsupported `updateMask`, or malformed JSON.|
| `401`       | Unauthorized. Missing or invalid authentication token.                           |
| `403`       | Forbidden. The authenticated user does not have access to this account.          |
| `404`       | Not Found. The specified `accountId` does not exist.                             |
| `422`       | Unprocessable Entity. Validation errors in menu data (e.g., invalid price).      |
| `500`       | Internal Server Error. An unexpected error occurred on the server.               |

## Notes

- When using `updateMask`, only the specified fields are modified. All other fields remain unchanged.
- Omitting `updateMask` on a PUT request replaces the entire menu object.
- Price is represented using `units` (whole number) and `nanos` (fractional part in nanoseconds). For example, $9.99 is `units: 9, nanos: 990000000`.
- Multiple menus can exist under a single account (e.g., Breakfast, Lunch, Dinner).
- Labels support multiple languages via the `languageCode` field.