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

## Publishing `@announcr/mcp` to npm

This plugin launches the server with `npx -y @announcr/mcp`. The package must be published before the plugin will work for other users.

From the Announcr monorepo root (after the MCP package is on `main`):

```bash
# 1. Build the package
pnpm --filter @announcr/mcp build

# 2. Log in to npm (one-time)
npm login

# 3. Publish (scoped package must be public)
cd packages/mcp
npm publish --access public
```

Notes:
- You need an npm account that owns (or can create) the `@announcr` scope.
- If the scope is not available, publish under your personal scope instead (e.g. `@daveyoung74/announcr-mcp`) and update `.mcp.json` accordingly.
- After publishing, verify with:

```bash
npx -y @announcr/mcp --help   # or just run it; it should start the stdio server
```

## License

MIT
