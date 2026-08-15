# UZDoom NetHub architecture

This PK3 is intentionally a **client-side shell/prototype**, not a fake implementation of
Internet networking.

## What the PK3 does
- Adds NetHub CVARs and an in-game configuration menu.
- Provides configurable player/AI/WAD-stack limits.
- Provides a placeholder AI-avatar actor and test key.
- Defines endpoint/model/server configuration values.
- Leaves the engine bridge boundary explicit.

## What requires an UZDoom source fork / companion bridge
1. Internet server discovery and connection.
2. Downloading WAD/PK3 files from a repository.
3. Hash/signature verification and dependency resolution.
4. Dynamic mounting of up to 60 content packages.
5. AI HTTP/WebSocket calls and response parsing.
6. Translating AI decisions into deterministic multiplayer commands.
7. Hardware telemetry and renderer/CPU worker scheduling.
8. True player counts beyond the engine's compiled/network protocol limits.

## Recommended design
- UZDoom client fork: `NetHubClient`.
- Optional dedicated relay/server: `NetHubServer`.
- Mod repository: HTTPS + SHA-256 manifest.
- AI bridge: local HTTP/WebSocket service, so API keys never live in a PK3.
- Multiplayer: authoritative server or deterministic lockstep, with strict command
  validation and bandwidth budgeting.
- Content stack: manifest -> dependency graph -> hashes -> mount order.
- Performance: dynamic actor budget, texture streaming, culling, particle budgets,
  audio voice limits, and renderer-specific presets.

## Important
Do not let a downloaded PK3 execute arbitrary native code. Treat remote mods as
untrusted content and require explicit user approval.
