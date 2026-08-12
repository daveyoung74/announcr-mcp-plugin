# Announcr MCP Plugin

Lets Grok (and any MCP client) push spoken announcements into [Announcr](https://announcr.fm).

The announcement is turned into TTS audio and delivered to the user’s devices / ambient speakers.

## Setup

1. Create a **webhook endpoint** in the Announcr UI and copy the `publicId` + one-time secret.
2. Under **What to hear**, subscribe to:
   - **Service**: `mcp`
   - **Event**: leave blank or `announce`
3. Set the two environment variables (never put the secret in the model context):

```bash
export ANNOUNCR_WEBHOOK_PUBLIC_ID=your_public_id
export ANNOUNCR_WEBHOOK_SECRET=your_secret
```

4. Install / enable this plugin (or add it as a custom MCP connector).

## Tool

### `send_announcement`

| Argument  | Required | Default     | Description                                      |
|-----------|----------|-------------|--------------------------------------------------|
| `message` | yes      | —           | Text that will be spoken (max 500 characters)    |
| `event`   | no       | `"announce"`| Event name used for subscription matching        |
| `service` | no       | `"mcp"`     | Service/app label (lets one webhook multiplex)   |

Example call:

```json
{
  "name": "send_announcement",
  "arguments": {
    "message": "Deploy finished successfully."
  }
}
```

## How it works

The server signs every request with the same HMAC-SHA256 scheme the Announcr gateway expects (`x-announcr-timestamp` + `x-announcr-signature`) and POSTs to `/hooks/in/{publicId}`.

## License

MIT
