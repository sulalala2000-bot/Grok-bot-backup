# Grok Bot box environment (high-level)

Export date: 2026-10-25 (Asia/Taipei). Public backup — secrets excluded.

## Layout (box)

| Path | Role |
|------|------|
| `/home/box/agent-data/workflows/` | User-installed skills (`*/SKILL.md`). Synced / authored skills live here. |
| `/home/box/agent-data/managed-skills/skills/` | Platform-managed skills (booking, shopping, sign-in, source-control, etc.). Not fully copied into this backup. |
| `/home/box/agent-data/plugins/` | Plugin cache under `cache/cursor-public/<plugin-id>/`. |
| `/home/box/agent-data/plugin-skills/` | Plugin skill index cache. |
| `/home/box/agent-data/agents/<uuid>/` | Per-agent profile, settings, automations, attachments, local store. |
| `/home/box/agent-data/agents/3f11adee-7640-4a93-9491-be5424fbf576/` | Agent **總經理** (this backup's profile target). |
| `/workspace/` | Scratch space for agents (shared machine FS). |
| `/cursor/stores/<store-id>/` | Persistent agent stores. |

## What this backup captures

- `skills/` — copies of user workflow `SKILL.md` files (many are Drive pointers).
- `agent-profile/` — scrubbed `profile.json` + `settings.json` for 總經理.
- `docs/plugins.md` — connector/plugin **names only**.
- `routines/` — note that automations were empty at export.
- `docs/ENVIRONMENT.md` — this file.
- `README.md` — bilingual restore notes.

## What lives only on Drive

Shared bidirectional skill library:

https://drive.google.com/drive/folders/1f7nTYNd2jdMLVeh1PCV7ajmLXfTFxYah

Thin pointer skills under `skills/` point back to that folder (and often a Windows mirror path under `G:\我的雲端硬碟\1_AI skills`).

## Explicitly excluded

- `box-secrets.json`, `host-secrets.json`, vault data
- `chrome-cookie-seed.json` and any cookies
- `store.db*` (agent local DB)
- `.env` files, passwords, API tokens
- Full email bodies; home/office addresses; phone numbers; credit cards
- Account-scope hashes from global `settings.json`
