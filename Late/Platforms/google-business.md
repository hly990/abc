# Google Business API - Late API Documentation

## Overview

The Google Business API enables posting to Google Business Profiles with support for text, images, and call-to-action buttons.

**Key Limitation:** Videos are not supported on this platform.

## Quick Start

Create a basic Google Business Profile post:

```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "🎉 We are open this holiday weekend! Stop by for our special seasonal menu.",
    "mediaItems": [
      {"type": "image", "url": "https://example.com/holiday-special.jpg"}
    ],
    "platforms": [
      {"platform": "googlebusiness", "accountId": "YOUR_ACCOUNT_ID"}
    ],
    "publishNow": true
  }'
```

## Image Requirements

| Property | Requirement |
|----------|-------------|
| Max Images | 1 per post |
| Formats | JPEG, PNG |
| Max File Size | 5 MB |
| Min Dimensions | 250 × 250 px |
| Recommended | 1200 × 900 px (4:3) |

### Aspect Ratios

| Ratio | Dimensions | Notes |
|-------|-----------|-------|
| 4:3 | 1200 × 900 px | **Recommended** |
| 1:1 | 1080 × 1080 px | Square, good for profile |
| 16:9 | 1200 × 675 px | Landscape |

**Note:** Google may crop images. Use 4:3 for best results.

## Call-to-Action Buttons

Add CTA buttons to drive user engagement:

```bash
curl -X POST https://getlate.dev/api/v1/posts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Book your appointment today! Limited spots available this week.",
    "mediaItems": [
      {"type": "image", "url": "https://example.com/booking.jpg"}
    ],
    "platforms": [{
      "platform": "googlebusiness",
      "accountId": "YOUR_ACCOUNT_ID",
      "platformSpecificData": {
        "callToAction": {
          "type": "BOOK",
          "url": "https://mybusiness.com/book"
        }
      }
    }],
    "publishNow": true
  }'
```

### Available CTA Types

| Type | Description | Best For |
|------|-------------|----------|
| `LEARN_MORE` | Link to more information | Articles, about pages |
| `BOOK` | Booking/reservation link | Services, appointments |
| `ORDER` | Online ordering link | Restaurants, food |
| `SHOP` | E-commerce link | Retail, products |
| `SIGN_UP` | Registration link | Events, newsletters |
| `CALL` | Phone call action | Contact, inquiries |

## Text-Only Posts

Posts without images are supported:

```json
{
  "content": "Happy Friday! 🎉 We're offering 20% off all services this weekend.",
  "platforms": [
    { "platform": "googlebusiness", "accountId": "acc_123" }
  ]
}
```

## Image URL Requirements

- **Public URL:** Must be publicly accessible
- **HTTPS:** Secure URLs only required
- **No redirects:** Direct link to image needed
- **No authentication:** Cannot require login

## Multi-Location Posting

Retrieve available locations:

```bash
curl -X GET https://getlate.dev/api/v1/accounts/YOUR_ACCOUNT_ID/gmb-locations \
  -H "Authorization: Bearer YOUR_API_KEY"
```

Post to multiple locations:

```json
{
  "content": "Now open at all locations! Visit us today 🎉",
  "mediaItems": [
    {"type": "image", "url": "https://example.com/store.jpg"}
  ],
  "platforms": [
    {
      "platform": "googlebusiness",
      "accountId": "YOUR_ACCOUNT_ID",
      "platformSpecificData": {
        "locationId": "locations/111111111"
      }
    },
    {
      "platform": "googlebusiness",
      "accountId": "YOUR_ACCOUNT_ID",
      "platformSpecificData": {
        "locationId": "locations/222222222"
      }
    }
  ],
  "publishNow": true
}
```

## Post Visibility

Posts appear on:
- Your Google Business Profile
- Google Search (when searching your business)
- Google Maps
- Google Knowledge Panel

## Character Limits

| Property | Limit |
|----------|-------|
| Post text | 1500 characters |
| CTA URL | Standard URL length |

## Common Issues

**Image not found:** Verify URL is publicly accessible, check for authentication requirements, ensure HTTPS, test URL in incognito browser

**Invalid image format:** Use JPEG or PNG only; WebP and GIF not supported

**Image too small:** Minimum 250 × 250 px required; 1200 × 900 px recommended

**Post not appearing:** Posts may take 24-48 hours to appear; check Google Business Console; ensure account is verified

**CTA not working:** Verify URL validity and accessibility; use HTTPS; avoid shortened URLs

## Inbox Management

Requires Inbox add-on ($1/social set/month)

### Reviews (Supported Features)
- ✅ List reviews
- ✅ Reply to reviews
- ✅ Delete reply

### Limitations
- **No DMs:** Google Business lacks messaging API access
- **No comments:** Posts don't support comments

## Food Menus

Manage restaurant menus:

```bash
curl -X GET https://getlate.dev/api/v1/accounts/YOUR_ACCOUNT_ID/gmb-food-menus \
  -H "Authorization: Bearer YOUR_API_KEY"
```

Update menus with pricing, dietary restrictions, and allergen information.

## Location Details

Read and update business information:

```bash
curl -X GET "https://getlate.dev/api/v1/accounts/YOUR_ACCOUNT_ID/gmb-location-details?readMask=regularHours,specialHours,profile,websiteUri" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

Manage: hours, special hours, description, phone numbers, website

## Media Management

Upload and manage photos:

```bash
curl -X POST https://getlate.dev/api/v1/accounts/YOUR_ACCOUNT_ID/gmb-media \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "sourceUrl": "https://example.com/photos/interior.jpg",
    "description": "Dining area with outdoor seating",
    "category": "INTERIOR"
  }'
```

**Photo categories:** COVER, PROFILE, LOGO, EXTERIOR, INTERIOR, FOOD_AND_DRINK, MENU, PRODUCT, TEAMS, ADDITIONAL

## Attributes

Manage business amenities and services:

```bash
curl -X PUT https://getlate.dev/api/v1/accounts/YOUR_ACCOUNT_ID/gmb-attributes \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "attributes": [
      {"name": "has_delivery", "values": [true]},
      {"name": "has_outdoor_seating", "values": [true]}
    ],
    "attributeMask": "has_delivery,has_outdoor_seating"
  }'
```

**Common attributes:** has_dine_in, has_takeout, has_delivery, has_wifi, has_outdoor_seating, pay_credit_card_types_accepted

## Place Actions

Manage booking and ordering buttons:

```bash
curl -X POST https://getlate.dev/api/v1/accounts/YOUR_ACCOUNT_ID/gmb-place-actions \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "uri": "https://order.ubereats.com/mybusiness",
    "placeActionType": "FOOD_ORDERING"
  }'
```

**Action types:** APPOINTMENT, ONLINE_APPOINTMENT, DINING_RESERVATION, FOOD_ORDERING, FOOD_DELIVERY, FOOD_TAKEOUT, SHOP_ONLINE

## Related API Endpoints

- Connect Google Business Account
- Create Post
- Upload Media
- GMB Reviews
- GMB Food Menus
- GMB Location Details
- GMB Media
- GMB Attributes
- GMB Place Actions
- Reviews
