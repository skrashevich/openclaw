# @openclaw/meshtastic

Meshtastic mesh channel plugin for OpenClaw using the node HTTP API.

## Overview

This extension connects OpenClaw to a Meshtastic device that exposes the HTTP API
(`/api/v1/fromradio`, `/api/v1/toradio`) via `@meshtastic/transport-http`.

Supported in v1:

- Direct messages (DM)
- Mesh broadcast channels
- Reply-to threading
- Pairing, allowlist, and group policy controls
- Outbound text chunking for mesh size limits

## License boundary

This plugin depends on `@meshtastic/core` and `@meshtastic/transport-http`, which are
**GPL-3.0-only**. The plugin package isolates that dependency; review GPL obligations
before redistribution.

## Installation

```bash
openclaw plugins install @openclaw/meshtastic
```

Dev checkout:

```bash
openclaw plugins install --link extensions/meshtastic
```

Restart the Gateway after installing or enabling plugins.

## Quick setup

```json5
{
  channels: {
    meshtastic: {
      enabled: true,
      host: "192.168.1.10",
      tls: false,
      port: 4433,
      dmPolicy: "pairing",
      groupPolicy: "allowlist",
      channels: [0],
      groups: {
        "channel:0": { requireMention: false },
      },
    },
  },
}
```

Environment variables (default account only):

- `MESHTASTIC_HOST` — node host or `host:port`
- `MESHTASTIC_PORT` — HTTP API port (default 4433)
- `MESHTASTIC_TLS` — set to `true` for HTTPS

## Target grammar

| Target               | Meaning                                 |
| -------------------- | --------------------------------------- |
| `!12345678`          | Direct message to node id               |
| `node:305419896`     | Direct message by decimal node number   |
| `channel:0` / `ch:0` | Broadcast on mesh channel 0             |
| `broadcast`          | Alias for primary channel (`channel:0`) |

## Testing

```bash
pnpm test extensions/meshtastic
```

## Troubleshooting

- Ensure the Meshtastic node exposes the HTTP API (default port 4433).
- This plugin uses HTTP transport, not Meshtastic TCP.
- If probe fails, verify network reachability and that TLS settings match the node.

## License

Plugin scaffolding: MIT. Meshtastic dependencies: GPL-3.0-only (see above).
