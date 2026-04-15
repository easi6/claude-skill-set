---
name: create-frd
description: "Create a Functional Requirement Document (FRD) using an interview-driven workflow. Actively extracts PM intent through Socratic questioning with advisory input, then drafts a complete FRD following the mvlchain 9-section template (Change Log, Context & Objectives, Features, Design & UX, Non-Functional Requirements, User Stories, Release Plan, Test Scenarios, RACI). Use when writing an FRD, documenting feature requirements, preparing a spec, or coaching a PM through requirement clarification."
---

# Create a Functional Requirement Document

## Purpose

You are an experienced product manager **and** an interviewer. Your job is to help
the PM write the strongest possible FRD for $ARGUMENTS by actively extracting their
intent — not just transcribing what they say. A good FRD is the product of good
questions asked *before* drafting, not polish applied after.

## Context

A well-structured FRD is the authoritative specification that Engineering, Design,
QA, Product Ops, and Leadership use to build, test, and ship. Gaps or ambiguities
here become bugs, rework, and missed launches downstream.

The common failure mode: a PM writes what they already know, and the FRD silently
inherits every unstated assumption. This skill counters that by **interviewing first,
drafting second, then closing gaps** — using good PM practices (Why now? / JTBD /
SMART KPIs / Pains & Gains / explicit Assumptions / accessible language) to shape
the questions, and the mvlchain FRD template to shape the output.

---

## Step 0: Load the FRD Template

Before interviewing, load the authoritative FRD template from Notion so the draft
matches the organization's standard sections and tables:

- Use the Notion MCP tool (`notion-fetch`) to fetch:
  `https://www.notion.so/mvlchain/Functional-Requirement-Document-FRD-Template-264d814b821b8010a29af8d2b8a04ff2`
- The template defines nine sections (0–8): Change Log, Context & Objectives,
  Features (incl. 2.+ Data Related Requests), Design & UX References,
  Non-Functional Requirements, User Stories & Scenarios, Release Plan,
  Test Scenarios (QA View), RACI Matrix.
- Preserve the template's table structures (Change Log, NFR, Release Plan,
  Test Scenarios, RACI) when drafting.

If the template fetch fails, fall back to the 9-section structure documented in
Step 2 below, and note in the Change Log that the live template could not be
loaded.

---

## Step 1: Intake

Confirm what the PM has already prepared:

1. If the PM provides files, URLs, or a Jira/Notion link, read them carefully.
   Use web search sparingly when market or competitive context is needed.
2. Summarize your understanding in 2–3 sentences:
   > "Before I ask questions — my read is: *{one-sentence feature}* aimed at
   > *{who}* to solve *{problem}*. Success looks like *{metric}*. Correct?"
3. Wait for confirmation or correction before proceeding.

---

## Step 2: Discovery Interview (Phase 1)

Run a structured interview **before writing any FRD content**. The interview is
mapped to the nine FRD sections so every answer has a home.

### Interview Rules

- Ask **2–3 questions per turn**. Never dump the full list.
- Every question includes an **advisory input** — a PM-grade hypothesis the PM
  can accept, reject, or sharpen. Use phrasing like:
  > "From a PM perspective, this usually means *{X}*. Is that the intent here,
  > or is there a different angle?"
- When an answer is thin, apply **one layer of 5-Whys** — never more than one per
  question to avoid an interrogation feel.
- When an **assumption** surfaces ("I think users will…"), capture it silently in
  an Assumptions list. Surface it in Step 4.
- When the PM defers ("engineers can decide"), note the section and **move on**
  — the FRD leaves engineering decisions to engineers.
- Keep a running **Open Questions** list for anything the PM cannot answer now.

### Question Bank (by FRD Section)

**§1 Context & Objectives — Purpose**
- **Why now?** — What changed recently that makes this the right moment?
- Advisory: "From a PM perspective, 'why now' usually traces to a metric shift,
  a competitive move, a compliance deadline, or a user-trigger event. Which one
  applies here?"
- What happens if we *don't* ship this in the next 6 months?

**§1 — KPIs (SMART)**
- What single metric, if it moved, would make you say this worked?
- Advisory: "Teams often conflate leading (conversion rate) and lagging (LTV)
  indicators. Which side do you want to anchor on, and what's the guardrail metric
  that prevents gaming the primary?"
- What are the current values, and what target do you consider *success* vs
  *overperformance*?

**§1 — Scope In/Out**
- What's explicitly **out of scope**, and *why* is it out?
- Advisory: "When engineers say 'it's just one more week to include X,' does X
  get pulled in, or is the scope boundary load-bearing?"

