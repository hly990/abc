# Usage API

Retrieve current plan usage statistics and limits for the authenticated account.

## Endpoint

```
GET /v1/usage-stats
```

Returns the current plan information, billing period, and usage data for the authenticated user.

## Authentication

This endpoint requires a valid Bearer token. The usage stats returned correspond to the account associated with the authenticated token.

## Query Parameters

This endpoint does not accept any query parameters.

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
| `usage.lastReset`        | string  | ISO 8601 timestamp of the last usage reset (start of billing period)  |

## Request Example

```bash
curl -X GET \
  "https://api.example.com/v1/usage-stats" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

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

## Response Examples by Plan

### Free Plan

```json
{
  "planName": "Free",
  "billingPeriod": {
    "startDate": "2025-08-01",
    "endDate": "2025-08-31"
  },
  "signupDate": "2025-07-20",
  "limits": {
    "uploads": 50,
    "profiles": 3
  },
  "usage": {
    "uploads": 23,
    "profiles": 2,
    "lastReset": "2025-08-01T00:00:00Z"
  }
}
```

### Business Plan

```json
{
  "planName": "Business",
  "billingPeriod": {
    "startDate": "2025-08-01",
    "endDate": "2025-08-31"
  },
  "signupDate": "2023-11-02",
  "limits": {
    "uploads": 10000,
    "profiles": 100
  },
  "usage": {
    "uploads": 3421,
    "profiles": 47,
    "lastReset": "2025-08-01T00:00:00Z"
  }
}
```

### Enterprise Plan

```json
{
  "planName": "Enterprise",
  "billingPeriod": {
    "startDate": "2025-07-15",
    "endDate": "2025-08-14"
  },
  "signupDate": "2022-06-10",
  "limits": {
    "uploads": -1,
    "profiles": -1
  },
  "usage": {
    "uploads": 15892,
    "profiles": 312,
    "lastReset": "2025-07-15T00:00:00Z"
  }
}
```

**Note:** A value of `-1` for a limit indicates unlimited usage.

## Error Responses

| Status Code | Description                                                               |
|-------------|---------------------------------------------------------------------------|
| `200`       | Success. Returns the usage statistics.                                    |
| `401`       | Unauthorized. Missing or invalid authentication token.                    |
| `403`       | Forbidden. The authenticated user does not have access to usage data.     |
| `500`       | Internal Server Error. An unexpected error occurred on the server.        |

## Notes

- Usage counters (`uploads`) reset at the beginning of each billing period, as indicated by the `lastReset` field.
- The `profiles` usage count reflects the current number of connected profiles and does not reset with each billing period.
- The `billingPeriod` corresponds to the subscription cycle and may not always align with calendar months (e.g., for Enterprise plans with custom billing dates).
- A `limits` value of `-1` indicates the feature is unlimited on the current plan.
- This endpoint does not accept query parameters. It always returns data for the authenticated user's account.
- Usage data is updated in near real-time but may have a slight delay of up to a few minutes.
