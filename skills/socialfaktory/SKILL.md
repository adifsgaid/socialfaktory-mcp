---
name: socialfaktory
description: "Write, generate, schedule and publish a brand's social content, in its own voice, on TikTok, Instagram, YouTube, X, LinkedIn, Facebook and Pinterest, through the SocialFaktory MCP server."
homepage: https://www.socialfaktory.com/docs/mcp
metadata:
  {
    "openclaw":
      {
        "emoji": "🏭",
        "requires": { "bins": ["mcporter"] },
        "install":
          [
            {
              "id": "node",
              "kind": "node",
              "package": "mcporter",
              "bins": ["mcporter"],
              "label": "Install mcporter (node)",
            },
          ],
      },
  }
---

# SocialFaktory

SocialFaktory runs a brand's social media from one place. Through its MCP server you can read
the brands, channels and media a user already has, write posts in the brand's voice, price and
generate short video, compose one post per channel, schedule or send it, and read the metrics
back. You need a SocialFaktory account with at least one brand: https://www.socialfaktory.com

## Connect once

Add the server to `./config/mcporter.json`:

```json
{
  "mcpServers": {
    "socialfaktory": { "url": "https://www.socialfaktory.com/mcp" }
  }
}
```

Then sign in. A browser opens on the SocialFaktory consent screen, where the user picks the
permissions (read, generate, publish), an optional brand, a monthly credit cap and an expiry:

```bash
mcporter auth socialfaktory
mcporter list socialfaktory --schema
```

## Call tools

```bash
mcporter call socialfaktory.list_brands
mcporter call socialfaktory.list_channels brand_id=brand_...
mcporter call socialfaktory.generate_text --args '{"brand_id":"brand_...","brief":"Autumn launch","channel":"x","idempotency_key":"a-fresh-uuid"}'
```

Always read the schema first (`mcporter list socialfaktory --schema`) and pass arguments exactly
as it names them.

## Rules the user expects you to follow

- Start with `list_brands`. Reading is free.
- `create_generation` spends the user's credits. Call `quote_generation` first, show the price,
  and run it only after the user agrees. `generate_text` reserves 3 credits and settles what it
  used; say so before running it.
- `create_posts` makes drafts. Show the draft, then `send_post` queues it for publishing.
  `delete_post` withdraws a scheduled post; a published post stays online.
- Send a fresh `idempotency_key` on every spending or publishing call and reuse it only to
  retry that same call within 24 hours.
- Poll `get_generation` and `get_upload` every 2 to 5 seconds until they finish.
- A refusal comes back as an error envelope with a reason. Explain it; never retry blindly.

## What it cannot do

It cannot connect or disconnect a social channel, change the plan or buy credits, or act at all
once the user revokes it in Settings, API tokens. Cloning a video from a link, and generating
still images or carousels, are not available through an agent yet.

Docs: https://www.socialfaktory.com/docs/mcp. Support: hello@socialfaktory.com
