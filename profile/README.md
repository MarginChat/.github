# margin

**Communication made simple.** Messages, channels, and voice & video calls for your team — in one app, built and hosted in Europe, open source and self-hostable.

[margin.chat](https://margin.chat) · [Download](https://margin.chat/download) · [Open in browser](https://margin.chat/app) · [App Store](https://apps.apple.com/app/margin-messenger/id6790111555)

---

## What margin is

An open-source, European alternative to Slack and Teams.

Most team chat tools try to do everything. margin does less, on purpose — the things teams actually use every day, simple enough that new people understand it on day one and no one needs an IT department to run it.

- **One place for everything** — messages, channels, and calls in a single app.
- **Optional Secret mode** — end-to-end encrypted private *and* group chats. Messages are stored encrypted on the server; your private key is encrypted with your password before it ever leaves your device.
- **Voice and video** — 1:1 calls go peer-to-peer, group calls are routed through our own SFU.
- **European by default** — hosted on Hetzner in Europe. Not AWS, not Azure, not Google Cloud.
- **Yours to run** — self-hostable on your own infrastructure, not just ours.

## Repositories

| Repository | Stack | What it is |
|---|---|---|
| [margin-server](https://github.com/Margin-Chat/margin-server) | Java 25, Spring Boot 4, PostgreSQL, Flyway | REST API, WebSocket signaling, auth, persistence |
| [margin-desktop](https://github.com/Margin-Chat/margin-desktop) | Electron, Vite, Svelte 5, TypeScript | The primary client — desktop app and web build |

The rest of the platform — the mobile client, the group-call SFU, the marketing site, and the deployment manifests — is being opened up repository by repository as each one is ready.

## How the pieces fit

```
  margin-desktop  ──────────  HTTPS / WSS  ──────────▶  margin-server
  margin-mobile   ◀─────────  REST + WS    ───────────  (Spring Boot + PostgreSQL)
        │                                                     │
        │  WebRTC (1:1, peer-to-peer)                          │  internal API + signaling
        ▼                                                      ▼
    the other peer                                        margin-sfu
                                                     (group call media relay)
```

Both clients are independent implementations of the same protocol — mobile is not a wrapper around the desktop app. The server relays call signaling but never sees 1:1 media, and never sees the contents of a secret chat.

## Vocabulary

The domain words are specific, and worth knowing before reading the code:

- **Margin** — a workspace. The top-level container for an organization.
- **Space** — a grouping inside a Margin: a department, a project, a team.
- **Channel** — a topic inside a Space, text or voice.
- **Conversation** — the thing that owns messages. A channel, a DM, or a group DM.

## Self-hosting

margin runs on your own infrastructure as well as it runs on ours — the server is a single Spring Boot service in front of PostgreSQL. See the [margin-server README](https://github.com/Margin-Chat/margin-server) to build and run it; the production Docker Compose and nginx manifests will follow when that repository goes public.

## Contributing

Issues and pull requests are welcome on any public repository. Plenty of ways to help don't involve writing code — bug reports, docs, and translations all count. Please search existing issues before opening a new one.

## License

AGPLv3. Read the code, audit the encryption, run it yourself.
