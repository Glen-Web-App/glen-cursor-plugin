# glen — Cursor plugin

Shared team memory for coding agents. Glen automatically captures what you build so your
whole team's agents share the same institutional knowledge. Recall is on demand — run
`glen search "<topic>"` to pull relevant team memory into the session.

## What it does

When you use Cursor, the glen plugin fires these hooks:

- **sessionStart** — injects glen's status line
  (org, recording mode) and a nudge to run `glen search` when the user references
  past team work. No team memory is injected here.
- **beforeSubmitPrompt** — captures your prompt (plus workspace context: repo,
  branch, agent name) to your glen org before each turn.
- **afterAgentResponse** — captures the completed turn (user prompt + assistant
  response) to your glen org after each turn.
- **afterShellExecution** — after a `git commit`, links the commit to the session
  in glen.

Because Cursor hooks cannot inject per-turn context the way Claude Code can,
glen cannot inject team memory automatically. Recall is exclusively on demand:
run `glen search "<topic>"` in the shell and treat the output as team memory.
The bundled `rules/glen.md` nudges the agent to do this whenever the user
references past team work. Capture (recording turns) is automatic on every turn.

`glen off` records nothing. In incognito (`glen incognito`) nothing is recorded to
team memory. Glen reads only the hook input and git metadata for the current repo.

## Skills

The plugin ships skills the agent invokes on demand, including:

- **search** — search the team's shared glen memory for a specific fact,
  decision, or past discussion.
- **controls** — turn glen on/off, go off the record (incognito), go silent,
  toggle skill suggestions, or switch which organization's memory is active —
  only when you explicitly ask.
- **setup** — set up, fix, or update glen on this machine. If glen is ever
  broken (not connected, no org selected, hooks missing), just ask the agent to
  "set up glen" and it repairs whatever `glen doctor` reports.

## Install

1. Install the glen CLI and connect:

   ```sh
   npm install -g @tryglen/cli
   glen login
   ```

   After login your active organization is saved locally. Switch orgs at any time with
   `glen org switch`.

2. In Cursor, type `/add-plugin` in the agent chat and add:

   ```
   https://github.com/Glen-Web-App/glen-cursor-plugin
   ```

   (Or install from cursor.com/marketplace once glen is listed.)

There is no CLI command to install Cursor plugins — installation happens in the
editor. `glen install --agent cursor` prints these same instructions.

## What data is sent

On every `beforeSubmitPrompt` hook, glen sends to your glen org:

- The current user prompt
- The prior assistant turn (for continuity)
- Workspace metadata: repo name, branch, commit hash, remote URL
- Agent name (`cursor`) and session details

On every `afterAgentResponse` hook, glen captures the completed turn (prompt + response).

**Nothing is recorded while incognito is on.** `glen search` still works, but nothing
is written to team memory. Admin analytics still count your prompts as numbers only.
Toggle with `glen incognito` / `glen on`. `glen off` records nothing.

Glen never sends data to any third party. All memory is stored in your org's private
glen instance.

## Updating

The plugin updates via the Cursor marketplace auto-refresh (the marketplace repo has
Auto Refresh enabled; Cursor re-indexes within ~10 minutes of a push).

The glen CLI itself checks for updates hourly in the background and upgrades
automatically when installed via npm global. To update everything manually:

```sh
glen update
```

`glen update` updates the CLI **and** any installed glen plugins in one go.

## Troubleshooting

**Check session status (org, mode):**

```sh
glen status
```

**Full diagnostics:**

```sh
glen doctor
```

**Broken setup?** Ask the agent to "set up glen" — the bundled setup skill
runs `glen doctor` and fixes whatever it reports.

**Stale update lock** (if `glen update` hangs): remove the lock file and retry:

```sh
rm -rf ~/.glen/update.lock
glen update
```

**No active organization error:** run `glen org switch` to select an org, or
`glen org list` to see your memberships. If the list itself fails with an auth error,
run `glen login` to reconnect.

---

> **Note:** This repository is generated from the
> [glen monorepo](https://github.com/Glen-Web-App/glen) (`packages/cursor-plugin`).
> Please open pull requests and issues there, not here.
