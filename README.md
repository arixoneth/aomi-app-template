# Aomi app template

A small Aomi app you can deploy as is, then edit. The Rust app is at the repo
root. `web/` and `client/` are two ways to talk to it once it is deployed.

## What's inside

- `src/` — the app (`playground-example`, build `v1`), built with `aomi-sdk =6.0.0`
  - `greet` and `about` are always available to the model.
  - `menu_search` and `order_total` belong to the `playground-example/cafe` skill
    (`src/skill/cafe.md`), so the model only sees them after it activates the skill.
- `tests/tools.rs` — tests for every tool, plus a check of the manifest and skill ownership
- `web/` — Vite + React page with `<AomiWidget>` in direct mode
- `client/` — Node scripts that use `@aomi-labs/client`; `pnpm --filter client tour` runs a scripted conversation
- `.aomi/config.json`, `aomi.toml` — what Build reads to deploy

The Rust build never touches the JS workspace.

## Test locally

```sh
cargo test
```

## Deploy

Pick one.

**Build:** open Aomi Build and choose "Start from the template".

**CLI:**

```sh
cargo install aomi-sdk --version 6.0.1 --features cli --locked
aomi-build login
aomi-build project create
aomi-build deploy
```

To deploy against staging instead, add `--backend https://api-staging.aomi.dev`.

Deploying gives you an application id. Put it in the env files below.

## Widget

```sh
cp web/.env.example web/.env      # set VITE_AOMI_APPLICATION_ID
pnpm install
pnpm --filter web dev
```

## Client

```sh
cp client/.env.example client/.env      # set AOMI_APPLICATION_ID
pnpm install
pnpm --filter client tour               # the scripted conversation
pnpm --filter client ask "Greet Ada, excitedly"
```

Both print the host they talk to when they start. They use the production host
unless you set `VITE_AOMI_BASE_URL` / `AOMI_BASE_URL=https://chat-staging.aomi.dev`.
