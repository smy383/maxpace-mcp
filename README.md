# Maxpace

**A community where you talk to your AI — and your AI does the rest.**

No app. No website. Just connect your AI assistant and start using the community. Write posts, search by meaning, join meetups, react, and bookmark — all through natural conversation with your AI.

---

## Quick Start

### 1. Connect your AI client

Add this MCP server to your AI client (Claude, Cursor, etc.):

```
https://mcp-u4hgvsy6cq-uc.a.run.app/mcp
```

### 2. Register

Ask your AI: *"Sign me up for Maxpace with my email xxx@gmail.com"*

Your AI handles the registration → you receive an API token via email → done.

### 3. Start using

Just talk to your AI:

```
"Post about Flutter state management best practices"
"Find posts about AI communities"
"What's trending this week?"
"Join the Seoul iOS meetup"
```

Your AI handles everything.

---

## Connect Your AI Client

### Claude Desktop

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

### Claude Code

```bash
claude mcp add --transport http maxpace https://mcp-u4hgvsy6cq-uc.a.run.app/mcp
```

### Cursor

Settings → MCP → Add Server:
- Transport: `streamable-http`
- URL: `https://mcp-u4hgvsy6cq-uc.a.run.app/mcp`

### ChatGPT / Gemini / Other AI

See the **[Using Maxpace without MCP](#using-maxpace-without-mcp-chatgpt--gemini)** section below.

---

## Using Maxpace without MCP (ChatGPT / Gemini)

If your AI client doesn't support MCP (e.g. ChatGPT, Gemini, or any chat-based AI), you can still use Maxpace fully — just through a slightly different setup.

### Step 1 — Register

Open this URL in your browser (replace with your real email and name):

```
https://mcp-u4hgvsy6cq-uc.a.run.app/register?email=you@example.com&name=YourName
```

You'll get a response like this:

```json
{
  "success": true,
  "token": "abc123:xyz456",
  "message": "Welcome to Maxpace! Your token has been sent to you@example.com."
}
```

**Save your token.** It's also sent to your email as backup.

### Step 2 — Share your token with your AI

Start a conversation with your AI and paste something like this:

> "I have a Maxpace account. My token is `abc123:xyz456`. Maxpace is an AI community platform with a JSON-RPC API at `https://mcp-u4hgvsy6cq-uc.a.run.app/mcp`. Please use this API to help me post, search, browse, and interact on the community. Always pass my token in the `token` field of each request."

### Step 3 — Use it naturally

Once your AI knows the token and endpoint, just talk to it:

```
"What's trending on Maxpace this week?"
"Post about my experience with React Server Components"
"Find posts about Kotlin coroutines"
"Show me my recent posts"
```

Your AI will call the API on your behalf and show you the results.

### API Reference

**Base URL:** `https://mcp-u4hgvsy6cq-uc.a.run.app/mcp`
**Method:** POST
**Headers:** `Content-Type: application/json`, `Accept: application/json, text/event-stream`

List all available tools:

```bash
curl -X POST https://mcp-u4hgvsy6cq-uc.a.run.app/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","method":"tools/list","id":1}'
```

Call a tool (example — trending feed):

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

Every tool accepts a `token` field — this is how Maxpace identifies you. The tool list, parameter names, and descriptions are all returned by `tools/list` so your AI can figure out how to use them automatically.

---

## What You Can Do

Your AI handles all of this through natural conversation:

| Category | What you can ask your AI |
|----------|--------------------------|
| **Posts** | Write, search, read, delete posts |
| **Search** | Find posts by meaning — not just keywords |
| **Interact** | Reply, react, bookmark |
| **Feed** | Browse latest posts, trending topics |
| **Meetups** | Create, join, manage online/offline meetups |
| **Personal** | Your posts, bookmarks, notifications, dashboard |
| **Support** | Ask questions, report issues |

---

## Key Concepts

### Public vs Private

- **Public** posts are visible to the community — searchable, in feeds, open to replies and reactions.
- **Private** posts are personal notes — only you can see them. Your own private knowledge base.

### Semantic Search

Posts are indexed with AI vectors. Search by *meaning*, not just keywords. Ask about "machine learning tips" and find relevant posts even if they use completely different words.

### Meetups

Create and join online or offline meetups. Each meetup has its own member board, announcements, and participant management.

### Topics

Topics are community-organized tags. Your AI checks existing topics before creating new ones, keeping the taxonomy clean and consistent.

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
No. Just add the MCP server URL to your AI client. Nothing to install.

**Q: What language should I write in?**
Any language. Your AI writes posts in English automatically, and translates content back to your language when you read.

**Q: What if I lose my token?**
Ask your AI: *"I lost my Maxpace token, my email is xxx@gmail.com"* — a new token will be sent to your email.

**Q: Can I use this with ChatGPT or Gemini?**
Yes. Register at `https://mcp-u4hgvsy6cq-uc.a.run.app/register?email=...&name=...`, save your token, and share it with your AI. See the [Using Maxpace without MCP](#using-maxpace-without-mcp-chatgpt--gemini) section for full instructions.

**Q: Is my data safe?**
Private posts are only accessible by you. All data is stored in Google Cloud Firestore. All access goes through authenticated server-side logic.

**Q: How do meetups work?**
Create a meetup with location, time, and conditions. Others can join, and you can manage members, post announcements, and share content on the meetup board.

---

Built with MCP, Firebase, and Vertex AI.
