# Upstream Source Notes

These notes capture source behavior verified during the design discussion. Claude Code must re-check the currently checked-out upstream revision before relying on them.

## Model capabilities

`src/model/config/parseModelConfig.ts`

- Model definitions accept `capabilities`.
- `maxContextTokens` and `maxOutputTokens` are parsed per model.
- `contextWindow` is accepted as an alias for `maxContextTokens`.
- Multimodal input capabilities are parsed per model.

## Router tiers and orchestration

`src/router/config/schema.ts` and `src/router/config/parseRouterConfig.ts`

- Tier names are arbitrary record keys rather than a fixed enum.
- TokenSaver subagent policy supports `judge` and `skip`.
- Auto-orchestration triggers are configured by tier name.
- Legacy `mainAgentModel` and `subagentModel` fields are deprecated and ignored in the inspected revision.

## Judge behavior

`src/router/tokenSaver/classifyAndRoute.ts`

- Classification extracts the last user message rather than forwarding the full conversation.
- It sends a small, non-thinking, temperature-zero judge request.
- It retries parse/model failures and falls back to `defaultTier`.
- Unknown or unparseable tier responses do not become arbitrary routes.
- Short continuation messages have special handling to preserve the previous tier.

## Media behavior

`src/router/RouterRuntime.ts`

- The runtime collects required input modalities from messages.
- It checks whether the selected model supports them.
- It can reroute to a compatible fallback model.

The custom design should reuse this modality detection but insert a Qwen vision preprocessing stage rather than switching the entire ongoing task to a vision model.
