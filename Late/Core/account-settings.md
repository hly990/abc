# Account Settings - API Reference

## Overview
The Account Settings API provides endpoints for managing platform-specific account configurations, including Facebook Messenger persistent menus, Instagram ice breakers, and Telegram bot commands. These settings customize the user interaction experience on each platform.

---

## Facebook Persistent Menu

The Persistent Menu provides a set of always-available actions in Facebook Messenger conversations. It appears as a menu icon in the chat interface.

### Get Persistent Menu
**GET** `/v1/accounts/{accountId}/messenger-menu`

### Set Persistent Menu
**PUT** `/v1/accounts/{accountId}/messenger-menu`

### Delete Persistent Menu
**DELETE** `/v1/accounts/{accountId}/messenger-menu`

**Constraints:**
- Maximum 3 top-level menu items
- Menu item titles cannot exceed 30 characters
- Changes may take up to 24 hours to propagate to all users

---

## Instagram Ice Breakers

Ice Breakers are predefined questions or prompts displayed when a user opens a new conversation with your Instagram account.

### Get Ice Breakers
**GET** `/v1/accounts/{accountId}/instagram-ice-breakers`

### Set Ice Breakers
**PUT** `/v1/accounts/{accountId}/instagram-ice-breakers`

### Delete Ice Breakers
**DELETE** `/v1/accounts/{accountId}/instagram-ice-breakers`

**Constraints:**
- Maximum 4 ice breakers per account
- Question text cannot exceed 80 characters
- The account must be an Instagram Professional account

---

## Telegram Bot Commands

Bot commands are shortcuts that appear in the Telegram command menu (triggered by typing `/`).

### Get Bot Commands
**GET** `/v1/accounts/{accountId}/telegram-commands`

### Set Bot Commands
**PUT** `/v1/accounts/{accountId}/telegram-commands`

### Delete Bot Commands
**DELETE** `/v1/accounts/{accountId}/telegram-commands`

**Constraints:**
- Maximum 100 commands per bot
- Command names: 1-32 characters, lowercase Latin letters, digits, and underscores only
- Command descriptions: 3-256 characters

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
