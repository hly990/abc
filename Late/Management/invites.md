# Invites - API Reference

## Overview
The Invites API provides endpoints for generating invitation tokens to invite new users to the Late platform. Invitations can be scoped to specific profiles and roles, controlling what access the invited user receives upon joining.

---

## Endpoints

### Create Invite Token
**POST** `/v1/invite/tokens`

Generates a new invitation token and URL for inviting a user to the platform.

**Request Body:**
- `email` (string, required): Email address of the user to invite
- `role` (string, required): Role to assign to the invited user - `admin`, `editor`, `viewer`
- `profileIds` (array of strings, optional): Profile IDs the invited user will have access to. If omitted, the user will have access to all profiles.
- `scope` (object, optional): Fine-grained permission scope for the invitation
  - `canPublish` (boolean, optional): Allow the user to publish posts (default: based on role)
  - `canApprove` (boolean, optional): Allow the user to approve posts (default: based on role)
  - `canManageAccounts` (boolean, optional): Allow the user to connect/disconnect social accounts (default: based on role)
  - `canViewAnalytics` (boolean, optional): Allow the user to view analytics (default: `true`)
  - `canManageInbox` (boolean, optional): Allow the user to manage messages, comments, and reviews (default: based on role)
- `expiresIn` (integer, optional): Token expiration time in hours (default: `72`, max: `720` / 30 days)
- `message` (string, optional): Custom message included in the invitation email (max 500 characters)

**Request Example:**
```json
{
  "email": "newmember@example.com",
  "role": "editor",
  "profileIds": ["6507a1b2c3d4e5f6a7b8c9d0"],
  "scope": {
    "canPublish": true,
    "canApprove": false,
    "canManageAccounts": false,
    "canViewAnalytics": true,
    "canManageInbox": true
  },
  "expiresIn": 168,
  "message": "Welcome to the team! You've been invited to help manage our social media content."
}
```

**Response (201):**
```json
{
  "message": "Invitation created successfully",
  "invite": {
    "_id": "inv_a1b2c3d4e5f6",
    "email": "newmember@example.com",
    "role": "editor",
    "profileIds": ["6507a1b2c3d4e5f6a7b8c9d0"],
    "scope": {
      "canPublish": true,
      "canApprove": false,
      "canManageAccounts": false,
      "canViewAnalytics": true,
      "canManageInbox": true
    },
    "token": "inv_tok_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6",
    "inviteUrl": "https://app.late.com/invite/inv_tok_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6",
    "status": "pending",
    "expiresAt": "2024-11-22T10:00:00Z",
    "invitedBy": {
      "userId": "user_abc123",
      "name": "Jane Owner",
      "email": "owner@example.com"
    },
    "createdAt": "2024-11-15T10:00:00Z"
  }
}
```

**Scope Parameter Details:**

The `scope` object provides fine-grained control over what the invited user can do, independent of their role. When a scope field is omitted, the default permission for the assigned role applies.

| Scope Field | Admin Default | Editor Default | Viewer Default | Description |
|-------------|---------------|----------------|----------------|-------------|
| `canPublish` | `true` | `true` | `false` | Ability to create and publish posts directly |
| `canApprove` | `true` | `false` | `false` | Ability to approve posts submitted by others |
| `canManageAccounts` | `true` | `false` | `false` | Ability to connect, disconnect, and manage social accounts |
| `canViewAnalytics` | `true` | `true` | `true` | Ability to view analytics and reporting dashboards |
| `canManageInbox` | `true` | `true` | `false` | Ability to read and respond to messages, comments, and reviews |

**Important Notes:**
- The `owner` role cannot be assigned via invitations. Only one owner exists per organization.
- Scope permissions cannot exceed the role's default permissions. For example, a `viewer` with `canPublish: true` will be rejected.
- An invitation email is automatically sent to the specified email address.
- The invitation token and URL can be shared manually if email delivery fails.

**Error Responses:**
- **400:** Invalid request body (missing email, invalid role, invalid scope, email format invalid)
- **401:** Unauthorized access
- **403:** Insufficient permissions to invite users (requires admin or owner role)
- **404:** One or more profile IDs not found
- **409:** An active invitation already exists for this email address
- **422:** Scope permissions exceed role defaults, or expiration exceeds maximum

---

## Invitation Lifecycle

1. **Created:** An invitation token is generated and the invitation email is sent.
2. **Pending:** The invitation is active and awaiting the recipient to accept.
3. **Accepted:** The recipient clicked the invite link and created their account.
4. **Expired:** The invitation expired without being accepted.
5. **Revoked:** The invitation was manually revoked before being accepted.

---

## Error Responses Summary

| Status Code | Description |
|-------------|-------------|
| **400** | Invalid request parameters or body |
| **401** | Unauthorized - invalid or expired API key |
| **403** | Insufficient permissions (requires admin or owner role) |
| **404** | Profile not found |
| **409** | Active invitation already exists for this email |
| **422** | Scope exceeds role defaults or invalid expiration |
| **429** | Rate limit exceeded |
| **500** | Internal server error |

---

## Notes

- Each email address can only have one active (pending) invitation at a time. To resend, the existing invitation must expire or be revoked first.
- Invitation tokens are single-use and become invalid after acceptance.
- The `inviteUrl` is a fully qualified URL that the recipient can open in their browser to accept the invitation and create their account.
- The invitation email is sent from `noreply@late.com` and includes the inviter's name and optional custom message.
- Rate limit: Maximum 20 invitations per hour per user.

---
