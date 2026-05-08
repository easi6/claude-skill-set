---
description: Design and create Jira tickets (Epic/Story/Dev-Task) from an FRD — analyzes scope, confirms platforms, collaborates on breakdown, creates issues in Jira, updates FRD with links
argument-hint: "<Notion FRD Link or file path>"
---

# /create-frd-ticket — Create Jira Tickets from FRD

Analyze an FRD, collaboratively design a Jira ticket structure with the PM, create
all issues in Jira (DHL project), and update the FRD with issue links.

## Invocation

```
/create-frd-ticket https://www.notion.so/mvlchain/FRD-...
/create-frd-ticket ./FRD-feature-name.md
```

## Workflow

### Step 1: Load & Analyze FRD

Reads the FRD and determines:
- **Epic necessity** — recommends Epic for multi-phase, cross-platform, or large features
- **Target platforms** — infers from FRD content (features, user stories, design refs)

### Step 2: Confirm with PM

Two AskUserQuestion prompts:
1. Epic necessity (AI default provided with rationale)
2. Target platforms (AI-inferred defaults, multi-select)

### Step 3: Propose & Collaborate

AI proposes an initial ticket tree, then enters free-form conversation:

```markdown
- [Epic] Feature Name
  - [Story] [server] Enable user authentication for login flow
    - [Dev-Task] [server] Enable user authentication for login flow
  - [Story] [web-admin] Manage user access permissions
  - [Story] [app-all-rider] Add login screen for riders
    - TBD: Social login scope pending
```

The PM directs: merge, split, remove, add, reorder tickets until satisfied.

### Step 4: Preview & Create in Jira

Shows a summary preview, confirms with PM, then creates issues in order:
Epic → Story (linked to Epic) → Dev-Task (linked to Story).

| Field | Epic | Story | Dev-Task |
|-------|------|-------|----------|
| Project | DHL | DHL | DHL |
| Summary | Feature Name | [platform] Title | [platform] Title |
| Description | Section 1 + 6 (markdown) | Section 1 + 6 (markdown) | `Please check the story ticket.` |
| Priority | Medium | Medium | Medium |

### Step 5: Update FRD

Replaces ticket entries in section 9 with Jira issue links:

```markdown
- [DHL-25460](https://mvlchain.atlassian.net/browse/DHL-25460) [server] Enable authentication
```

TBD items remain unchanged.

## Ticket Rules

- **Summaries**: English, non-technical, business/user-facing tone
- **Dev-Task summary**: Same as parent Story summary
- **Dev-Task**: Optional — PM decides granularity
- **TBD**: Marks unfinalized scope, not created as Jira issues
