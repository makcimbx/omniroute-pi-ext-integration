# OmniRoute Manager for Pi

A [Pi Coding Agent](https://github.com/earendil-works/pi/tree/main/packages/coding-agent) extension that integrates [OmniRoute](https://github.com/diegosouzapw/OmniRoute) — an AI gateway that routes requests across 44+ LLM providers with automatic fallback, load balancing, and cost optimization.

## What it does

- **Status bar** shows which model actually served each response — e.g. `CheapFix → gemini-2.5-flash (gemini · Google Account)`
- **Combo preview** on model switch — immediately shows the routing order before you send a message
- **Startup warnings** if any provider connections are expired or need re-authentication
- **Combo management** — edit models, create, and delete combos from within pi
- **Provider browser** — drill into providers to see accounts, connection health, and available models
- **Model sync** — push all OmniRoute models and combos to pi's Ctrl+P model picker
- **Live usage limits** — query provider APIs directly for real-time quota and rate limit data
- **Health diagnostics** — call log analysis, config diagnostics, and auto-fix

## Install

```bash
pi install git:github.com/md-riaz/omniroute-pi-ext-integration
```

### Configure pi to use OmniRoute as a provider

Start Pi and run the setup wizard:
```bash
/omni setup
```
This will ask for your OmniRoute URL and API key, and configure `~/.pi/agent/models.json` automatically.

Then run `/omni sync` inside pi to populate the full model list automatically.

### Start pi

```bash
pi
```

The extension auto-loads. You'll see `OmniRoute ready — N combos` on startup.

## Commands

| Command | Description |
|---------|-------------|
| `/omni` | Status dashboard — health, active combos, provider issues |
| `/omni combos` | Manage combos — edit models, create, delete |
| `/omni providers` | Browse providers, models & add new ones |
| `/omni health` | Call log analysis + config diagnostics & auto-fix |
| `/omni limits` | Live usage quotas — queries provider APIs directly |
| `/omni sync` | Sync all OmniRoute models and combos to pi's Ctrl+P picker |
| `/omni setup` | Setup OmniRoute URL and API key |
| `/omni dashboard` | Show OmniRoute web dashboard URL |

### `/omni limits`

Fetches live quota data directly from the OmniRoute API:

```
─── antigravity/oscar@yulife (Free) ───
  gemini-3-pro-high: [████████░░░░░░░░░░░░] 60% left — resets 2026-04-12 09:55
  gemini-3-pro-low: [████████░░░░░░░░░░░░] 60% left — resets 2026-04-12 09:55
  + 13 more model(s) at 100%

─── codex/oscarharry@gmail.com (free) ───
  ❌ session: EXHAUSTED — resets 2026-04-12 08:39
  code_review: [░░░░░░░░░░░░░░░░░░░░] 100% left — resets 2026-04-15

─── kiro/OH@gm kiro (KIRO FREE) ───
  credit (0/50): [░░░░░░░░░░░░░░░░░░░░] 100% left — resets 2026-04-28
```



## How it works

Pi sends requests to OmniRoute, which routes them to the best available provider based on your combo strategy (priority, weighted, round-robin, least-used, etc.). After each response, the extension polls OmniRoute's call logs until it finds the entry for that turn, then displays the actual model and account in pi's status bar.

### Combos

Combos are model groups with routing strategies. For example:

```
RoundRobin [round-robin]:
  gemini-2.5-flash › gpt-5.4 › claude-sonnet-4.5 › gemini-3-flash
```

Create and edit combos in the OmniRoute dashboard or with `/omni combos`.

## Requirements

- [Pi Coding Agent](https://github.com/earendil-works/pi/tree/main/packages/coding-agent) v0.60.0+
- [OmniRoute](https://github.com/diegosouzapw/OmniRoute) v2.9.0+ running locally or on your network

## License

MIT
