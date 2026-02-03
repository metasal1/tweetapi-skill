# TweetAPI Skill for Claude Code

A Claude Code skill that provides comprehensive access to Twitter/X functionality through the TweetAPI third-party API. Build Twitter automation, bots, monitoring tools, and integrate Twitter functionality into your workflows.

## Features

- **67 API endpoints** covering all major Twitter functionality
- **User operations:** Get profiles, tweets, followers, following, verified followers
- **Tweet operations:** Fetch details, retweets, quote tweets, translate tweets
- **Post operations:** Create tweets, quote tweets, replies, post with media
- **Interactions:** Like, retweet, bookmark, follow, unfollow
- **Communities:** Search, join, post, manage communities
- **Lists:** Get list details, tweets, members, followers
- **Spaces:** Get Space details and audio streams
- **Direct Messages:** Both encrypted (X Chat) and legacy unencrypted DMs
- **Search:** Search tweets, users, media with filters
- **Analytics:** Track user analytics and engagement

## Installation

```bash
# Install the skill using Claude Code
/install https://github.com/metasal1/tweetapi-skill
```

## Usage

Once installed, Claude Code can automatically use this skill when you ask questions or request actions related to Twitter/X:

```
"Help me build a Twitter bot that monitors mentions"
"Fetch the latest tweets from @elonmusk"
"Show me how to post a tweet with an image using TweetAPI"
"Create a script to track follower growth"
```

## Quick Start

### Authentication

TweetAPI uses two authentication methods:

1. **API Key** (read-only): For fetching public data
2. **Auth Token** (write access): For posting, liking, following, DMs

Get your API key from [tweetapi.com](https://www.tweetapi.com/)

### Example: Fetch User Profile

```bash
curl -H "X-API-Key: YOUR_API_KEY" \
  "https://api.tweetapi.com/tw-v2/user/by-username?username=elonmusk"
```

### Example: Post a Tweet

```bash
curl -X POST "https://api.tweetapi.com/tw-v2/interaction/create-post" \
  -H "X-API-Key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "authToken": "YOUR_AUTH_TOKEN",
    "text": "Hello from TweetAPI!"
  }'
```

## Documentation

Full documentation is available in [SKILL.md](./SKILL.md), including:

- Complete endpoint reference (67 endpoints)
- Authentication methods and setup
- Pagination and rate limits
- Common use cases and examples
- Best practices and error handling

## Use Cases

- **Tweet Monitoring:** Track mentions, keywords, user activity
- **Auto-Reply Bots:** Respond to mentions and DMs automatically
- **Community Management:** Post and engage in Twitter communities
- **Analytics Tracking:** Monitor follower growth and engagement
- **Content Automation:** Schedule and post tweets programmatically
- **DM Automation:** Send automated direct messages
- **Whale Wallet Tracking:** Monitor crypto influencers and traders

## Rate Limits

- **Basic:** 60 requests/minute
- **Pro:** 120 requests/minute
- **Business:** 240 requests/minute

## Requirements

- TweetAPI account and API key ([sign up here](https://www.tweetapi.com/))
- Claude Code CLI

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

## Related Skills

- [solana-dev](https://github.com/metasal1/solana-dev-skill) - Solana blockchain development
- [whale-wallet-analysis](https://github.com/metasal1/whale-wallet-analysis-skill) - Track crypto whale wallets
- [social-signal-bot](https://github.com/metasal1/social-signal-bot-skill) - Social media monitoring

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

**Base URL:** `https://api.tweetapi.com/tw-v2`

**API Documentation:** [SKILL.md](./SKILL.md)
