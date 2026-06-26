---
name: socialsyncs
description: SocialSyncs is a tool to schedule social media and chat posts to 28+ channels X, LinkedIn, LinkedIn Page, Reddit, Instagram, Facebook Page, Threads, YouTube, Google My Business, TikTok, Pinterest, Dribbble, Discord, Slack, Kick, Twitch, Mastodon, Bluesky, Lemmy, Farcaster, Telegram, Nostr, VK, Medium, Dev.to, Hashnode, WordPress, ListMonk
homepage: https://docs.socialsyncs.co/public-api/introduction
metadata: {"clawdbot":{"emoji":"🌎","requires":{"bins":[],"env":["SOCIALSYNCS_API_KEY"]}}}
---

# SocialSyncs Skill

SocialSyncs is a tool to schedule social media and chat posts to 28+ channels:

X, LinkedIn, LinkedIn Page, Reddit, Instagram, Facebook Page, Threads, YouTube, Google My Business, TikTok, Pinterest, Dribbble, Discord, Slack, Kick, Twitch, Mastodon, Bluesky, Lemmy, Farcaster, Telegram, Nostr, VK, Medium, Dev.to, Hashnode, WordPress, ListMonk

## Setup

1. Get your API key: https://app.socialsyncs.co/settings
2. Click on "Settings"
3. Click "Reveal"
4. Set environment variables:
   ```bash
   export SOCIALSYNCS_API_KEY="your-api-key"
   ```

## Get all added channels

```bash
curl -X GET "https://app.socialsyncs.co/api/public/v1/integrations" \
  -H "Authorization: $SOCIALSYNCS_API_KEY"
```

## Get the next available slot for a channel

```bash
curl -X GET "https://app.socialsyncs.co/api/public/v1/find-slot/:id" \
  -H "Authorization: $SOCIALSYNCS_API_KEY"
```

## Upload a new file (form-data)

```bash
curl -X POST "https://app.socialsyncs.co/api/public/v1/upload" \
  -H "Authorization: $SOCIALSYNCS_API_KEY" \
  -F "file=@/path/to/your/file.png"
```

## Upload a new file from an existing URL

```bash
curl -X POST "https://app.socialsyncs.co/api/public/v1/upload-from-url" \
  -H "Authorization: $SOCIALSYNCS_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com/image.png"
  }'
```

## Post list

```bash
curl -X GET "https://app.socialsyncs.co/api/public/v1/posts?startDate=2024-12-14T08:18:54.274Z&endDate=2024-12-14T08:18:54.274Z&customer=optionalCustomerId" \
  -H "Authorization: $SOCIALSYNCS_API_KEY"
```

## Schedule a new post

Settings for different channels can be found in:
https://docs.socialsyncs.co/public-api/introduction
On the bottom left menu

When scheduling a new posts, if you attach media, you must upload it first and use the url of the uploaded media.
Upload URL must contain: uploads.socialsyncs.co.

```bash
curl -X POST "https://app.socialsyncs.co/api/public/v1/posts" \
  -H "Authorization: $SOCIALSYNCS_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "type": "schedule",
  "date": "2024-12-14T10:00:00.000Z",
  "shortLink": false,
  "tags": [],
  "posts": [
    {
      "integration": {
        "id": "your-x-integration-id"
      },
      "value": [
        {
          "content": "Hello from the SocialSyncs API! 🚀",
          "image": [{ "id": "img-123", "path": "https://uploads.socialsyncs.co/photo.jpg" }]
        }
      ],
      "settings": {
        "__type": "provider name",
        rest of the settings
      }
    }
  ]
}'
```

## Delete a post

```bash
curl -X DELETE "https://app.socialsyncs.co/api/public/v1/posts/:id" \
  -H "Authorization: $SOCIALSYNCS_API_KEY"
```
