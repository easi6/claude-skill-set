---
description: Design a Jira ticket structure (Epic/Story/Dev-Task) from an FRD — analyzes scope, confirms platforms, collaborates on breakdown, appends to FRD section 9
argument-hint: "<Notion FRD Link or file path>"
---

# /design-frd-ticket — Design Jira Tickets from FRD

Analyze an FRD and collaboratively design a Jira ticket structure with the PM.
Appends the finalized ticket tree to the FRD's section 9 (Tasks).

## Invocation

```
/design-frd-ticket https://www.notion.so/mvlchain/FRD-...
/design-frd-ticket ./FRD-feature-name.md
```

## Workflow

### Step 1: Load & Analyze FRD

Reads the FRD and determines:
- **Epic necessity** — recommends Epic for multi-phase, cross-platform, or large features
- **Target platforms** — infers from FRD content (features, user stories, design refs)

Platform options: `server`, `web-admin`, `web-corp`, `web-homepage`, `web-all`,
`app-ios-driver`, `app-and-driver`, `app-ios-rider`, `app-and-rider`,
`app-ios-all`, `app-and-all`, `app-all-driver`, `app-all-rider`

### Step 2: Confirm with PM

Two AskUserQuestion prompts:
1. Epic necessity (AI default provided with rationale)
2. Target platforms (AI-inferred defaults, multi-select)

### Step 3: Propose & Collaborate

AI proposes an initial ticket tree, then enters free-form conversation:

```markdown
- [Epic] Feature Name
  - [Story] [server] Enable user authentication for login flow
    - [Dev-Task] [server] Set up authentication service
  - [Story] [web-admin] Manage user access permissions
  - [Story] [app-all-rider] Add login screen for riders
    - TBD: Social login scope pending
```

The PM directs: merge, split, remove, add, reorder tickets until satisfied.

### Step 4: Append to FRD

Writes the confirmed tree to FRD section 9 (Tasks).

## Ticket Rules

- **Summaries**: English, non-technical, business/user-facing tone
- **Epic summary**: `[Region-Only] Feature Name` or plain `Feature Name`
- **Story/Dev-Task summary**: `[platform] Description`
- **Dev-Task**: Optional — PM decides granularity
- **TBD**: Marks unfinalized scope, not created as Jira issues

## Next Step

After designing tickets:
> `/create-frd-ticket {same FRD link}` — creates the issues in Jira
