---
name: tweetapi
description: >
  Third-party Twitter/X API for developers and researchers. Provides comprehensive access to tweets, users, DMs, communities, lists, spaces, and analytics.
  Use when: (1) Building Twitter automation, bots, or monitoring tools, (2) Fetching tweet/user data programmatically, (3) Implementing Twitter interactions (posting, liking, following), (4) Accessing encrypted DMs (X Chat) or legacy DMs.
---

# TweetAPI

Complete third-party API for Twitter/X with 67 endpoints covering all major Twitter functionality.

## Quick Start

**Base URL:** `https://api.tweetapi.com/tw-v2`

**Authentication:** All requests require `X-API-Key` header
```bash
curl -H "X-API-Key: YOUR_API_KEY" https://api.tweetapi.com/tw-v2/[endpoint]
```

**Rate Limits:**
- Basic: 60 requests/minute
- Pro: 120 requests/minute
- Business: 240 requests/minute

**Get API Key:** Sign up at https://www.tweetapi.com/

## Authentication Methods

### 1. API Key (Read-Only Access)
For fetching public data (tweets, users, followers, etc.):
```bash
curl -H "X-API-Key: YOUR_API_KEY" \
  "https://api.tweetapi.com/tw-v2/user/by-username?username=elonmusk"
```

### 2. Auth Token (Write Access)
For posting, liking, following, DMs - requires Twitter `auth_token` cookie:

**How to get authToken:**
1. Log into twitter.com in your browser
2. Open DevTools → Application → Cookies
3. Find `auth_token` cookie value
4. Use in API requests as `authToken` parameter

**OR use the login endpoint:**
```bash
curl -X POST "https://api.tweetapi.com/tw-v2/auth/login" \
  -H "X-API-Key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "yourUsername",
    "password": "yourPassword",
    "proxy": "proxy.example.com:8080@user:pass"
  }'
```

### 3. X Chat (Encrypted DMs)
Requires additional setup and `ct0` (CSRF token):
```bash
# Step 1: Setup X Chat
curl -X POST "https://api.tweetapi.com/tw-v2/xchat/setup" \
  -H "X-API-Key: YOUR_API_KEY" \
  -d '{
    "authToken": "YOUR_AUTH_TOKEN",
    "userId": "YOUR_USER_ID",
    "pin": "YOUR_XCHAT_PIN"
  }'

# Step 2: Send encrypted DM
curl -X POST "https://api.tweetapi.com/tw-v2/xchat/send" \
  -H "X-API-Key: YOUR_API_KEY" \
  -d '{
    "authToken": "YOUR_AUTH_TOKEN",
    "recipientId": "RECIPIENT_USER_ID",
    "message": "Hello!"
  }'
```

## Endpoint Categories (67 Total)

### USER (16 endpoints)
- Get user by username/ID (single or bulk)
- Get user tweets, replies, timeline
- Get followers, following, verified followers, subscriptions
- Check friendship status
- Account details and history

**Examples:**
```bash
# Get user profile
GET /tw-v2/user/by-username?username=elonmusk

# Get user tweets
GET /tw-v2/user/tweets?userId=44196397

# Get followers with pagination
GET /tw-v2/user/followers?userId=44196397&cursor=CURSOR_VALUE

# Get multiple users at once (max 200)
GET /tw-v2/user/by-usernames?usernames=elonmusk,SpaceX,Tesla
```

### TWEET (5 endpoints)
- Get tweet details (single or bulk up to 200)
- Get retweets and quote tweets
- Translate tweets

**Examples:**
```bash
# Get tweet details
GET /tw-v2/tweet/details?tweetId=1234567890

# Get multiple tweets (max 200)
GET /tw-v2/tweet/details-by-ids?ids=123,456,789

# Translate tweet to Spanish
POST /tw-v2/tweet/translate
{"tweetId": "1234567890", "dstLang": "es"}
```

### POST (6 endpoints)
- Create tweet (text, with media, quote tweet)
- Reply to tweet (text or with media)
- Delete tweet

**Examples:**
```bash
# Post a tweet
POST /tw-v2/interaction/create-post
{
  "authToken": "YOUR_AUTH_TOKEN",
  "text": "Hello, world!",
  "proxy": "proxy:port@user:pass"
}

# Quote tweet
POST /tw-v2/interaction/create-post-quote
{
  "authToken": "YOUR_AUTH_TOKEN",
  "text": "Interesting perspective!",
  "attachmentUrl": "https://x.com/username/status/1234567890",
  "proxy": "proxy:port@user:pass"
}

# Post with image
POST /tw-v2/interaction/create-post-with-media
{
  "authToken": "YOUR_AUTH_TOKEN",
  "text": "Check this out!",
  "media": [{"url": "https://example.com/image.jpg"}],
  "proxy": "proxy:port@user:pass"
}
```

