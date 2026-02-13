# Account Settings - API Reference

## Overview
The Account Settings API provides endpoints for managing platform-specific account configurations, including Facebook Messenger persistent menus, Instagram ice breakers, and Telegram bot commands. These settings customize the user interaction experience on each platform.

---

## Facebook Persistent Menu

The Persistent Menu provides a set of always-available actions in Facebook Messenger conversations. It appears as a menu icon in the chat interface.

### Get Persistent Menu
**GET** `/v1/accounts/{accountId}/messenger-menu`

Retrieves the current persistent menu configuration for a Facebook Messenger account.

**Path Parameters:**
- `accountId` (string, required): The Facebook account ID

**Response (200):**
```json
{
  "accountId": "64f0a1b2c3d4e5f6a7b8c9d0",
  "platform": "facebook",
  "menu": {
    "callToActions": [
      {
        "type": "postback",
        "title": "Get Started",
        "payload": "GET_STARTED"
      },
      {
        "type": "web_url",
        "title": "Visit Website",
        "url": "https://example.com"
      },
      {
        "type": "postback",
        "title": "Contact Support",
        "payload": "CONTACT_SUPPORT"
      }
    ],
    "locale": "default"
  }
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Account not found or no menu configured

---

### Set Persistent Menu
**PUT** `/v1/accounts/{accountId}/messenger-menu`

Creates or replaces the persistent menu for a Facebook Messenger account.

**Path Parameters:**
- `accountId` (string, required): The Facebook account ID

**Request Body:**
- `callToActions` (array of objects, required): Menu items (minimum 1, maximum 3)
  - `type` (string, required): Item type - `postback` or `web_url`
  - `title` (string, required): Display text for the menu item (max 30 characters)
  - `payload` (string, conditional): Required when `type` is `postback`. The payload string sent when the item is tapped.
  - `url` (string, conditional): Required when `type` is `web_url`. The URL to open.
- `locale` (string, optional): Locale for the menu (default: `default`). Use ISO locale codes for localized menus.

**Request Example:**
```json
{
  "callToActions": [
    {
      "type": "postback",
      "title": "Get Started",
      "payload": "GET_STARTED"
    },
    {
      "type": "web_url",
      "title": "Shop Now",
      "url": "https://example.com/shop"
    },
    {
      "type": "postback",
      "title": "Help",
      "payload": "HELP_MENU"
    }
  ]
}
```

**Constraints:**
- Maximum 3 top-level menu items
- Menu item titles cannot exceed 30 characters
- The account must be a Facebook Page with Messenger enabled
- Changes may take up to 24 hours to propagate to all users

**Response (200):**
```json
{
  "message": "Persistent menu updated successfully",
  "menu": {
    "callToActions": [
      {
        "type": "postback",
        "title": "Get Started",
        "payload": "GET_STARTED"
      },
      {
        "type": "web_url",
        "title": "Shop Now",
        "url": "https://example.com/shop"
      },
      {
        "type": "postback",
        "title": "Help",
        "payload": "HELP_MENU"
      }
    ],
    "locale": "default"
  }
}
```

**Error Responses:**
- **400:** Invalid request body (too many items, title too long, missing required fields)
- **401:** Unauthorized access
- **403:** Account does not support persistent menus
- **404:** Account not found

---

### Delete Persistent Menu
**DELETE** `/v1/accounts/{accountId}/messenger-menu`

Removes the persistent menu from a Facebook Messenger account.

**Path Parameters:**
- `accountId` (string, required): The Facebook account ID

**Response (200):**
```json
{
  "message": "Persistent menu deleted successfully",
  "accountId": "64f0a1b2c3d4e5f6a7b8c9d0"
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Account not found or no menu configured

---

## Instagram Ice Breakers

Ice Breakers are predefined questions or prompts displayed when a user opens a new conversation with your Instagram account. They help guide the initial interaction.

### Get Ice Breakers
**GET** `/v1/accounts/{accountId}/instagram-ice-breakers`

Retrieves the current ice breaker configuration for an Instagram account.

**Path Parameters:**
- `accountId` (string, required): The Instagram account ID

**Response (200):**
```json
{
  "accountId": "64f0b2c3d4e5f6a7b8c9d0e1",
  "platform": "instagram",
  "iceBreakers": [
    {
      "question": "What products do you offer?",
      "payload": "PRODUCTS_INQUIRY"
    },
    {
      "question": "What are your business hours?",
      "payload": "BUSINESS_HOURS"
    },
    {
      "question": "I need help with an order",
      "payload": "ORDER_HELP"
    },
    {
      "question": "Tell me about your services",
      "payload": "SERVICES_INFO"
    }
  ]
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Account not found or no ice breakers configured

---

### Set Ice Breakers
**PUT** `/v1/accounts/{accountId}/instagram-ice-breakers`

Creates or replaces the ice breakers for an Instagram account.

**Path Parameters:**
- `accountId` (string, required): The Instagram account ID

**Request Body:**
- `iceBreakers` (array of objects, required): Ice breaker prompts (minimum 1, maximum 4)
  - `question` (string, required): The prompt text displayed to users (max 80 characters)
  - `payload` (string, required): The payload string sent when the user taps the prompt

**Request Example:**
```json
{
  "iceBreakers": [
    {
      "question": "What products do you offer?",
      "payload": "PRODUCTS_INQUIRY"
    },
    {
      "question": "What are your business hours?",
      "payload": "BUSINESS_HOURS"
    },
    {
      "question": "I need help with my order",
      "payload": "ORDER_HELP"
    },
    {
      "question": "How can I contact support?",
      "payload": "CONTACT_SUPPORT"
    }
  ]
}
```

**Constraints:**
- Maximum 4 ice breakers per account
- Question text cannot exceed 80 characters
- The account must be an Instagram Professional account (Business or Creator)
- Payloads must be unique within the ice breaker set

**Response (200):**
```json
{
  "message": "Ice breakers updated successfully",
  "iceBreakers": [
    {
      "question": "What products do you offer?",
      "payload": "PRODUCTS_INQUIRY"
    },
    {
      "question": "What are your business hours?",
      "payload": "BUSINESS_HOURS"
    },
    {
      "question": "I need help with my order",
      "payload": "ORDER_HELP"
    },
    {
      "question": "How can I contact support?",
      "payload": "CONTACT_SUPPORT"
    }
  ]
}
```

**Error Responses:**
- **400:** Invalid request body (too many ice breakers, question too long, duplicate payloads)
- **401:** Unauthorized access
- **403:** Account does not support ice breakers (not a Professional account)
- **404:** Account not found

---

### Delete Ice Breakers
**DELETE** `/v1/accounts/{accountId}/instagram-ice-breakers`

Removes all ice breakers from an Instagram account.

**Path Parameters:**
- `accountId` (string, required): The Instagram account ID

**Response (200):**
```json
{
  "message": "Ice breakers deleted successfully",
  "accountId": "64f0b2c3d4e5f6a7b8c9d0e1"
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Account not found or no ice breakers configured

---

## Telegram Bot Commands

Bot commands are shortcuts that appear in the Telegram command menu (triggered by typing `/`). They help users discover available bot functionality.

### Get Bot Commands
**GET** `/v1/accounts/{accountId}/telegram-commands`

Retrieves the current bot command list for a Telegram bot account.

**Path Parameters:**
- `accountId` (string, required): The Telegram bot account ID

**Response (200):**
```json
{
  "accountId": "64f0c3d4e5f6a7b8c9d0e1f2",
  "platform": "telegram",
  "commands": [
    {
      "command": "start",
      "description": "Start interacting with the bot"
    },
    {
      "command": "help",
      "description": "Show available commands and usage"
    },
    {
      "command": "subscribe",
      "description": "Subscribe to updates and notifications"
    },
    {
      "command": "settings",
      "description": "Manage your preferences"
    }
  ]
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Account not found or no commands configured

---

### Set Bot Commands
**PUT** `/v1/accounts/{accountId}/telegram-commands`

Creates or replaces the bot command list for a Telegram bot account.

**Path Parameters:**
- `accountId` (string, required): The Telegram bot account ID

**Request Body:**
- `commands` (array of objects, required): Bot commands (minimum 1, maximum 100)
  - `command` (string, required): The command name without the leading `/` (1-32 characters, lowercase letters, digits, and underscores only)
  - `description` (string, required): Description of the command (3-256 characters)

**Request Example:**
```json
{
  "commands": [
    {
      "command": "start",
      "description": "Start interacting with the bot"
    },
    {
      "command": "help",
      "description": "Show available commands and usage"
    },
    {
      "command": "subscribe",
      "description": "Subscribe to daily updates"
    },
    {
      "command": "unsubscribe",
      "description": "Unsubscribe from updates"
    },
    {
      "command": "feedback",
      "description": "Send feedback to the team"
    }
  ]
}
```

**Constraints:**
- Maximum 100 commands per bot
- Command names: 1-32 characters, lowercase Latin letters, digits, and underscores only
- Command descriptions: 3-256 characters
- Command names must be unique within the set
- Commands are updated immediately via the Telegram Bot API

**Response (200):**
```json
{
  "message": "Bot commands updated successfully",
  "commands": [
    {
      "command": "start",
      "description": "Start interacting with the bot"
    },
    {
      "command": "help",
      "description": "Show available commands and usage"
    },
    {
      "command": "subscribe",
      "description": "Subscribe to daily updates"
    },
    {
      "command": "unsubscribe",
      "description": "Unsubscribe from updates"
    },
    {
      "command": "feedback",
      "description": "Send feedback to the team"
    }
  ]
}
```

**Error Responses:**
- **400:** Invalid request body (invalid command name format, description too short/long, duplicate commands)
- **401:** Unauthorized access
- **403:** Account is not a Telegram bot
- **404:** Account not found

---

### Delete Bot Commands
**DELETE** `/v1/accounts/{accountId}/telegram-commands`

Removes all bot commands from a Telegram bot account, clearing the command menu.

**Path Parameters:**
- `accountId` (string, required): The Telegram bot account ID

**Response (200):**
```json
{
  "message": "Bot commands deleted successfully",
  "accountId": "64f0c3d4e5f6a7b8c9d0e1f2"
}
```

**Error Responses:**
- **401:** Unauthorized access
- **404:** Account not found or no commands configured

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters or body |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Feature not supported for this account type |
| **404** | Account not found or settings not configured |
| **429** | Rate limit exceeded |
| **500** | Internal server error |
