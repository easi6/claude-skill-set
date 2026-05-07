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

## Step 4: Collect Role Results (Internal)

Each role agent returns its review result to the main agent. These are **internal
intermediate results — do NOT show them to the PM directly.**

Each role produces 3-8 questions in a numbered list grouped by topic. The main agent
collects all role results and proceeds to Step 5.

---

## Step 5: Deduplicate & Present

After collecting all role results, **deduplicate, merge, and present a single
consolidated review** to the PM. This is the **only output the PM sees**.

### Deduplication Rules

1. **Same root cause = merge.** If two or more roles flagged questions that trace back
   to the same FRD gap (e.g., Developer asks "what happens when X?" and QA asks
   "how do I test the X case?"), merge into a single item.
2. **Pick the most specific form.** When merging, keep the question that references
   a concrete section, threshold, or scenario — discard the vaguer version.
3. **Tag all contributing roles.** After merging, note which roles raised it
   (e.g., `— Developer, QA`).
4. **Unique perspectives stay separate.** Only merge when the underlying FRD gap is
   the same. Two questions about the same feature but asking about genuinely different
   gaps (e.g., missing business rule vs. missing user flow) remain separate items.

### Output Format

```markdown
# FRD Review: {Title}

## Template Completeness
{List any major sections from the FRD template that are missing or incomplete}

## Questions for PM
1. {Most impactful question} — {role(s)}
2. {Second most impactful} — {role(s)}
...
```

Total consolidated questions should typically be 5-15. Every question should make
the PM think "oh, I need to clarify that" — not "that's obvious" and not
"I can't answer that."
