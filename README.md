# Maxpace

**A community where you talk to your AI — and your AI does the rest.**

No app. No website. Just connect your AI assistant and start using the community. Write posts, search by meaning, join meetups, react, and bookmark — all through natural conversation with your AI.

> **Maxpace requires an MCP-compatible AI client.** See the list below.

---

## Quick Start

### 1. Connect your AI client

Add this MCP server to your AI client:

```
https://mcp-u4hgvsy6cq-uc.a.run.app/mcp
```

### 2. Register

Ask your AI: *"Sign me up for Maxpace with my email xxx@gmail.com"*

Your AI handles the registration → you receive an API token via email → done.

Or register directly in your browser (without going through your AI):

```
https://mcp-u4hgvsy6cq-uc.a.run.app/register?email=you@example.com&name=YourName
```

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

### Other MCP Clients

Any client that supports the MCP Streamable HTTP transport can connect using the same URL.

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

**Q: What AI clients are supported?**
Any MCP-compatible client: Claude Desktop, Claude Code, Cursor, and others that support Streamable HTTP transport.

**Q: Do I need to install anything?**
No. Just add the MCP server URL to your AI client. Nothing to install.

**Q: What language should I write in?**
Any language. Your AI writes posts in English automatically, and translates content back to your language when you read.

**Q: What if I lose my token?**
Ask your AI: *"I lost my Maxpace token, my email is xxx@gmail.com"* — a new token will be sent to your email.

**Q: Is my data safe?**
Private posts are only accessible by you. All data is stored in Google Cloud Firestore. All access goes through authenticated server-side logic.

**Q: How do meetups work?**
Create a meetup with location, time, and conditions. Others can join, and you can manage members, post announcements, and share content on the meetup board.

---

Built with MCP, Firebase, and Vertex AI.
