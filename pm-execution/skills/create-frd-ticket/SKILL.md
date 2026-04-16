---
name: create-frd-ticket
description: "Create Jira issues from an FRD's ticket structure (section 9 Tasks). Parses the designed ticket tree, shows a preview, and creates Epic/Story/Dev-Task issues in the DHL project with proper linking. After creation, replaces the ticket tree in the FRD with Jira issue links. Requires /design-frd-ticket to have been run first. Use when a PM wants to create Jira tickets from a finalized FRD ticket plan."
hint: "<Notion FRD Link or file path with existing ticket section>"
---

# Create FRD Ticket

## Purpose

You create Jira issues from a finalized ticket structure in an FRD. The ticket tree
was designed by `/design-frd-ticket` and lives in the FRD's section 9 (Tasks).
Your job is to parse it, preview it, create the issues in Jira, and update the FRD
with issue links.

## Prerequisites

- The FRD must have a **section 9 (Tasks)** with a ticket tree.
- If section 9 is missing or empty, tell the PM to run `/design-frd-ticket` first.

---

## Step 0: Load the FRD

The user will provide the FRD source. Determine the type:

1. **Notion URL** (contains `notion.so`): Use the Notion MCP tool (`notion-fetch`) to read the page.
2. **File path**: Read the file directly.
3. **No input provided**: Ask the user to provide the FRD source.

Once loaded, confirm:
> "FRD loaded: **{title}**"

---

## Step 1: Parse Ticket Tree

Parse section 9 (Tasks) to extract the ticket tree. Identify:

- **Issue type**: `[Epic]`, `[Story]`, `[Dev-Task]`
- **Platform prefix**: `[server]`, `[web-admin]`, `[app-ios-rider]`, etc.
- **Summary**: The text after the type/platform tags
- **Hierarchy**: Parent-child relationships from indentation
- **TBD items**: Skip these — do NOT create Jira issues for TBD entries

---

## Step 2: Extract Description Content

From the FRD, extract content for the `description` field:

### What Section

Gather content from these FRD sections **by section number** (the exact heading text
may vary between FRDs — match by the leading number, not the title):
- **Section 1** (e.g., "Context & Objectives", "Background & Objectives") — Purpose, KPIs, Scope
- **Section 6** (e.g., "Release Plan") — Phases, timeline, milestones

This combined content is the "What section" used for Epic and Story descriptions.

### Description Mapping

| Issue Type | Description |
|-----------|-------------|
| Epic | What section content |
| Story | What section content |
| Dev-Task | `Please check the story ticket.` |

---

## Step 3: Preview

Before creating any issues, present a preview to the PM.

### Preview Format

```
==================================================
  Jira Issue Creation Preview
==================================================
  Project: DHL
  Priority: Medium (all)
--------------------------------------------------

  [Epic] Feature Name
    [Story] [server] Business description
      [Dev-Task] [server] Task description
      [Dev-Task] [server] Another task
    [Story] [web-admin] Business description
    [Story] [app-all-rider] Business description

  Skipped (TBD):
    - TBD: Scope for social login pending

  Total: 1 Epic, 3 Stories, 2 Dev-Tasks = 6 issues
==================================================
```

Ask the PM to confirm:
> "Proceed with creating these {N} issues in Jira?"

**Do NOT create any issues until the PM confirms.**

---

## Step 4: Create Issues in Jira

Use the Atlassian MCP tools to create issues in the following order:

### Creation Order

1. **Epic first** (if exists) — create and capture the issue key
2. **Stories** — create with Epic link (if Epic exists), capture issue keys
3. **Dev-Tasks** — create with parent Story link, capture issue keys

### Jira Field Values

| Field | Epic | Story | Dev-Task |
|-------|------|-------|----------|
| Project | DHL | DHL | DHL |
| Issue Type | Epic | Story | Dev-Task |
| Summary | `Feature Name` | `[platform] Title` | `[platform] Title` |
| Description | What section | What section | `Please check the story ticket.` |
| Priority | Medium | Medium | Medium |

### Jira Configuration

- **Cloud ID**: `mvlchain.atlassian.net`
- **Project Key**: `DHL`

### Error Handling

- If an issue creation fails, report the error and continue with remaining issues.
- After all attempts, list which issues succeeded and which failed.
- Do NOT retry failed issues automatically — let the PM decide.

---

## Step 5: Update FRD with Issue Links

After all issues are created, update the FRD's section 9 (Tasks) by replacing
ticket entries with Jira issue links.

### Link Format

Replace each created ticket entry with its Jira link:

**Before:**
```markdown
# 9. Tasks

- [Epic] Feature Name
  - [Story] [server] Business description
    - [Dev-Task] [server] Task description
  - TBD: Pending scope decision
```

**After:**
```markdown
# 9. Tasks

- [DHL-25459](https://mvlchain.atlassian.net/browse/DHL-25459) Feature Name
  - [DHL-25460](https://mvlchain.atlassian.net/browse/DHL-25460) [server] Business description
    - [DHL-25461](https://mvlchain.atlassian.net/browse/DHL-25461) [server] Task description
  - TBD: Pending scope decision
```

### Update Rules

1. Replace `[Epic]`, `[Story]`, `[Dev-Task]` tags with `[ISSUE-KEY](URL)`.
2. Keep platform prefix `[platform]` in Story and Dev-Task lines.
3. Keep `TBD:` items unchanged — they are not Jira issues.
4. Preserve the indentation hierarchy.

### Output Location

- **Notion URL source**: Update the Notion page directly using Notion MCP tools.
- **File source**: Edit the file directly.
- **Pasted text**: Present the updated section for the PM to copy.

---

## Step 6: Summary

After completion, present a summary:

```
Created {N} issues in DHL:
  Epic:     DHL-25459 — Feature Name
  Story:    DHL-25460 — [server] Business description
  Dev-Task: DHL-25461 — [server] Task description
  ...

FRD Section 9 (Tasks) updated with issue links.
```

---

## Notes

- This skill only creates issues — it does NOT design ticket structure.
  Use `/design-frd-ticket` first.
- TBD items are never created in Jira.
- All summaries preserve the English, non-technical tone from the design phase.
- If any issue creation fails, the FRD is still updated for the successful ones.
