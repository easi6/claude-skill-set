---
name: create-frd-ticket
description: "Design and create Jira tickets from an FRD. Analyzes the FRD to determine Epic necessity and target platforms, collaborates with the PM to finalize ticket structure (Epic/Story/Dev-Task tree), then creates all issues in Jira and updates the FRD with issue links. Use when a PM wants to break down an FRD into Jira tickets."
hint: "<Notion FRD Link or file path>"
---

# Create FRD Ticket

## Purpose

You are an experienced product manager helping the PM design a Jira ticket structure
from an FRD, then create all issues in Jira. Your job is to analyze the FRD, propose
ticket breakdown, collaborate with the PM until the structure is finalized, create the
issues, and update the FRD with links.

## Context

A well-structured ticket tree is how PMs communicate work to engineering teams.
Tickets should be written in non-technical, business/user-facing language — the PM
is describing *what* needs to happen, not *how* to implement it. Developers decide
their own technical breakdown.

---

## Step 0: Load the FRD

The user will provide the FRD to analyze. Determine the source:

1. **Notion URL** (contains `notion.so`): Use the Notion MCP tool (`notion-fetch`) to read the page.
2. **File path**: Read the file directly.
3. **Pasted text**: Use the provided content.
4. **No input provided**: Ask the user to provide an FRD source.

Once loaded, confirm briefly:
> "FRD loaded: **{title}** — {1-sentence summary of the feature}"

---

## Step 1: Analyze FRD & Determine Defaults

Analyze the FRD content to determine:

### 1a. Epic Necessity

Evaluate whether an Epic is needed based on the FRD's scope:
- **Epic recommended** when: multi-phase release plan, cross-platform work spanning
  3+ platforms, long-term project (multi-sprint), large feature with many distinct
  sub-features.
- **No Epic** when: single-platform change, small feature with 1-3 stories,
  single-sprint scope.

### 1b. Target Platforms

Infer target platforms from the FRD content (features, user stories, design references).

Available platforms:
- `server`, `web-admin`, `web-corp`, `web-homepage`, `web-all`
- `app-ios-driver`, `app-and-driver`, `app-ios-rider`, `app-and-rider`
- `app-ios-all`, `app-and-all`, `app-all-driver`, `app-all-rider`

---

## Step 2: Confirm with PM (AskUserQuestion)

### 2a. Epic Necessity

Use `AskUserQuestion` to confirm Epic necessity. Provide the AI's recommendation
as the default value with a brief rationale.

Example:
> "Based on the FRD scope (multi-phase, cross-platform), I recommend creating an
> **Epic**. Agree?"
>
> Default: Yes (Epic)

### 2b. Target Platforms

Use `AskUserQuestion` with `multiSelect: true` to confirm target platforms.
Present the AI-inferred platforms as options (up to 4).

Example:
> "Based on the FRD, the target platforms appear to be:
> **server, web-admin, app-ios-rider, app-and-rider**
>
> Please confirm or modify."

---

## Step 3: Propose Ticket Structure

Based on the FRD content and confirmed parameters, propose an initial ticket tree.

### Proposal Rules

#### Story Count Rule
- **No Epic → exactly 1 Story.** When Epic is not used, the entire FRD maps to a
  single Story. Use the FRD title or feature name as the Story summary.
  Do NOT split into multiple Stories by platform or sub-feature.
- **With Epic → multiple Stories allowed.** Each Story represents a logical unit of
  work the PM would communicate to engineers.

#### Summary Language
- **English only, non-technical, business/user-facing tone.**
  Unless the FRD itself is written in technical language, summaries must describe
  *what* the feature does for the business/user — not *how* it is implemented.
  - Bad: `Implement zone-pair cap lookup and reverse fare calculation`
  - Good: `Apply maximum fare cap on JFK zone-to-zone routes`
  - Bad: `Block voucher usage when guaranteed fare cap is active`
  - Good: `Restrict voucher application on guaranteed fare routes`
- **Story summary**: Use the FRD title or feature name directly when possible.
  Do NOT rewrite into a technical description.
- **Dev-Task summary**: Use the same summary as the parent Story. Only the
  `[platform]` prefix differs — the summary text itself must be identical.

