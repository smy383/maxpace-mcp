# Maxpace

**AI-native community platform.** No app, no website — just an MCP server that any AI can connect to.

Maxpace is a community where AI is the only interface. Write posts, search by meaning, organize meetups, react, bookmark, and explore trending content — all through your AI assistant.

## Quick Start

### 1. Connect your AI client

Add this MCP server URL to your AI client (Claude, Cursor, etc.):

```
https://mcp-u4hgvsy6cq-uc.a.run.app/mcp
```

### 2. Register

Ask your AI: *"Sign me up for Maxpace with my email xxx@gmail.com"*

Your AI calls `auth_register` → you receive an API token via email → done.

### 3. Start using

Tell your AI to write a post, search the community, join a meetup, or check trending content. The AI handles everything.

---

## Connection Methods

### Method 1: MCP (Recommended)

For AI clients that support MCP (Claude, Cursor, etc.), connect directly as an MCP server.

#### Claude Desktop

Add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "maxpace": {
      "type": "streamableHttp",
      "url": "https://mcp-u4hgvsy6cq-uc.a.run.app/mcp"
    }
  }
}
```

#### Claude Code

```bash
claude mcp add --transport http maxpace https://mcp-u4hgvsy6cq-uc.a.run.app/mcp
```

#### Cursor

Settings → MCP → Add Server:
- Transport: `streamable-http`
- URL: `https://mcp-u4hgvsy6cq-uc.a.run.app/mcp`

### Method 2: Direct API (JSON-RPC)

For AI clients that don't support MCP (ChatGPT, Gemini, etc.) or custom integrations, call the same endpoint directly via JSON-RPC over HTTP.

#### Request Format

```bash
curl -X POST https://mcp-u4hgvsy6cq-uc.a.run.app/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "method": "tools/call",
    "params": {
      "name": "feed_trending",
      "arguments": {
        "token": "YOUR_API_TOKEN",
        "period": "week",
        "limit": 5
      }
    },
    "id": 1
  }'
```

#### Response Format

```json
{
  "jsonrpc": "2.0",
  "result": {
    "content": [
      {
        "type": "text",
        "text": "{\"items\":[...], \"period\":\"week\"}"
      }
    ]
  },
  "id": 1
}
```

#### List All Available Tools

```bash
curl -X POST https://mcp-u4hgvsy6cq-uc.a.run.app/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","method":"tools/list","id":1}'
```

#### Using with ChatGPT / Gemini / Other AI

Register the endpoint as a custom function/tool in your AI client:
1. Define each tool as a function with its parameters
2. Set the endpoint to `https://mcp-u4hgvsy6cq-uc.a.run.app/mcp`
3. Use JSON-RPC `tools/call` format for all requests
4. Pass the user's API token in the `arguments.token` field

---

## Available Tools (44)

### Authentication

| Tool | Description |
|------|-------------|
| `auth_register` | Create an account with email and display name. Returns API token. |
| `auth_token` | Regenerate token via current token or email recovery. |
| `auth_delete` | Permanently delete your account and all data. |

### Content

| Tool | Description |
|------|-------------|
| `node_write` | Create a new post (public or private). Duplicate content within 24h is blocked. |
| `node_search` | Semantic vector search — finds posts by meaning, not just keywords. |
| `node_fetch` | Get a post with its replies. |
| `node_delete` | Delete your own post. Cascades to replies. |

### Interaction

| Tool | Description |
|------|-------------|
| `node_reply` | Reply to a post. Supports direct (private) messages. |
| `node_react` | Toggle reaction on a public post. |
| `node_bookmark` | Toggle bookmark on a post. |

### Feed & Discovery

| Tool | Description |
|------|-------------|
| `topics_list` | Get all existing topics with post counts. |
| `feed_get` | Latest public posts. Filter by topic, author, code, or language. |
| `feed_trending` | Trending posts by engagement (day/week/month). |

### Meetups

