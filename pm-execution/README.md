# pm-execution

FRD lifecycle toolkit for the mvlchain PM workflow.

## Skills (4)

- **create-frd** — Interview-driven Functional Requirement Document authoring against the mvlchain 9-section FRD template (Change Log, Context & Objectives, Features, Design & UX, NFR, User Stories, Release Plan, Test Scenarios, RACI). Phase 1 runs a Socratic discovery interview with advisory input; Phase 2 drafts against the live Notion template; Phase 3 surfaces gaps and assumptions.
- **review-frd** — Multi-role review of an FRD from Developer, QA, and Designer perspectives. Compact, actionable questions the PM can answer — no implementation-detail asks.
- **design-frd-ticket** — Design Jira ticket structure (Epic/Story/Dev-Task) from an FRD. Analyzes scope, confirms Epic necessity and target platforms, collaborates with PM on breakdown, appends finalized tree to FRD section 9 (Tasks).
- **create-frd-ticket** — Create Jira issues from an FRD's ticket structure in section 9. Previews issues before creation, creates in DHL project with proper linking, updates FRD with Jira issue links.

## Commands (4)

- `/pm-execution:write-frd` — Run the `create-frd` skill from a feature idea, problem statement, Notion/Jira link, or document.
- `/pm-execution:review-frd` — Run the `review-frd` skill on an existing FRD (Notion URL, Jira key, file, or text).
- `/pm-execution:design-frd-ticket` — Run the `design-frd-ticket` skill to design Jira ticket structure from an FRD.
- `/pm-execution:create-frd-ticket` — Run the `create-frd-ticket` skill to create Jira issues from designed tickets.

## Workflow

```
[Biz/Ops request] → /write-frd → [draft FRD]
                       ↓
                   /review-frd → [Dev/QA/Designer questions]
                       ↓
               /design-frd-ticket → [ticket tree in FRD Section 9]
                       ↓
               /create-frd-ticket → [Jira issues created]
```

## Author

Maintained by mvlchain (dev@mvlchain.io).

## License

MIT
