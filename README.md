# Announcr MCP Plugin

Lets Grok (and any MCP client) push spoken announcements into [Announcr](https://announcr.fm).

## Setup

1. Create a webhook endpoint in the Announcr UI and copy the publicId + secret.
2. Under **What to hear**, subscribe to `service=mcp` (event can be blank or `announce`).
3. Set the two environment variables:

```bash
export ANNOUNCR_WEBHOOK_PUBLIC_ID=...
export ANNOUNCR_WEBHOOK_SECRET=...