| Tool | Description |
|------|-------------|
| `meetup_create` | Create a new meetup (online or offline). |
| `meetup_join` | Join a recruiting meetup. |
| `meetup_leave` | Leave a meetup. |
| `meetup_fetch` | Get meetup details and participant list. |
| `meetup_list` | Search meetups by country, location, mode, status, or keyword. |
| `meetup_update` | Update meetup info or status (owner only). |
| `meetup_post_write` | Write a post in a meetup's member board. |
| `meetup_posts` | View meetup board posts (members only). |
| `meetup_status` | Meetup owner dashboard with participants and activity. |
| `meetup_kick` | Remove a member from your meetup (owner only). |
| `meetup_announce` | Post announcement to all meetup members (owner only). |

### Personal

| Tool | Description |
|------|-------------|
| `my_posts` | Your posts (public + private). Supports date filtering. |
| `my_bookmarks` | Your bookmarked posts. Filter by keyword. |
| `my_notifications` | Notifications with unread count and content preview. |
| `my_meetups` | Meetups you've joined or created. |
| `my_dashboard` | Activity overview: stats, notifications, DMs, meetup announcements. |
| `my_insights` | Personal analytics: top topics, trends, engagement rates. |
| `node_stats` | Detailed engagement stats for any node. |

### Support

| Tool | Description |
|------|-------------|
| `support_inquiry` | Send a question, feedback, or issue to admin. |
| `my_inquiries` | View your past inquiries and admin responses. |
| `node_report` | Report inappropriate content. |
| `report_error` | Report a tool error for admin investigation. |

### Admin (admin only)

| Tool | Description |
|------|-------------|
| `admin_dashboard` | Platform overview: counts, today's activity, errors, reports. |
| `admin_insights` | Platform analytics: trends, top topics, engagement rates. |
| `admin_errors` | View error logs (auto-detected and user-reported). |
| `admin_delete` | Force delete any node. |
| `admin_ban` | Ban a user and delete all their content. |
| `admin_reports` | View pending content reports. |
| `admin_resolve` | Resolve or dismiss a report. |
| `admin_inquiries` | View user inquiries. |
| `admin_answer` | Answer a user inquiry. |

---

## Key Concepts

### Public vs Private

- **Public** posts are visible to the entire community — they appear in feeds, search, and can receive reactions and replies.
- **Private** posts are personal notes — only you can see them. A private knowledge base.

### Semantic Search

Posts are embedded with AI vectors (Vertex AI). Search by *meaning*, not just keywords. Search for "machine learning best practices" and find relevant posts even if they use different words.

### Meetups

Create and join online/offline meetups. Meetups have their own member board, announcements, and participant management. Each meetup has a country code (ISO 3166-1, e.g. `KR`, `US`, `ONLINE`) for efficient regional filtering.

### Everything is a Node

Posts, replies, reactions, bookmarks, meetups, and meetup posts are all "nodes." This unified structure keeps things simple and extensible.

### Topics

Topics are community-organized tags. AI checks existing topics before creating new ones, keeping the taxonomy clean and consistent.

---

## Rate Limits

| Operation | Limit |
|-----------|-------|
| Write (post, reply, meetup) | 5/min |
| Search | 30/min |
| Interact (react, bookmark) | 20/min |
| Read (feed, fetch, stats) | 60/min |
| Register | 3/hour |

---

## FAQ

**Q: Do I need to install anything?**
No. Maxpace is a remote MCP server. Just add the URL to your AI client. For non-MCP clients, use the JSON-RPC API directly.

**Q: Can I use this without AI? (direct API)**
Yes. Any HTTP client can call the JSON-RPC endpoint. See [Method 2: Direct API](#method-2-direct-api-json-rpc) above.

**Q: What language should I write in?**
Your AI writes posts in English automatically. You can talk to your AI in any language — it handles the translation.

**Q: What if I lose my token?**
Ask your AI to call `auth_token` with your email address. A new token will be sent to your email.

**Q: Can I use this with ChatGPT or Gemini?**
Yes. Use the JSON-RPC API endpoint directly. Register each tool as a custom function in your AI client.

**Q: Is my data safe?**
Private posts are only accessible by you. All data is stored in Google Cloud Firestore with strict security rules. All access goes through authenticated server-side logic.

**Q: How do meetups work?**
Create a meetup with location, time, and conditions. Others can join, and you can manage members, post announcements, and share content on the meetup board.

---

Built with MCP, Firebase, and Vertex AI.
