---
name: help
description: Explains where to start with enCoders in a few lines and lists every enCoders command with a one-line explanation in a table. A pure read that changes nothing. Use when the user asks for help with enCoders, what commands exist, how to get started, or which command fits what they want to do.
compatibility: Requires Claude Code (terminal, IDE extension, or Desktop Code tab)
---

# enc:help

A PURE READ: never modify config, state, or any project file while
executing this skill, and never start another command on the user's
behalf. Render the sections below and stop.

## Step 0 — environment gate

Run `test "$CLAUDECODE" = "1"` with Bash. If it fails, STOP and show
exactly: "enCoders runs only in Claude Code (terminal, IDE, or the Desktop Code
tab). Your project config and brief are untouched — open this repo in
Claude Code to continue."

## Step 1 — where to start

Check whether `.enc-workflow.yml` exists at the project root, then render
**Where to start** as at most five short lines:

- No config yet: "This project is not set up for enCoders. Start with
  `/enc:init`. It detects whether the repo is new or existing, asks a few
  questions, and writes the config. Have only an idea so far? Run
  `/enc:brainstorm` first; it needs no config."
- Config present: "enCoders is set up here. Run `/enc:status` to see what is in
  flight and what to do next."

Then one line with the usual flow, verbatim:

`/enc:init` → `/enc:brainstorm` → `/enc:plan` → `/enc:implement-phase` (one phase at a time; you run the full suite, review, and do git at each gate) → next phase

And one line for the side paths: something broken or slow → `/enc:diagnose`;
review any change → `/enc:code-review`; settings → `/enc:configure`.

## Step 2 — the command table

Render this table verbatim under the heading **Commands**:

| Command | What it does |
|---|---|
| `/enc:help` | This overview: where to start and every command |
| `/enc:init` | Sets up enCoders in any project: detects the scenario, interviews you, writes `.enc-workflow.yml`, scaffolds, runs the doctor |
| `/enc:brainstorm [scope]` | Turns a raw idea into a written brief (`docs/brief.md` or a feature brief). Needs no config |
| `/enc:plan` | Breaks a feature into independently shippable phases in `docs/plans/<feature>.md` |
| `/enc:implement-phase [id]` | Builds one phase through the agent pipeline and review gates, then stops at your gate. No argument resumes the phase in flight |
| `/enc:code-review [target\|prompt]` | Runs the review pipeline over a diff, path, staged or uncommitted work, or a plain-English scope |
| `/enc:diagnose [symptom]` | Finds the root cause of a bug or slowdown through a gated loop, then offers to plan the fix. Never applies one |
| `/enc:create-task "<desc>"` | Captures work as `docs/tasks/<slug>.md` |
| `/enc:status` | Shows where the workflow stands: mode, phase, pending gates, carried-forward debts, token usage. Changes nothing |
| `/enc:configure [domain]` | Views or changes any config value or coding convention, as a diff you approve, then runs the doctor |
| `/enc:mode [enforced\|advisory\|off]` | Shows or changes enCoders mode: the doctrine rendered into CLAUDE.md; enforced, advisory, off |
| `/enc:add-agent` | Adds a custom agent or specialist reviewer to the roster, with its tool limits in an agent file |
| `/enc:upgrade` | Brings enCoders config, CLAUDE.md guides, settings denies, and scaffold up to the installed plugin and the repo as it is now, as diffs you approve, then runs the doctor |
| `/enc:doctor` | Checks that config, repo, agents, models, and environment still agree (checks 0–9, run by hand) |

Close with one line: "`testing`, `simplicity`, and `domain-modeling` are
reference skills that enCoders reads on its own; they are not commands."
