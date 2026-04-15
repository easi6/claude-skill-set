# Claude Skill Set — mvlchain FRD Toolkit

Minimal Claude Code skill set built for the mvlchain PM workflow. One plugin,
two skills, one command — focused on **authoring and reviewing FRDs**.

## What's inside

- **`/write-frd`** — Interview-driven Functional Requirement Document authoring
  against the mvlchain 9-section FRD template. Runs a Socratic discovery
  interview (Why now? / KPIs / Scope / Assumptions / 5-Whys) before drafting,
  then fetches the live Notion template and produces an FRD matched to the
  organization's section and table structure.
- **`/review-frd`** — Multi-role review of an FRD from Developer, QA, and
  Designer perspectives. Surfaces only questions the PM can answer — no
  implementation-detail asks.

That's the whole toolkit. Other PM skills (discovery, strategy, GTM, etc.) were
intentionally removed — this repo targets a specific workflow, not general PM
work.

## Workflow

```
[Biz/Ops request]
     ↓
  /write-frd           →  interview PM → draft FRD (Notion 9-section template)
     ↓
  /review-frd        →  Dev / QA / Designer questions
```

## Installation

### Claude Cowork

1. Open **Customize** (bottom-left)
2. Go to **Browse plugins** → **Personal** → **+**
3. Select **Add marketplace from GitHub**
4. Enter: `easi6/claude-skill-set`

### Claude Code (CLI)

```bash
claude plugin marketplace add easi6/claude-skill-set
claude plugin install pm-execution@pm-skills
```

### Other AI assistants (skills only)

The `pm-execution/skills/*/SKILL.md` files follow the universal skill format
and work with any tool that reads it. Commands (`/slash-commands`) are
Claude-specific.

```bash
# Example: copy skills for OpenCode (project-level)
mkdir -p .opencode/skills/
cp -r pm-execution/skills/* .opencode/skills/
```

## Dependencies

- **Notion MCP** — `/write-frd` and `/review-frd` both call `notion-fetch`
  to load the mvlchain FRD template. The Notion plugin must be connected.
- **Atlassian MCP** (optional) — `/review-frd` can accept Jira links via
  `getJiraIssue`.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