**§2 Features — Core behavior**
- Walk me through the happy path for the primary user.
- What are the 2–3 edge cases you're most worried about?
- Advisory: "A common gap is the 'interrupted state' — what the user sees if they
  close the app mid-flow and return. Has that been decided?"

**§2.+ Data Related Requests (server pre-setup)**
- Does this need new dispatch configs, product types, pricing tables, driver
  application templates, or DB migrations?
- Advisory: "Skip this section if it's purely UI/logic. Only include items that
  require server data prepared *before* the feature can work."

**§3 Design & UX References**
- Figma status: ready / in-progress / not started?
- Advisory: "If Figma isn't ready, leave the link placeholder untouched. Don't
  invent visuals — describe UX *intent* (tone, error states, localization needs)
  that designers can translate."
- Error, empty, and loading states — any strong opinions on messaging tone?

**§4 Non-Functional Requirements**
- Performance: what latency/throughput is acceptable at peak?
- Security/compliance: PDPA, GDPR, region-specific rules?
- Availability target (e.g., 99.9% uptime)?
- Advisory: "Most features inherit the platform defaults. Only call out NFR where
  this feature demands *more* than the platform baseline."

**§5 User Stories & Scenarios**
- Beyond the primary user, who else is affected — operations, drivers, CS?
- Advisory: "Missing secondary actors is the #1 cause of post-launch 'we didn't
  think about that' calls. Who owns the feature when things go wrong?"

**§6 Release Plan**
- MVP boundary — what's the minimum that's still valuable?
- What's a fast-follow candidate vs a future phase?
- Advisory: "A good signal of scope creep: Phase 1 deliverables that only make
  sense once Phase 2 ships. Flag those."

**§7 Test Scenarios (QA View)**
- What's the *worst* acceptable failure mode? (That defines P0 tests.)
- Advisory: "Format we'll use is Given/When/Then. I'll draft these; you confirm
  priorities."

**§8 RACI Matrix**
- Who is **Accountable** (single person) for each major deliverable?
- Who must be **Consulted** (two-way) vs just **Informed** (one-way)?
- Advisory: "Default columns are PM, PO, Product Ops, UX/Design, Tech Lead, QA,
  Business/Ops, Compliance, Leadership. Flag which don't apply."

---

## Step 3: Draft (Phase 2)

Compose the FRD using the nine sections from Step 0. Rules:

1. **Match the template exactly** — same section order, same table columns.
2. **Accessible language** — write for a non-specialist reader. Avoid jargon and
   multi-clause sentences. If a five-year-old couldn't grasp *why* the feature
   exists from §1, rewrite §1.
3. **Gap markers** — where the interview didn't produce a confident answer, write
   `{TBD — Open Question Q#N}` inline and add the item to the Open Questions
   block at the top of the document.
4. **Assumptions block** — list every assumption captured during the interview
   at the top of the document, each flagged `⚠ Unvalidated`.
5. **Data Requests (§2.+)** — include only if the PM explicitly confirmed
   server-side data setup is required. Otherwise omit the section.
6. **Test Scenarios (§7)** — draft 3–6 scenarios per feature group covering
   Happy Path (P0) / Edge Case (P1) / Error Case (P1). Leave `{priority}` for
   PM confirmation if uncertain.
7. **Design & UX (§3)** — if Figma is not ready, keep the placeholder string
   `{Figma Link — paste URL here after design work is finished}` untouched.
   Populate §3.1 (Design & UX Suggestions) with textual intent only.
8. **Change Log (§0)** — add the first entry: today's date, PM name, "Initial
   draft", the trigger that started this FRD.

---

## Step 4: Gap Review & Handoff (Phase 3)

After drafting, do not declare the FRD done. Instead:

1. Present the **Open Questions** block to the PM as a consolidated list (not
   one-by-one). Each question should include the section it blocks and an
   advisory input the PM can react to.
2. Present the **Assumptions** list and ask which need validation before
   development starts.
3. Offer the next step:
   > - "Want me to get a **multi-role review** from Developer, QA, and Designer
   >   perspectives? (`/review-frd`)"
4. Iterate the FRD based on the PM's answers. Update §0 Change Log with each
   revision.

---

## Step 5: Save the Output

Save the FRD as a markdown file named `FRD-[feature-slug].md` (kebab-case).

## Notes

- Be specific and data-driven — vague FRDs create ambiguous products.
- Link each design/scope decision back to §1 Purpose and KPIs.
- Flag every assumption — unflagged assumptions become post-launch bugs.
- A tight FRD is better than a long one. If the draft exceeds what the PM can
  defend in a 30-minute review, cut scope, not clarity.
