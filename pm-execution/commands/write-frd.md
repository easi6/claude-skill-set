---
description: Create a comprehensive Functional Requirement Document from a feature idea or problem statement, using an interview-driven workflow that extracts PM intent before drafting
argument-hint: "<feature or problem statement>"
---

# /write-frd — Functional Requirement Document

Create a structured FRD that aligns Engineering, Design, QA, and Leadership and
guides development. Accepts anything from a vague idea to a detailed brief.

This command runs the **create-frd** skill, which interviews the PM first
(extracting intent through Socratic questions with advisory input), then drafts
the FRD against the mvlchain 9-section template.

## Invocation

```
/write-frd SSO support for enterprise customers
/write-frd Users are dropping off during onboarding — we need to fix step 3
/write-frd [upload a brief, research doc, or strategy deck]
```

## Workflow

### Step 1: Understand the Feature

Accept the input in any form:
- A feature name ("SSO support")
- A problem statement ("Enterprise customers keep asking for centralized auth")
- A user request ("Users want to export their data as CSV")
- A vague idea ("We should do something about onboarding drop-off")
- An uploaded document (brief, research, Slack thread, email)
- A Notion or Jira link

### Step 2: Run the Interview

Invoke the **create-frd** skill to run a structured discovery interview.
The interview is mapped to the nine FRD sections so every answer has a home:

- §1 Context & Objectives — *Why now? KPIs (SMART). Scope In/Out.*
- §2 Features — *Happy path, edge cases, server-side data requests.*
- §3 Design & UX — *Figma status, UX intent, error/empty states.*
- §4 Non-Functional Requirements — *Performance, security, availability.*
- §5 User Stories — *Primary and secondary actors.*
- §6 Release Plan — *MVP boundary, phasing.*
- §7 Test Scenarios — *Worst acceptable failure = P0.*
- §8 RACI — *Accountable person per deliverable.*

Interview rules the skill follows:
- 2–3 questions per turn (never a question dump)
- Every question paired with an **advisory input** the PM can react to
- One layer of 5-Whys on thin answers — not more
- Surface assumptions silently, consolidate at the end

### Step 3: Draft Against the mvlchain FRD Template

The skill fetches the FRD template from Notion (via `notion-fetch`) and drafts
against the exact structure:

```
0. Change Log            — date / author / change / reason / reference
1. Context & Objectives  — Purpose, KPIs (Current/Target), Scope In/Out
2. Features              — 2.1, 2.2, … per module + 2.+ Data Related Requests
3. Design & UX           — Figma link placeholder, UX intent, design principles
4. Non-Functional        — Performance, Scalability, Security, Availability
5. User Stories          — "As a…" + acceptance criteria
6. Release Plan          — Phase / Business / Technical / Timeline
7. Test Scenarios (QA)   — TC-ID, Type, Priority, Given/When/Then
8. RACI Matrix           — PM, PO, Product Ops, UX, Tech Lead, QA, …
```

Gaps from the interview become `{TBD — Open Question Q#N}` markers plus a
consolidated **Open Questions** block at the top. Unvalidated assumptions go in
an **Assumptions** block flagged `⚠ Unvalidated`.

### Step 4: Gap Review and Handoff

After drafting, the skill presents Open Questions and Assumptions for the PM to
resolve, then offers:

- "Want me to get a **multi-role review** from Developer, QA, and Designer? (`/review-frd`)"

Save the FRD as `FRD-[feature-slug].md`.

## Notes

- Be opinionated about scope — a tight FRD is better than an expansive vague one.
- Non-Goals (Scope Out) are as important as Goals — they prevent scope creep.
- KPIs must be specific: "improve NPS" is bad, "increase NPS from 32 to 45 within
  90 days of launch" is good.
- Open Questions should be genuinely unresolved — don't list things the PM can
  answer from context.
- If the user provides research, weave insights into §1 Context with attribution.
- If Figma isn't ready, leave the §3 Figma placeholder untouched — never invent
  visuals.
