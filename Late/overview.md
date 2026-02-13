# Late API Documentation - Overview

## Service Description

Late is a social media scheduling platform offering API access for managing content across 13+ platforms. The service provides "complete API docs for posting, scheduling, analytics, and inbox management" across major social networks.

## Supported Platforms

The API supports the following social media channels:

- Twitter/X
- Instagram
- Facebook
- LinkedIn
- TikTok
- YouTube
- Pinterest
- Reddit
- Bluesky
- Threads
- Google Business
- Telegram
- Snapchat

## Core Capabilities

**Content Management:**
- Schedule posts across multiple platforms simultaneously
- Upload media including images, videos, and documents
- Manage multiple accounts organized into profiles
- Configure posting queues with recurring time slots

**Analytics & Engagement:**
- Track post performance metrics across platforms
- Access unified inbox for DMs, comments, and reviews
- Manage account settings per platform
- View publishing logs for transparency

**Team Features:**
- Invite team members for collaboration
- Manage user permissions and API keys
- Organize accounts into groups

## Key Concepts

**Profiles** - Containers grouping social media accounts by brand or project

**Accounts** - Individual connected social media profiles

**Posts** - Content scheduled for publication across selected platforms

**Queue** - Optional automated scheduling system with recurring time slots

## API Details

**Base URL:** `https://getlate.dev/api/v1`

**Rate Limiting** - Based on subscription tier:
- Free: 60 requests/minute
- Build: 120 requests/minute
- Accelerate: 600 requests/minute
- Unlimited: 1,200 requests/minute

## Getting Started

The documentation recommends three initial steps: authentication setup, creating first posts, and exploring platform-specific features. SDKs are available for multiple programming languages, and OpenAPI specifications can be downloaded for API-first development.
