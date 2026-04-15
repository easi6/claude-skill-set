# pm-execution

FRD authoring and review skills for the mvlchain PM workflow.

## Skills (2)

- **create-frd** — Interview-driven Functional Requirement Document authoring against the mvlchain 9-section FRD template (Change Log, Context & Objectives, Features, Design & UX, NFR, User Stories, Release Plan, Test Scenarios, RACI). Phase 1 runs a Socratic discovery interview with advisory input; Phase 2 drafts against the live Notion template; Phase 3 surfaces gaps and assumptions.
- **review-frd** — Multi-role review of an FRD from Developer, QA, and Designer perspectives. Compact, actionable questions the PM can answer — no implementation-detail asks.

## Commands (2)

- `/pm-execution:write-frd` — Run the `create-frd` skill from a feature idea, problem statement, Notion/Jira link, or document.
- `/pm-execution:review-frd` — Run the `review-frd` skill on an existing FRD (Notion URL, Jira key, file, or text).

## Workflow

```
[Biz/Ops request] → /write-frd → [draft FRD]
                       ↓
                   /review-frd → [Dev/QA/Designer questions]
```

## Author

Maintained by mvlchain (dev@mvlchain.io).

## License

MIT
