# Claude Skill Set — mvlchain FRD Toolkit

FRD lifecycle toolkit for the mvlchain PM workflow — authoring, review, ticket
design, and Jira creation.

## What's inside

- **`/write-frd`** — Interview-driven Functional Requirement Document authoring
  against the mvlchain 9-section FRD template. Runs a Socratic discovery
  interview (Why now? / KPIs / Scope / Assumptions / 5-Whys) before drafting,
  then fetches the live Notion template and produces an FRD matched to the
  organization's section and table structure.
- **`/review-frd`** — Multi-role review of an FRD from Developer, QA, and
  Designer perspectives. Role reviews are deduplicated internally — the PM
  sees a single consolidated question list with no repeated findings.
- **`/create-frd-ticket`** — Design and create Jira tickets from an FRD.
  Analyzes scope, confirms Epic necessity and target platforms with the PM,
  collaborates on ticket breakdown (Epic/Story/Dev-Task), creates all issues
  in the DHL project, then updates the FRD with Jira issue links.

## Workflow

```
[Biz/Ops request]
     ↓
  /write-frd              →  interview PM → draft FRD (Notion 9-section template)
     ↓
  /review-frd             →  deduplicated Dev / QA / Designer questions
     ↓
  /create-frd-ticket      →  design tickets + create in Jira + update FRD with links
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

- **Notion MCP** — `/write-frd`, `/review-frd`, and `/create-frd-ticket` use
  `notion-fetch` to load and update FRD pages.
- **Atlassian MCP** — `/review-frd` accepts Jira links via `getJiraIssue`.
  `/create-frd-ticket` creates issues via `createJiraIssue` in the DHL project.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
