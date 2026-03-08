# gh-plan-to-issues (Codex skill)

Create GitHub Issues from a plan/spec using the GitHub CLI (`gh`).

It creates:
- **Exactly 1 epic** issue
- **Up to 10 task** issues
- A **task checklist** on the epic (e.g. `- [ ] #123`)
- A consistent **link back to the epic** in each task body (e.g. `Epic: #229`)

This skill exists to make the intent unambiguous: when you ask to “create issues”, Codex should **actually run `gh`** to create them (not just print Markdown drafts/commands).

## Requirements

- `gh` installed and authenticated (`gh auth status`)
- Node.js (to run the scripts)

## Install

Copy this folder into your Codex skills directory and restart the Codex app (skills are loaded on startup).

Common locations:
- `~/.codex/skills/gh-plan-to-issues/`
- `$CODEX_HOME/skills/gh-plan-to-issues/` (if `CODEX_HOME` is set)

## Validate the skill

Run:

```bash
node scripts/validate_skill.mjs
```

## Usage

In practice, you usually just ask Codex (with this skill) to create issues from the plan. If you want to run it yourself, use the scripts below.

Create a JSON spec (start from `references/spec.example.json`), then dry-run:

```bash
node scripts/create_issues_from_spec.mjs /path/to/spec.json --dry-run
```

Create issues:

```bash
node scripts/create_issues_from_spec.mjs /path/to/spec.json
```

### Duplicate handling

By default the script aborts if it finds issues with matching titles.
To override:

```bash
node scripts/create_issues_from_spec.mjs /path/to/spec.json --allow-duplicates
```

## What the script does

`scripts/create_issues_from_spec.mjs`:
- Infers the repo (or uses `repo` from the spec)
- Filters requested labels to labels that already exist in the repo
- Creates the epic + tasks
- Updates the epic body to contain a `## Tasks` checklist referencing created tasks

## Optional helper: Markdown plan -> JSON spec

If you already have a Markdown plan/spec, generate a starter JSON spec:

```bash
node scripts/spec_from_markdown.mjs /path/to/plan.md > /tmp/gh-plan-to-issues-spec.json
node scripts/create_issues_from_spec.mjs /tmp/gh-plan-to-issues-spec.json --dry-run
```

You can also pipe directly:

```bash
node scripts/spec_from_markdown.mjs /path/to/plan.md | node scripts/create_issues_from_spec.mjs - --dry-run
```