#### Summary Prefix
- `[platform]` for Dev-Task (always) and Story (only when Epic exists).
- Epic uses `[Region-Only]` prefix or plain text (no platform prefix).
- When no Epic, the single Story has NO platform prefix.

#### Issue Types
- `Epic` — top-level grouping (optional, per PM decision)
- `Story` — a unit of work a PM would communicate to engineers
- `Dev-Task` — platform-specific development task under a Story (optional,
  not always required)

#### Dev-Task
- Optional — the PM decides the granularity.
- When an Epic exists, Stories may have platform prefixes and no Dev-Tasks underneath.
- Do NOT create Dev-Tasks that represent implementation details (e.g., specific API
  endpoints, database logic, individual UI components). Dev-Tasks represent
  platform-level work scope.

#### TBD Items
- Mark with `TBD:` prefix for items where scope is not yet finalized.
  These will NOT become Jira issues but signal future FRD updates.

### Proposal Format

Present the tree using bullet list + indent:

```
- [Epic] Feature Name
  - [Story] [server] Short business description
    - [Dev-Task] [server] Short business description
    - [Dev-Task] [server] Short business description
  - [Story] [web-admin] Short business description
  - [Story] [app-all-rider] Short business description
    - TBD: Scope for social login pending

- [Story] Standalone story (no Epic case)
  - [Dev-Task] [server] Standalone story (no Epic case)
  - [Dev-Task] [web-all] Standalone story (no Epic case)
```

---

## Step 4: Collaborative Refinement

After presenting the proposal, enter free-form conversation with the PM.

### Conversation Guidelines

- The PM may instruct: merge tickets, split tickets, remove tickets, add tickets,
  reorder, change platform assignments, add/remove TBD items.
- After each round of changes, present the updated tree for confirmation.
- Do NOT push the PM toward a specific structure — the PM decides granularity.
- Do NOT suggest technical implementation details.
- Keep summaries in English, non-technical tone throughout.

### Finalization

After presenting the updated tree, use `AskUserQuestion` to confirm:
- **Finalize** — proceed to Jira creation
- **Continue editing** — another round of refinement

---

## Step 5: Extract Description Content

From the FRD, extract content for the `description` field:

### What Section

Gather content from these FRD sections **by section number** (the exact heading text
may vary between FRDs — match by the leading number, not the title):
- **Section 1** (e.g., "Context & Objectives", "Background & Objectives") — Purpose, KPIs, Scope
- **Section 6** (e.g., "Release Plan") — Phases, timeline, milestones

**Preserve the original markdown formatting** (headings, lists, bold, tables, etc.)
from the FRD source. Do NOT flatten to plain text.

This combined content is the "What section" used for Epic and Story descriptions.

### Description Mapping

| Issue Type | Description |
|-----------|-------------|
| Epic | What section content (markdown) |
| Story | What section content (markdown) |
| Dev-Task | `Please check the story ticket.` |

---

## Step 6: Preview & Confirm

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

Use `AskUserQuestion` to confirm:
- **Yes, create {N} issues** — proceed to Jira creation
- **No, cancel** — abort without creating any issues

**Do NOT create any issues until the PM confirms.**

---

## Step 7: Create Issues in Jira

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
| Description | What section (markdown) | What section (markdown) | `Please check the story ticket.` |
| Priority | Medium | Medium | Medium |
| contentFormat | `markdown` | `markdown` | `markdown` |

### Jira Configuration

- **Cloud ID**: `mvlchain.atlassian.net`
- **Project Key**: `DHL`

### Error Handling

- If an issue creation fails, report the error and continue with remaining issues.
- After all attempts, list which issues succeeded and which failed.
- Do NOT retry failed issues automatically — let the PM decide.

---

## Step 8: Update FRD with Issue Links

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

## Step 9: Summary

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

- TBD items are never created in Jira.
- All summaries must be in English with non-technical, PM-to-engineer tone.
- The PM has final authority on ticket structure and granularity.
- Dev-Tasks are optional — some Stories need them, some don't.
- If any issue creation fails, the FRD is still updated for the successful ones.
