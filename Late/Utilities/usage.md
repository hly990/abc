# Usage API

Retrieve current plan usage statistics and limits for the authenticated account.

## Endpoint

```
GET /v1/usage-stats
```

Returns the current plan information, billing period, and usage data for the authenticated user.

## Authentication

This endpoint requires a valid Bearer token.

## Response Fields

| Field                    | Type    | Description                                                           |
|--------------------------|---------|-----------------------------------------------------------------------|
| `planName`               | string  | The name of the current subscription plan                             |
| `billingPeriod`          | object  | The current billing period start and end dates                        |
| `billingPeriod.startDate`| string  | ISO 8601 date of the billing period start                             |
| `billingPeriod.endDate`  | string  | ISO 8601 date of the billing period end                               |
| `signupDate`             | string  | ISO 8601 date when the account was created                            |
| `limits`                 | object  | The usage limits for the current plan                                 |
| `limits.uploads`         | integer | Maximum number of media uploads allowed per billing period            |
| `limits.profiles`        | integer | Maximum number of connected social profiles allowed                   |
| `usage`                  | object  | Current usage within the billing period                               |
| `usage.uploads`          | integer | Number of media uploads used in the current billing period            |
| `usage.profiles`         | integer | Number of social profiles currently connected                         |

## Response Example

```json
{
  "planName": "Professional",
  "billingPeriod": {
    "startDate": "2025-08-01",
    "endDate": "2025-08-31"
  },
  "signupDate": "2024-03-15",
  "limits": {
    "uploads": 2000,
    "profiles": 25
  },
  "usage": {
    "uploads": 847,
    "profiles": 12,
    "lastReset": "2025-08-01T00:00:00Z"
  }
}
```

## Error Responses

| Status Code | Description                                                               |
|-------------|---------------------------------------------------------------------------|
| `200`       | Success. Returns the usage statistics.                                    |
| `401`       | Unauthorized. Missing or invalid authentication token.                    |
| `403`       | Forbidden.                                                                |
| `500`       | Internal Server Error.                                                    |

## Notes

- A `limits` value of `-1` indicates the feature is unlimited on the current plan.
- Usage data is updated in near real-time but may have a slight delay of up to a few minutes.
