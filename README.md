# SocialFaktory MCP server

<img src="assets/logo-512.png" width="96" alt="SocialFaktory">

Write, generate, schedule and publish a brand's social content, in its own voice, on every channel.

SocialFaktory runs a brand's social media from one place, and this connector lets your agent run it with you. It reads the brands, channels and media you already have, writes posts in the brand's own voice, prices and generates short video, takes a file you upload, composes one post per channel, schedules or sends it on TikTok, Instagram, YouTube, X, LinkedIn, Facebook and Pinterest, and reads the metrics back. Reading is free. Generating spends the credits in your wallet, and the agent is told to quote first and ask you. Posts are drafts until you send them, and nothing reaches a channel without the publish permission you grant on the consent screen, where you also pin the connection to one brand, cap what it may spend each month and choose when it expires. Cloning a video from a link, and generating still images or carousels, are not available through an agent yet. Connecting a social channel is a browser sign-in and stays in the app.

This repository holds the listing files for the hosted server. The server itself runs at `https://www.socialfaktory.com/mcp` and is not open source. You need a SocialFaktory account with at least one brand and a connected social channel: https://www.socialfaktory.com

- Endpoint: `https://www.socialfaktory.com/mcp` (Streamable HTTP)
- Auth: OAuth 2.1 with PKCE, dynamic client registration and Client ID Metadata Documents, or a personal access token from Settings > API tokens
- Docs: https://www.socialfaktory.com/docs/mcp
- Privacy: https://www.socialfaktory.com/privacy
- Terms: https://www.socialfaktory.com/terms
- Support: hello@socialfaktory.com

## Install

### Claude Code

```bash
claude mcp add --transport http socialfaktory https://www.socialfaktory.com/mcp
```

Then run `/mcp` and sign in when prompted.

### Claude Desktop

Settings > Connectors > Add > Add custom connector, paste `https://www.socialfaktory.com/mcp`, leave client id and secret empty, click Add and sign in.

### Cursor

[Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=socialfaktory&config=eyJ1cmwiOiJodHRwczovL3d3dy5zb2NpYWxmYWt0b3J5LmNvbS9tY3AifQ==)

```json
{
  "mcpServers": {
    "socialfaktory": { "url": "https://www.socialfaktory.com/mcp" }
  }
}
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "socialfaktory": { "type": "http", "url": "https://www.socialfaktory.com/mcp" }
  }
}
```

### Codex CLI

```bash
codex mcp add socialfaktory --url https://www.socialfaktory.com/mcp
codex mcp login socialfaktory
```

### Gemini CLI

```bash
gemini extensions install https://github.com/adifsgaid/socialfaktory-gemini-extension
```

### Windsurf

```json
{
  "mcpServers": {
    "socialfaktory": { "serverUrl": "https://www.socialfaktory.com/mcp" }
  }
}
```

### Any other client, with a token

```
Authorization: Bearer YOUR_TOKEN
```

Mint the token in the app under Settings > API tokens. Pick the scopes (read, generate, publish), an optional brand pin, a monthly credit cap and an expiry.

## The 20 tools

| Tool | What it does |
|---|---|
| `list_brands` | List the brands this token may act for. |
| `list_channels` | List a brand's connected social channels with the caption and media limits each one enforces. |
| `list_media` | Browse the finished exports in a brand's media library, by folder or by name. |
| `list_formats` | List the video formats a generation can be built from, and the render styles they can be drawn in. |
| `get_wallet` | Read the credit balance of the account the token belongs to. |
| `quote_generation` | Price a generation before running it. |
| `create_generation` | Run a video generation and charge the user's credits for it. |
| `get_generation` | Read one generation: where it is, the finished video, and the editor asset it produced. |
| `list_generations` | List the generations this token may see, newest first. |
| `generate_text` | Write post variants for a brand from a brief, a link, a finished generation or a media item. |
| `get_text_generation` | Read one text generation and the variants it wrote. |
| `create_upload` | Open a ticket for a file from the user's machine, content the service has not seen. |
| `complete_upload` | Tell the service the bytes are in place and start the ingest. |
| `get_upload` | Read an upload ticket. |
| `check_reference` | Ask whether a video link can be used as the source of a swap, and if so record it on the account and start fetching it. |
| `get_channel_requirements` | Ask the connected social account what it accepts right now: its limits, the TikTok privacy levels the creator allows, and the Pinterest boards a pin can go to. |
| `create_posts` | Compose one post per channel from the same media. |
| `send_post` | Send one draft to the social account the user connected for its channel, at its scheduled time or straight away. |
| `list_posts` | List posts with their scheduled time, the link they were released at, and their metrics. |
| `delete_post` | Delete a post from the account. |

## What it cannot do

It cannot connect or disconnect a social channel, change your plan or buy credits, read your password or sign in as you, or act at all once you revoke it in Settings, API tokens. Cloning a video from a link, and generating still images or carousels, are not available through an agent yet.

## Files in this repository

- `server.json`: the entry published to the official MCP Registry as `com.socialfaktory/mcp` (https://registry.modelcontextprotocol.io/v0.1/servers/com.socialfaktory%2Fmcp/versions/latest)
- `.cursor-plugin/plugin.json`, `mcp.json`: Cursor plugin manifest and server config
- `skills/socialfaktory/SKILL.md`: OpenClaw skill, published on ClawHub
- `assets/`: logo

The files in this repository are MIT licensed. The hosted service is governed by its own terms.
