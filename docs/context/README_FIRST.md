# ReturnFlow codex-ready bundle

Start here.

This bundle is the upgraded build handoff for the **Returns & Exchanges Desk** toy app.

The earlier research pack already had:
- the grounded source map
- the rulebook
- the domain model
- seed examples
- validation slice
- evaluator target
- roastable eval artefacts

What this bundle adds is the missing bit that makes the build easier:
- **16 golden examples** with actual state, expected tool calls, expected tool results, and good/bad assistant outputs
- a **combined build state** file plus split state tables
- a **Codex build prompt**
- a **shortlist of the best traces to preload into the review UI**

## The files that matter most

1. `CODEX_BUILD_PROMPT.md`
   - paste this into Codex with the rest of the bundle attached

2. `golden_examples.jsonl`
   - these are the canonical examples
   - each one has a user message, state refs, expected tool call, expected tool result, and good/bad answer pair

3. `build_state.json`
   - this is the seed state for the app
   - same data is also split in `state_seed/`

4. `returns_pack/`
   - the original research pack and machine-readable specs
   - especially `toy_app_spec.json`, `tool_specs.json`, `rules.csv`, `trace_schema.json`, `evaluator_candidate.json`, and `roastable_artefacts.json`

## Suggested build order

1. Load `build_state.json`
2. Implement `lookup_return_eligibility`
3. Implement `mutate_return_case`
4. Turn the 16 golden examples into seeded traces
5. Build a simple review UI
6. Preload the 8 traces in `live_review_shortlist.json`
7. Add the intentionally bad eval artefacts from `returns_pack/roastable_artefacts.json`

## Non-goals

Do **not** build:
- real Shopify integrations
- real auth
- real label purchase
- real payment processing
- real customer-account flows
- a universal legal returns engine

## Keep these parts source-faithful

- order-level rule snapshots
- delivery-date fallback to fulfillment date + transit buffer
- self-serve vs admin asymmetry
- self-serve exchanges unsupported
- 250-line-item cap in self-serve
- fulfilled/unrefunded requirement for admin return creation
- US-only Shopify-admin return-label path
- partial processing
- cancel/open/close/canceled state machine

## Quick reality check

This should feel like a small, local, believable product. Not a Shopify clone.

Call it **ReturnFlow** or something similarly generic.
