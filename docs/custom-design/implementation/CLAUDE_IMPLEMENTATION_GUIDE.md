# Claude Code Implementation Guide

You are implementing an approved PilotDeck customization. Do not redesign the product without explicit approval.

## Mandatory workflow

1. Read all relevant specifications in this package.
2. Inspect the current PilotDeck source and tests before proposing changes.
3. Identify existing abstractions and extension points that can be reused.
4. Produce a concise implementation plan containing:
   - current behavior
   - target behavior
   - affected modules
   - smallest viable design
   - compatibility risks
   - test plan
   - rollback plan
5. Stop and request approval before editing source files.
6. Implement one phase at a time with small, reviewable changes.
7. After each phase, run focused tests and report exact results.
8. Run the required governance reviews before declaring completion.
9. Do not commit or push unless explicitly instructed.

## Engineering requirements

- Apply `../prompts/global-engineering-policy.en.md` globally.
- Prefer extension of existing router/model/workspace/memory abstractions.
- Do not duplicate functionality already present in PilotDeck.
- Avoid adding dependencies unless existing facilities cannot reasonably solve the problem.
- Maintain backward compatibility for existing `pilotdeck.yaml` whenever feasible.
- Keep secrets out of source, logs, fixtures, and Git history.
- Guardians are read-only reviewers; implementation agents make revisions.
- Destructive or production-impacting operations require explicit human confirmation.

## Source areas to inspect first

The design review previously identified these upstream areas. Verify them against the checked-out version; paths may have changed.

- `src/model/config/parseModelConfig.ts`
  - per-model capability parsing
  - `maxContextTokens`, `maxOutputTokens`, `contextWindow`
- `src/router/tokenSaver/classifyAndRoute.ts`
  - last-user-message classification
  - retries, parsing and default-tier fallback
- `src/router/tokenSaver/generateJudgePrompt.ts`
  - current tier prompt generation
- `src/router/RouterRuntime.ts`
  - media modality detection and compatible fallback routing
  - decision lifecycle and session routing
- `src/router/config/schema.ts`
  - arbitrary tiers, subagent policy, orchestration config
- `src/router/orchestrate/`
  - orchestration prompt and tool restrictions
- `src/agent/loop/AgentLoop.ts`
  - effective context/output limits and compression integration
- UI settings and runtime event components
  - model/route visibility and fallback warnings

## Design-specific implementation direction

- Preserve the existing lightweight Judge behavior: current request only, not full history.
- Add domain + tier structured output with strict validation.
- Reuse the existing modality collection mechanism where possible, but replace direct multimodal fallback for this use case with a vision preprocessing stage that outputs structured text before DeepSeek execution.
- Implement model-level limit precedence as:

```text
model capability → agent fallback → system default
```

- Implement one orchestration layer only; workers must not recursively orchestrate.
- Keep raw Tool payloads in Archive and summaries in Memory.
- Add route/model metadata to UI and durable logs.

## Required approval gates

- Plan Review before source changes.
- Architecture Review for router, workspace, memory, or schema changes.
- Simplification Review before final tests.
- Final Engineering Audit before deployment recommendation.
