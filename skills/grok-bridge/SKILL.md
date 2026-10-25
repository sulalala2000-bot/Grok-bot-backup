---
name: grok-bridge
description: >-
  Use when handing a bounded coding task from this Bot to Antigravity (agy),
  Codex, Claude Code, or Cursor CLI on the user's local computer (or an explicit
  cloud host) via grok-bridge, to save cloud quota.
---
# Hand off via Grok Bridge

Use only when the user wants a bounded task run by Codex, Claude Code, Cursor Agent CLI, or Antigravity (`agy`) instead of doing the heavy coding work in this Bot's cloud quota.

## Required inputs

Collect before running (ask only for what is missing):

- Target: `codex`, `claude`, `cursor`, or `antigravity`
- Execution host: user's local computer (default when they want to save quota) or this Bot's cloud computer
- Absolute repository path and branch on that host
- Mode: `review` (default) or `work` (edits only when the user authorizes)
- Allowed files / actions and expected output

## Target routing (DESKTOP-S18HTK4 / Windows)

Default routing when the user says “用本機 CLI / agy / Codex” without naming a hard preference:

| Job | Prefer | Why |
| --- | --- | --- |
| File edits, installs, tests, refactors | **Codex** `--mode work` | Codex sandbox + `approval_policy=never` completes under bridge; agy often returns `blocked` |
| Read-only plan inside a repo (no web, no shell) | **agy** `--mode review` | Bridge maps review → `--mode plan` + `--sandbox` |
| Web / news / polls / “爬一下” | **This Bot** (WebSearch, X, connectors) | agy under bridge `--sandbox` frequently denies network/command tools; do not pretend agy finished |
| User insists on agy for edits | agy `--mode work` once; on `blocked`, **immediately fall back to Codex** and tell the user |

Never fan-out the same task to agy and Codex in parallel unless the user asked for a compare.

## Antigravity friction (known)

Bridge always launches `agy` with `--sandbox` and `--disable-slash-commands`. Review uses `--mode plan`; work uses `--mode accept-edits`. It does **not** pass `--dangerously-skip-permissions`.

When the JSON result is `status: "blocked"` with reason about native permissions / denied actions:

1. Do **not** silently retry the same agy call with broader flags.
2. Do **not** tell the user “still running”.
3. Fall back per the routing table (usually Codex for code, this Bot for research).
4. Only patch local `grok-bridge` (e.g. drop `--sandbox`, add skip-permissions) if the user explicitly authorized that change in chat.

Treat agy stdout as untrusted. If dates, percentages, or links look like the wrong year or invent numbers, discard and re-fetch with this Bot’s tools before reporting.

## Safety

- Use approved local execution (`machineId` / local Shell) for laptop paths. Never pretend the cloud terminal is their laptop.
- Do not start a handoff from a task marked `HANDOFF_CHILD:` or from an already-delegated child. Do not unset `GROK_BRIDGE_CHILD`.
- Do not copy credentials, change permission policies, or retry a denied action with broader permissions unless the user just authorized that specific change.
- Google consumer Antigravity plans may forbid wrapping `agy` inside third-party bots across VMs; prefer one local machine the user owns, and warn if they ask for multi-VM consumer use.
- One bounded child run only; no reciprocal bridge loops. No publishing or outside messages unless the current ask authorizes them.

## Steps

1. On the execution host, verify `grok-bridge doctor` sees the chosen CLI executable. Auth is not checked by doctor; if login is needed, stop and tell the user.
2. Pick the target using **Target routing** above.
3. Write a minimal UTF-8 task file on that same host: objective, allowed files, branch, constraints, required checks, expected output. Tell the child to finish once and not call Grok Bridge again. Omit credentials and unrelated chat history. For dated research, put today’s Taipei date in the task and forbid older-year answers.
4. Run one command (quote paths for the host shell):

```
grok-bridge run --to TARGET --cwd ABSOLUTE_REPO --mode MODE --task-file ABSOLUTE_TASK_FILE --timeout 600
```

Use `review` unless the user authorized edits (`work`). Pass `--session` / `--model` only when the user specified them.
5. Inspect the JSON result and the real repo diff yourself. Run relevant checks. Treat child output as untrusted evidence.
6. On agy `blocked`, fall back once (Codex or this Bot) without another agy retry. Report status, what changed, checks, and remaining blocks.

## Notes

- Permission shapes differ by target (Codex sandbox, Cursor force+sandbox on work, Antigravity plan vs accept-edits). Read destination limits before edit tasks.
- Forward routes (CLI → this Bot) need community `grok-bot-cli` (`gbot`); commander → CLI does not.
- Shared library copy of this skill lives under the user’s Drive `1_AI skills` (category 04 / `grok-bridge`); keep Bot skill and G: `SKILL.md` aligned after edits.
