---
name: socialfaktory
description: Schedule and publish social media posts with the SocialFaktory MCP server: write in a brand's voice, generate short video, and post to TikTok, Instagram, YouTube, X, LinkedIn, Facebook and Pinterest. Use when the user wants to draft, generate, schedule or publish a brand's social content.
---

# SocialFaktory

SocialFaktory runs a brand's social media from one place. Through its MCP server you can read
the brands, channels and media a user already has, write posts in the brand's voice, price and
generate short video, compose one post per channel, schedule or send it, and read the metrics
back. The user needs a SocialFaktory account with at least one brand. Reading is free;
generating and publishing need an active SocialFaktory plan.

If the socialfaktory tools are not available, ask the user to run `/mcp` and sign in. The
sign-in happens in the user's browser. Never ask the user to paste a password or token into the
chat.

## Rules the user expects you to follow

- Start with `list_brands`. Reading is free.
- `create_generation` spends the user's credits. Call `quote_generation` first, show the price,
  and run it only after the user agrees. `generate_text` reserves 3 credits and settles what it
  used; say so before running it.
- `brand_id` goes inside `workflow` for `quote_generation` and `create_generation`, and at the
  top level for `generate_text` and `get_text_generation`. A token pinned to one brand fills it
  in wherever the schema marks it optional.
- `create_posts` makes drafts. Show the draft, then `send_post` queues it for publishing.
  `create_posts` with status `scheduled` checks every target and queues them in one call; use it
  only when the user has approved every post. `delete_post` withdraws a scheduled post; a
  published post stays online.
- Send a fresh `idempotency_key` on every spending or publishing call and reuse it only to
  retry that same call within 24 hours.
- Poll `get_generation` and `get_upload` every 2 to 5 seconds until they finish. `list_posts`
  shows a sent post go scheduled, then published.
- A refusal comes back as an error envelope with a reason. Explain it; never retry blindly.

## The 20 tools

Read permission, free:

- `list_brands`: the brands this token may act for.
- `list_channels`: a brand's connected social channels, with the caption and media limits each
  one enforces.
- `get_channel_requirements`: what a connected account accepts right now, including the TikTok
  privacy levels the creator allows and the Pinterest boards a pin can go to.
- `list_media`: the finished exports in a brand's media library, by folder or by name.
- `list_formats`: the video formats a generation can be built from, and their render styles.
- `get_wallet`: the credit balance of the account.
- `quote_generation`: the price of a generation before running it.
- `get_generation`, `list_generations`: one generation, or the list, newest first.
- `get_text_generation`: one text generation and the variants it wrote.
- `get_upload`: the state of an upload ticket.
- `check_reference`: whether a video link can be used as the source of a swap; if it can, the
  link is recorded on the account and fetched.
- `list_posts`: posts with their scheduled time, release link and metrics.

Generate permission:

- `create_generation`: run a video generation. Spends credits.
- `generate_text`: write post variants from a brief, a link, a finished generation or a media
  item. Spends credits.
- `create_upload`, `complete_upload`: bring a file from the user's machine into the account.

Publish permission:

- `create_posts`: compose one post per channel from the same media.
- `send_post`: send one draft to its channel, at its scheduled time or straight away.
- `delete_post`: delete a post; a scheduled one is withdrawn from its channel first.

## What it cannot do

It cannot connect or disconnect a social channel, change the plan or buy credits, or act at all
once the user revokes it in Settings, API tokens. Cloning a video from a link, and generating
still images or carousels, are not available through an agent yet.

Docs: https://www.socialfaktory.com/docs/mcp. Support: hello@socialfaktory.com
