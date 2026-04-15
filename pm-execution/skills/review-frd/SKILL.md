---
name: review-frd
description: Review FRD documents from Developer, QA, and Designer perspectives.
hint: "<Notion FRD Link or Jira Ticket Link or Text>"
---

# FRD Multi-Role Reviewer

Help the PM strengthen their FRD by surfacing the essential questions that Developer,
QA, and Designer would ask in a review meeting.

## Core Premise

FRDs are written by **non-developer PMs**. The PM defines *what* and *why*. Engineers
decide *how*. Every question in this review must be something the PM can answer with
product knowledge — never ask about implementation details (API schemas, database
design, algorithms, infrastructure, test frameworks, UI component choices).

The review should be **compact and actionable**. Only surface questions where, if the
PM doesn't answer them, something will actually go wrong during development. Skip
anything teams can figure out on their own.

## Step 0: Load the FRD Template

Before reviewing, load the standard FRD template from Notion to use as a reference
for completeness checks:

- Use the Notion MCP tool (`notion-fetch`) to fetch the FRD template page:
  `https://www.notion.so/mvlchain/Functional-Requirement-Document-FRD-Template-264d814b821b8010a29af8d2b8a04ff2`
- This template defines the expected sections (Change Log, Context & Objectives,
  Features, Design & UX, Non-Functional Requirements, User Stories, Release Plan,
  Test Scenarios, RACI Matrix).
- Use this template as a baseline when evaluating the FRD's completeness in each
  role's review.

## Step 1: Load the FRD

The user will provide the FRD to review. Determine the source:

1. **Notion URL** (contains `notion.so`): Use the Notion MCP tool (`notion-fetch`) to read the page.
2. **Jira key** (like `PROJ-123`): Use the Atlassian MCP tool (`getJiraIssue`) to fetch the issue.
3. **File path**: Read the file directly.
4. **No input provided**: Ask the user to provide an FRD source (Notion URL, Jira key, or file path).

Once loaded, confirm briefly:
> "FRD loaded: **{title}** ({word count} words)"

---

## Step 2: Role Selection

Ask the user which roles should review the FRD (multi-select, all selected by default):

- **Developer** — Missing business rules, undefined behaviors, ambiguous conditions
- **QA** — Untestable criteria, missing scenarios, ambiguous pass/fail
- **Designer** — Undefined user states, missing UX decisions, copy/messaging gaps

---

## Step 3: Review (Parallel)

Dispatch each selected role as a parallel review. Each review receives:
1. The full FRD content
2. The FRD template (loaded in Step 0)
3. The role-specific agent prompt from `resources/`

Use the following agent prompt files for each role:

- **Developer**: `resources/developer-agent.md`
- **QA**: `resources/qa-agent.md`
- **Designer**: `resources/designer-agent.md`

Read the corresponding agent file and use it as the system prompt for each
parallel review. Pass the FRD content and template as context.

---

## Step 4: Output Format

Each role produces a compact review. No severity labels, no tables — just a clean
numbered list of questions grouped by topic.

```markdown
# {Role} Review

{1-2 sentence overall assessment}

**§{N} {Topic}**
1. {Specific question the PM can answer}
2. {Specific question the PM can answer}

**§{N} {Topic}**
3. {Specific question the PM can answer}
```

Keep it short. 3-8 questions per role. Every question should make the PM think
"oh, I need to clarify that" — not "that's obvious" and not "I can't answer that."

---

## Step 5: Consolidated Summary

After all role reviews, produce one summary:

```markdown
# FRD Review Summary: {Title}

## Template Completeness
{List any major sections from the FRD template that are missing or incomplete}

## Cross-Role Issues
{Issues flagged by 2+ roles, with the different perspectives noted}

## Questions for PM
1. {Most impactful question} — from {role(s)}
2. {Second most impactful} — from {role(s)}
...
```

The consolidated question list should be **deduplicated and prioritized**. If Developer
and QA both flagged the same overlapping threshold, merge into one item. Total
consolidated questions should typically be 5-15.
