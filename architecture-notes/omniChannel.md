# Omnichannel

## What it is
Omnichannel = Rocket.Chat's customer-support / live-chat module. Handles
website visitors chatting with support agents, across channels (web widget,
WhatsApp, email, SMS, etc — all routed into the same inbox).

Owned by the `@RocketChat/omnichannel` team internally — this is the area
where your current PR (livechat enterprise sidebar test) lives.

## Key concepts
- **Visitor** — the external person chatting in (not a registered Rocket.Chat user)
- **Agent** — the support person answering
- **Department** — a routing category (e.g. "Sales", "Support") visitors get assigned to
- **Queue** — where incoming chats wait before being assigned to an agent
- **Room** — same underlying concept as a regular chat room, just of type `l` (livechat)

## Where the code lives

| Path | What's there |
|---|---|
| `apps/meteor/app/livechat/` | Core (free/community) livechat backend logic |
| `apps/meteor/ee/app/livechat-enterprise/` | Enterprise-only livechat features (this is what your PR touches) |
| `apps/meteor/ee/app/canned-responses/` | Enterprise: pre-written quick-reply templates |
| `apps/meteor/client/components/omnichannel/` | Frontend components (agent's inbox UI, etc) |
| `apps/meteor/ee/client/omnichannel/` | Enterprise frontend components |
| `apps/meteor/server/services/omnichannel-voip/`, `app/voip/` | Voice-call version of omnichannel |
| `packages/livechat/` | The embeddable widget code that runs on a visitor's website (separate from the agent-side app) |

## Rough flow (as commonly understood, verify as you read code)
1. Visitor sends message via the widget (`packages/livechat`)
2. Message creates/updates a livechat room, goes into a **queue**
3. Queue dispatch assigns it to an available agent (based on department rules)
4. Agent responds from `client/components/omnichannel/` UI
5. Various hooks fire on message send/room state change (e.g. `markRoomResponded`) to track SLAs, auto-close timers, etc.

## Testing split (relevant to your PR)
- `tests/e2e/omnichannel/` — Playwright, full browser simulation, slow
- Unit tests — faster, test one function/module in isolation without a real browser
- Your PR is about replacing an e2e test with a unit test for a specific
  sidebar logout case — the general pattern in this repo is: e2e tests are
  expensive to run, so when a behavior can be verified without a real
  browser, maintainers prefer converting it to a unit test

## My notes (fill in as you explore/work on issues)
-
-
-

## Still confused about
-
-