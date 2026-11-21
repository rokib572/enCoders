# Privacy Policy

**enCoders** · Last updated: 2026-11-07

enCoders is a Claude Code plugin that runs entirely on your machine. It has no server, no account, and no telemetry. The author of enCoders never receives, sees, or stores any of your data.

## What enCoders reads

- Files in the project you run it in (source code, `.enc-workflow.yml`, plans, tasks), so its agents can plan, implement, and review work.
- It deliberately **refuses** to read secret files (`.env*`, keys, credentials, and the other patterns in `secret_globs`), even when asked: `/enc:init` writes Read, Edit, and Bash deny rules for every secret pattern into your project's `.claude/settings.json`, which Claude Code enforces in every session.

enCoders does not collect personal data such as names, emails, or addresses.

## What enCoders writes

Only files inside your project, for example `.enc-workflow.yml`, `.enc/state.json`, `docs/plans/`, `docs/tasks/`, `docs/PROGRESS.md`, and `.claude/settings.json`. Everything stays on your machine and under your version control. Delete those files and enCoders' data is gone.

## What enCoders sends off your machine

**Nothing.** The plugin ships no executable code at all (it is Markdown, JSON, YAML, and one icon) and runs no hooks. Nothing in it can make a network call on its own.

**Claude itself.** enCoders runs inside Claude Code, so your prompts and the files Claude reads are processed by Anthropic under [Anthropic's privacy policy](https://www.anthropic.com/legal/privacy), exactly as in any Claude Code session. enCoders adds no other recipient.

## Retention

enCoders operates no service and retains nothing. Files it writes live in your project for as long as you keep them.

## Children

enCoders is a software development tool and is not intended for anyone under 18.

## Changes and contact

Changes to this policy are published in this file in the [repository](https://github.com/rokib572/enCoders). Questions: support@encoderstech.com.
