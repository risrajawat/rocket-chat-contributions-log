# Meteor & Repo Structure

Rocket.Chat is built on **Meteor** (a full-stack JS framework, handles both
client and server in one codebase) plus **React** for UI. It's a monorepo —
one repo, multiple packages.

## Why this matters for contributing
Most of the actual app code you'll touch lives under `apps/meteor/`, not the
repo root. The other folders (`packages/`) are shared libraries used across
Rocket.Chat's different projects (server, Apps Engine, Livechat widget, etc).

## Top-level layout (apps/meteor/)

| Folder | What's in it |
|---|---|
| `app/` | Core features, organized by feature (e.g. `app/livechat/`, `app/lib/`) — this is where most feature logic lives |
| `client/` | Frontend-only code (React components, hooks, pages) |
| `server/` | Backend-only code (REST API, services, models) |
| `ee/` | Enterprise Edition code — paid/licensed features (e.g. `ee/app/livechat-enterprise/`) |
| `lib/` | Shared helper functions/classes used by both client and server |
| `packages/` | Meteor packages customized for this project |
| `tests/` | `tests/e2e` (Playwright, browser-driven) and `tests/unit` (Jest, function-level) |

## Client vs Server vs App split (important, confusing at first)
- `app/livechat/` → business logic that can run on either client or server, split into sub-folders internally
- `server/` → REST endpoints, DB models, backend services (no UI)
- `client/` → React components, UI state, nothing that talks directly to the DB
- A single "feature" (like Livechat) usually has code spread across all three — that's normal, not a mistake in the codebase

## Meteor-specific things to know
- Meteor auto-loads files based on folder location (no manual imports needed for some legacy code) — can be confusing if you're used to plain Node/React projects
- `Meteor.methods()` = how the old-style client-server RPC calls work (newer code prefers REST API + services instead)
- Rocket.Chat is gradually moving away from Meteor toward a more standard Node/microservices structure — so you'll see both old (Meteor-style) and new (service-style) patterns in the same repo