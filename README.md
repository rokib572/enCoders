<p align="center">
  <img src=".claude-plugin/logo.png" alt="enCoders logo" width="160">
</p>

# enCoders

[![version](https://img.shields.io/badge/enCoders-1.0.0-blue)](.claude-plugin/plugin.json)
[![license](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![requires](https://img.shields.io/badge/requires-Claude_Code-8A2BE2)](https://code.claude.com)

**A supervised, multi-agent development workflow for Claude Code.** A team of specialised agents plans, builds, tests, and reviews your feature one phase at a time. The main session only orchestrates, and every phase stops at a gate that only you can open.

> **Requires Claude Code.** enCoders runs in the terminal CLI, the IDE extensions, or the Desktop app's Code tab. It does not run in Cowork or Chat.

## Install

```
/plugin marketplace add rokib572/enCoders
/plugin install enc@encoders
```

Then open your project and run `/enc:init`. Not sure where to begin? Run `/enc:help`.

## How it works

1. **`/enc:brainstorm`** turns an idea into a written brief. It asks no setup questions.
2. **`/enc:init`** interviews you, writes one config file (`.enc-workflow.yml`), and scaffolds the workflow.
3. **`/enc:plan`** breaks a feature into small, independently shippable phases.
4. **`/enc:implement-phase`** builds one phase: a coder agent writes the code, a test agent writes and runs scoped tests, and separate reviewer agents check logic, security, and simplicity.
5. **You hold the gate.** enCoders stops after each phase. You run the full test suite, review the diff, do the git work, and start the next phase yourself.

## Example prompts

**1. Start from an idea**

```
/enc:brainstorm a habit tracker where users log daily habits and see weekly streaks
```

You get a guided conversation about the problem, users, and MVP scope, ending in `docs/brief.md`.

**2. Plan and build a feature**

```
/enc:plan add password reset by email to the auth module
```

enCoders reads your codebase and writes a phased plan to `docs/plans/`. After you approve it, run `/enc:implement-phase` to build the first phase through the agent pipeline and review gates.

**3. Find the cause of a bug**

```
/enc:diagnose the checkout total is wrong when a discount code and free shipping are both applied
```

enCoders reproduces the bug, narrows it down, and reports the confirmed root cause. It never applies a fix on its own; it offers to plan one.

More things you can ask:

- `/enc:code-review the auth module, watch for race conditions`
- `/enc:status` to see what is in flight and what to do next
- `/enc:configure` to change any setting, shown as a diff you approve

## Commands

Every command is typed by you; nothing here runs on its own. Arguments in square brackets are optional.

### Getting started

**`/enc:help`**
Explains where to start and lists every command. If the project is not set up yet it points you to `/enc:init`; if it is, it points you to `/enc:status`. It changes nothing.

**`/enc:init`**
Sets up enCoders in a project. It detects your situation (empty directory, existing repository, freshly scaffolded project, or an earlier Claude setup to adopt), then interviews you about architecture and coding conventions. It writes `.enc-workflow.yml`, scaffolds the workflow files, proposes the deny rules for `.claude/settings.json`, and finishes by running the doctor until it is green.

**`/enc:brainstorm [scope]`**
Turns a raw idea into a written brief through a guided conversation: the problem, the users, the MVP features, and the non-goals. It asks no stack, tooling, or setup questions and needs no config. The result is `docs/brief.md` for a product, or a feature brief for an existing repository.

### Planning and building

**`/enc:plan`**
Breaks a feature into phases. It reads the real codebase first, then writes `docs/plans/<feature>.md` with independently shippable phases, each sized for one implementation run and each declaring where its tests will observe behaviour from. It flags speculative scope before you see the plan, and presents the plan for your approval.

**`/enc:implement-phase [id]`**
Builds one phase of a plan. It shows you the dispatch plan first: which agents run, on which layers, and which review gates apply. After you approve, the coder implements each layer, the test agent writes and runs scoped tests, and the reviewers check the result in the order your config sets. A failed gate goes back to the coder with the reviewer's findings, and only that gate re-runs. When the phase is done it reports results and token usage, then stops at your gate. With no argument it resumes the phase in flight, even in a fresh session.

**`/enc:create-task "<description>"`**
Captures a piece of work without planning it yet. It writes `docs/tasks/<slug>.md` with a title, acceptance criteria, affected layers, and size.

### Reviewing and debugging

**`/enc:code-review [target or prompt]`**
Runs the configured review pipeline over a diff, a path, staged changes, all uncommitted work, or a scope described in plain English, such as "the auth module, watch for race conditions". Findings are reported by severity with file and line.

**`/enc:diagnose [symptom]`**
Finds the root cause of a bug or performance regression through a gated loop. It builds a check that fails on this specific bug, minimises the reproduction, ranks hypotheses that can be proven wrong, and tests them one variable at a time. It reports the confirmed cause and offers to plan a fix. It never applies one.

### Checking and changing the setup

**`/enc:status`**
Shows where the workflow stands: the mode, the active phase, where the pipeline is, pending human gates, carried-forward debts, and token usage. It is a pure read and changes nothing. Use it when you pick work back up.

**`/enc:doctor`**
Checks that the config and reality still agree, across ten checks: environment, config schema, paths, commands, agent roster, models, rules, secrets hygiene, pipeline wiring, and workflow state. It runs your configured lint and typecheck commands to prove they pass. It runs automatically after every config change, and you can run it by hand at any time.

**`/enc:configure [domain]`**
Views or changes any setting: profile, agents, models, autonomy, review pipeline, git level, commands, or coding conventions. Every change is shown as a diff, applied only when you approve it, and followed by a doctor run.

**`/enc:mode [enforced|advisory|off]`**
Shows or changes how strictly the orchestrator-only rule applies. In `enforced` (the default) the main session must hand every code change to an agent. `advisory` keeps the same guidance with a softer rule. `off` removes it. Secret, git, and full-test-suite protections stay on in every mode.

**`/enc:add-agent`**
Adds your own agent or specialist reviewer to the roster. It asks for a name, purpose, model, and autonomy, suggests sensible tool limits, and shows the config change as a diff before applying it.

**`/enc:upgrade`**
Brings a project's setup up to the installed plugin version and the repository as it is now. It sweeps the config schema, new and retired settings, new agents, model ids, the `CLAUDE.md` guides, the deny rules, and the scaffold files. Each change is a diff you approve, and it ends with a doctor run compared against the starting one.

### Reference skills

`testing`, `simplicity`, and `domain-modeling` are not commands. They are guidance the agents read on their own: how to write tests worth keeping, how to keep a change small, and how to keep one shared vocabulary for the project.

## Agents

| Agent | Role |
|---|---|
| `architect` | Design decisions and architecture decision records |
| `coder` | Implements one layer of a phase |
| `e2e-test` | Writes and runs its own test files |
| `ui-design` | Frontend components, when the project has a web layer |
| `code-reviewer` | Logic correctness and plan conformance |
| `security-check` | Authorization, injection, tenancy, and secret hygiene |
| `simplicity-check` | Over-engineering and duplication |
| `mechanical-gate` | Runs lint and typecheck; has no write tools |
| `code-styling` | Naming, import, and formatting fixes |

## What the plugin does on your machine

The plugin ships no executable code and no hooks: it is Markdown, JSON, YAML, and one logo. Everything it does, it does through Claude Code in your project:

- **Writes files in your project:** `.enc-workflow.yml`, `.enc/state.json`, `CLAUDE.md` (inside a marked region), files under `docs/`, and agent files under `.claude/agents/`.
- **Adds deny rules to your project's `.claude/settings.json`**, shown as a diff you approve. They stop agents from reading secret files (`.env`, keys, credentials), running git operations you have not allowed, and running your full test suite. It only adds denies, never allows, and records what it wrote in `.claude/enc-denies.json`.
- **Runs your project's own commands:** `/enc:doctor` executes the lint and typecheck commands named in `.enc-workflow.yml`, and agents run tests by file path. Read the config of a repository you just cloned before running the doctor, as you would a `package.json` scripts block.
- **Sends nothing anywhere.** No server, no account, no telemetry. See [PRIVACY.md](PRIVACY.md).

Detailed procedures live in [`references/`](references/), and each command's full behaviour is in its file under [`skills/`](skills/).

## Support

Questions and issues: [GitHub issues](https://github.com/rokib572/enCoders/issues) or support@encoderstech.com.

## License

[MIT](LICENSE) © EncodersTech
