---
description: Review an FRD from Developer, QA, and Designer perspectives — surfaces only questions the PM can answer
argument-hint: "<Notion FRD Link or Jira Ticket Link or Text>"
---

# /review-frd — Multi-Role FRD Review

Run the **review-frd** skill on an FRD and return Developer, QA, and Designer
questions the PM should resolve before development starts. Questions are
compact, specific, and only things the PM can answer with product knowledge —
no implementation detail asks.

## Invocation

```
/review-frd https://www.notion.so/mvlchain/...          # Notion FRD URL
/review-frd PROJ-123                                    # Jira ticket
/review-frd [paste FRD text or upload a markdown file]
```

## Workflow

The skill runs in five steps:

1. **Load the FRD template** (Notion MCP `notion-fetch`) — used as a
   completeness baseline against mvlchain's 9-section structure.
2. **Load the FRD**:
   - Notion URL → `notion-fetch`
   - Jira key (e.g., `PROJ-123`) → `getJiraIssue`
   - File path or pasted text → read directly
3. **Select roles** — Developer / QA / Designer (all three by default).
4. **Parallel review** — each role applies its agent prompt from
   `resources/{developer,qa,designer}-agent.md` to surface 3-8 questions,
   grouped by FRD section.
5. **Consolidated summary** — template completeness check + cross-role
   issues + deduplicated prioritized question list (typically 5-15 items).

## Output

Each role produces a compact review:

```markdown
# {Role} Review

{1-2 sentence overall assessment}

**{N}. {Topic}**
1. {Specific question the PM can answer}
2. {Specific question the PM can answer}
```

Followed by a consolidated summary:

```markdown
# FRD Review Summary: {Title}

## Template Completeness
{Missing or incomplete sections from the mvlchain FRD template}

## Cross-Role Issues
{Issues flagged by 2+ roles, with different perspectives noted}

## Questions for PM
1. {Most impactful question} — from {role(s)}
2. {Second most impactful} — from {role(s)}
```

## Review Rules (inherited from the skill)

- Only questions the PM can answer — no API design, data models, test
  frameworks, or visual design decisions.
- Only high-impact questions — if the PM ignores it and nothing breaks, skip.
- Specific — reference exact section, feature, or threshold.
- Compact — 3-8 questions per role, never padded.

## Notes

- Pair with `/write-frd` — draft first, then review before handoff.
- If the FRD is missing major sections from the template, the summary surfaces
  them in **Template Completeness** before per-role questions.