### INTERACTION (12 endpoints)
- Like/unlike, retweet/unretweet
- Bookmark/unbookmark
- Follow/unfollow
- Add/remove from list
- Get notifications
- Get user analytics

**Examples:**
```bash
# Like a tweet
POST /tw-v2/interaction/like-post
{"authToken": "YOUR_AUTH_TOKEN", "tweetId": "1234567890"}

# Follow a user
POST /tw-v2/interaction/follow
{"authToken": "YOUR_AUTH_TOKEN", "userId": "44196397"}

# Get notifications
GET /tw-v2/interaction/notifications?authToken=YOUR_AUTH_TOKEN&timelineType=All
```

### LIST (4 endpoints)
- Get list details, tweets, members, followers

**Examples:**
```bash
# Get list tweets
GET /tw-v2/list/tweets?listId=1234567890

# Get list members
GET /tw-v2/list/members?listId=1234567890
```

### COMMUNITY (8 endpoints)
- Get community details, tweets, members
- Search communities
- Post to community (text or with media)
- Join/leave community

**Examples:**
```bash
# Search communities
GET /tw-v2/community/search?query=programming

# Post to community
POST /tw-v2/interaction/create-community-post
{
  "authToken": "YOUR_AUTH_TOKEN",
  "text": "Hello community!",
  "communityId": "1234567890",
  "proxy": "proxy:port@user:pass"
}

# Join community
POST /tw-v2/interaction/join-community
{"authToken": "YOUR_AUTH_TOKEN", "communityId": "1234567890"}
```

### SPACE (2 endpoints)
- Get Space details by ID
- Get Space audio stream URL

**Examples:**
```bash
# Get Space details
GET /tw-v2/space/by-id?spaceId=1MnxnPmLRQBGO

# Get stream URL
GET /tw-v2/space/stream-url?mediaKey=28_1991353658102149130
```

### SEARCH (1 endpoint)
- Search tweets, users, media

**Examples:**
```bash
# Search latest tweets
GET /tw-v2/search?query=javascript&type=Latest

# Search people
GET /tw-v2/search?query=developer&type=People

# Search photos
GET /tw-v2/search?query=sunset&type=Photos
```

### X CHAT - Encrypted DMs (5 endpoints)
- Setup X Chat encryption
- List conversations
- Send encrypted message
- Get conversation history (decrypted)
- Check DM permissions

**Examples:**
```bash
# Setup (required first)
POST /tw-v2/xchat/setup
{
  "authToken": "YOUR_AUTH_TOKEN",
  "userId": "YOUR_USER_ID",
  "pin": "123456"
}

# Send encrypted DM
POST /tw-v2/xchat/send
{
  "authToken": "YOUR_AUTH_TOKEN",
  "recipientId": "RECIPIENT_USER_ID",
  "message": "Encrypted hello!"
}

# Get conversation history (decrypted)
POST /tw-v2/xchat/history
{
  "authToken": "YOUR_AUTH_TOKEN",
  "conversationId": "userId1:userId2",
  "limit": 50
}
```

### UNENCRYPTED DM - Legacy (7 endpoints)
- Send DM (text or with media)
- Get inbox state, conversations
- Poll for updates
- Check DM permissions

**Examples:**
```bash
# Send DM
POST /tw-v2/interaction/send-dm
{
  "authToken": "YOUR_AUTH_TOKEN",
  "conversationId": "123456789-987654321",
  "text": "Hello!",
  "media": [{"url": "https://example.com/image.jpg"}],
  "proxy": "proxy:port@user:pass"
}

# Get inbox
GET /tw-v2/interaction/inbox-initial-state?authToken=YOUR_AUTH_TOKEN

# Get conversation
GET /tw-v2/interaction/conversation?authToken=YOUR_AUTH_TOKEN&conversationId=123-456
```

## Pagination

Most endpoints support cursor-based pagination:
```bash
# First request
GET /tw-v2/user/followers?userId=44196397

# Response includes cursor
{
  "data": [...],
  "cursor": "NEXT_CURSOR_VALUE"
}

# Next page
GET /tw-v2/user/followers?userId=44196397&cursor=NEXT_CURSOR_VALUE
```

## Proxies

Many write operations (posting, DMs, login) require or recommend proxies:
```json
{
  "proxy": "hostname:port@username:password"
}
```

