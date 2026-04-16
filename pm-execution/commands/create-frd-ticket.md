---
description: Create Jira issues (Epic/Story/Dev-Task) from an FRD's ticket structure in section 9, with preview before creation and automatic FRD update with issue links
argument-hint: "<Notion FRD Link or file path with existing ticket section>"
---

# /create-frd-ticket — Create Jira Issues from FRD

Parse the ticket tree from FRD section 9 (Tasks), preview the issues, create them
in Jira (DHL project), and update the FRD with issue links.

**Prerequisite**: Run `/design-frd-ticket` first to design the ticket structure.

## Invocation

```
/create-frd-ticket https://www.notion.so/mvlchain/FRD-...
/create-frd-ticket ./FRD-feature-name.md
```

## Workflow

### Step 1: Load & Parse

Reads the FRD and parses section 9 (Tasks) to extract:
- Issue hierarchy: Epic > Story > Dev-Task
- Platform prefixes: `[server]`, `[web-admin]`, `[app-ios-rider]`, etc.
- TBD items (skipped, not created)

### Step 2: Preview

Shows a summary of all issues to be created before touching Jira:

```
==================================================
  Jira Issue Creation Preview
==================================================
  Project: DHL | Priority: Medium (all)
--------------------------------------------------
  [Epic] Feature Name
    [Story] [server] Business description
      [Dev-Task] [server] Task description
    [Story] [web-admin] Business description

  Skipped (TBD):
    - TBD: Social login scope pending

  Total: 1 Epic, 2 Stories, 1 Dev-Task = 4 issues
==================================================
```

Waits for PM confirmation before proceeding.

### Step 3: Create in Jira

Creates issues in order: Epic -> Story (linked to Epic) -> Dev-Task (linked to Story).

Jira fields:

| Field | Epic | Story | Dev-Task |
|-------|------|-------|----------|
| Project | DHL | DHL | DHL |
| Summary | Feature Name | [platform] Title | [platform] Title |
| Description | Section 1 + 6 content | Section 1 + 6 content | `Please check the story ticket.` |
| Priority | Medium | Medium | Medium |

### Step 4: Update FRD

Replaces ticket entries in section 9 with Jira issue links:

**Before:**
```markdown
- [Story] [server] Enable authentication
```

**After:**
```markdown
- [DHL-25460](https://mvlchain.atlassian.net/browse/DHL-25460) [server] Enable authentication
```

TBD items remain unchanged.

## Notes

- Requires `/design-frd-ticket` to have been run first (section 9 must exist).
- TBD items are never created — they stay as placeholders for future FRD updates.
- If any issue creation fails, successfully created issues are still linked in the FRD.
