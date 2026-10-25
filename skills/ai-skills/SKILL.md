---
name: 共用 AI skills 雙向同步
description: >-
  Use when creating, editing, importing, or syncing skills across Grok Bot / agy
  / Codex so they stay aligned with the shared Google Drive library.
---
# 共用 AI skills 雙向同步

## Source of truth

- Drive: https://drive.google.com/drive/folders/1f7nTYNd2jdMLVeh1PCV7ajmLXfTFxYah (id `1f7nTYNd2jdMLVeh1PCV7ajmLXfTFxYah`)
- Windows mirror (Google Drive for Desktop): `G:\我的雲端硬碟\1_AI skills`
- Categories in use: `01_生活與日常自動化`, `02_YouTube一條龍與發片企劃`, `03_AI影音製作模組`, `04_開發與程式輔助` (plus archive copies of Google / Antigravity builtins in 05–06)

## Bidirectional rules

1. **Read**: Prefer the skill folder under that G: path on the user PC before inventing a new recipe.
2. **Write**: After creating or editing a shared skill, write `SKILL.md` (and helpers) back into the matching category folder on G:. DriveFS syncs it to Google Drive for every agent.
3. **Do not** keep a divergent private copy of the same skill unless the user asks for a bot-only experiment; if you do, label it clearly.
4. **agy**: `~/.gemini/config/skills.json` already points at categories 01–04 on G:. New skills under those folders are discoverable after restarting `agy`.
5. **Codex / Antigravity host plugins**: Keep host-specific installers (e.g. grok-bridge plugin) local, but put the shared `SKILL.md` recipe in this library too.
6. **Grok Bot catalog**: Mirror discoverable skills into Bot skills with the same name; keep the G: path in the body as the helper/script location.

## Sync checklist

When the user says sync, or after a meaningful skill change:

1. List `G:\我的雲端硬碟\1_AI skills\0{1-4}_*` skill folders.
2. For each: ensure Bot skill exists/updated; ensure `SKILL.md` on G: matches.
3. Update `README.md` in the library if a skill was added/removed.
4. Remind that other Grok Bots pick up shared user memory + global Bot skills automatically; agy needs the G: path (already configured).