**Why proxies?**
- Better success rates for automated actions
- Avoid IP-based rate limits
- Required for login endpoint

## Common Use Cases

### 1. Tweet Monitoring Bot
```bash
# Get user's latest tweets
GET /tw-v2/user/tweets?userId=USER_ID

# Get tweet engagement
GET /tw-v2/tweet/details?tweetId=TWEET_ID

# Search for keywords
GET /tw-v2/search?query=keyword&type=Latest
```

### 2. Auto-Reply Bot
```bash
# Monitor mentions (via notifications)
GET /tw-v2/interaction/notifications?authToken=TOKEN&timelineType=Mentions

# Reply to tweet
POST /tw-v2/interaction/reply-post
{
  "authToken": "TOKEN",
  "tweetId": "TWEET_ID",
  "text": "Thanks for reaching out!"
}
```

### 3. Community Manager
```bash
# Search communities
GET /tw-v2/community/search?query=web3

# Join community
POST /tw-v2/interaction/join-community
{"authToken": "TOKEN", "communityId": "ID"}

# Post to community
POST /tw-v2/interaction/create-community-post
{
  "authToken": "TOKEN",
  "communityId": "ID",
  "text": "Great discussion!"
}
```

### 4. Analytics Tracker
```bash
# Get user analytics
GET /tw-v2/interaction/user-analytics?authToken=TOKEN&granularity=Daily

# Track follower growth
GET /tw-v2/user/followers?userId=USER_ID

# Monitor engagement
GET /tw-v2/tweet/details?tweetId=TWEET_ID
```

### 5. DM Automation
```bash
# Get inbox
GET /tw-v2/interaction/inbox-initial-state?authToken=TOKEN

# Send DM
POST /tw-v2/interaction/send-dm
{
  "authToken": "TOKEN",
  "conversationId": "USER_ID_1-USER_ID_2",
  "text": "Thank you for following!"
}

# Poll for new messages
GET /tw-v2/interaction/dm-user-updates?authToken=TOKEN&cursor=CURSOR
```

## Important Notes

### Character Limits
- Non-premium users: 280 characters
- Premium users: 25,000 characters

### Media Limits
- Images: Up to 4 per tweet
- Videos: 1 per tweet
- DMs: 1 media item per message

### ID Formats
- User IDs: Numeric strings (e.g., "44196397")
- Tweet IDs: Numeric strings (e.g., "1234567890")
- Conversation IDs:
  - 1-on-1: "userId1-userId2"
  - Groups: Single ID string

### Response Format
All endpoints return JSON:
```json
{
  "data": {...},
  "cursor": "optional_pagination_cursor"
}
```

Errors:
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Error description"
  }
}
```

### Rate Limit Headers
- `X-RateLimit-Limit`: Total requests allowed per minute
- `X-RateLimit-Remaining`: Requests remaining
- `X-RateLimit-Reset`: Timestamp when limit resets
- HTTP 429: Rate limit exceeded

## Best Practices

1. **Always check rate limits** - Monitor response headers
2. **Use bulk endpoints** - Fetch up to 200 users/tweets at once when possible
3. **Implement pagination** - Don't miss data, use cursors
4. **Handle errors gracefully** - Implement retry logic with exponential backoff
5. **Use proxies for write operations** - Better success rates
6. **Cache user/tweet data** - Reduce API calls
7. **Respect Twitter's terms** - Don't spam, harass, or violate platform rules

## Error Handling

Common errors:
- `401 Unauthorized`: Invalid API key or auth token
- `403 Forbidden`: Action not allowed (e.g., protected account)
- `404 Not Found`: Resource doesn't exist
- `429 Too Many Requests`: Rate limit exceeded
- `500 Internal Server Error`: TweetAPI server issue

Recommended retry strategy:
```javascript
async function retryRequest(fn, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (error.status === 429) {
        // Wait for rate limit reset
        const resetTime = error.headers['x-ratelimit-reset'];
        await sleep(resetTime - Date.now());
      } else if (i === maxRetries - 1) {
        throw error;
      } else {
        // Exponential backoff
        await sleep(Math.pow(2, i) * 1000);
      }
    }
  }
}
```

## Support

- **Telegram:** @tweetapi
- **Email:** support@tweetapi.com
- **Status:** https://status.tweetapi.com
- **Pricing:** USD $1–$197/month (4 tiers)

## Legal

TweetAPI is a third-party service, **not affiliated with X Corp or Twitter**.

When using TweetAPI:
- Comply with Twitter's Terms of Service
- Respect user privacy
- Don't engage in spam or abuse
- Follow applicable laws and regulations
