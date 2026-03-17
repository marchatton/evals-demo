# Build prompt for Codex

Build a small local demo app called **ReturnFlow** using the files in this bundle.

## Goal

Create a toy **returns and exchanges operations desk** that is good enough for a demo-first podcast episode about **product-level LLM evals**.

The app should let a support rep or ops user:
- ask free-text questions about return or exchange eligibility
- inspect grounded order state
- run one of two lightweight tools:
  - `lookup_return_eligibility`
  - `mutate_return_case`
- review traces in a simple UI
- label traces
- run a first evaluator against those traces
- include intentionally bad eval artefacts for an `eval-audit`

## Audience and demo shape

The end demo is for a broad builder audience. Designers, engineers, PMs, marketers, generalists. The app should be understandable on a screen share in under 90 seconds.

So:
- keep the UI compact
- make state visible
- do not hide the blocker codes or reasons
- optimise for reviewability, not product polish

## Build constraints

- local app only
- static JSON / CSV backing data
- no external APIs
- no auth
- no real payments
- no real shipping provider integration
- no emails
- no background jobs
- no server complexity unless you need a tiny local API

You can choose the stack, but bias toward the fastest path to a stable local demo repo.

## Files to use first

Read these first:
1. `README_FIRST.md`
2. `golden_examples.jsonl`
3. `build_state.json`
4. `returns_pack/tool_specs.json`
5. `returns_pack/toy_app_spec.json`
6. `returns_pack/rules.csv`
7. `returns_pack/trace_schema.json`
8. `returns_pack/evaluator_candidate.json`
9. `returns_pack/roastable_artefacts.json`

## What to build

### 1. Core state model

Load the seed data from `build_state.json` or the split files in `state_seed/`.

At minimum support:
- stores
- orders
- line items
- immutable return-rule snapshots
- return requests
- return cases
- exchange items
- inventory records
- staff permissions

### 2. Tool 1: `lookup_return_eligibility`

Implement a deterministic function that:
- takes actor mode, actor id, order id, requested action, line item requests, and timestamp
- returns whether the action is allowed
- returns normalized blocker codes and controlling rule ids
- returns a compact fees preview when relevant

It should be source-faithful to the rules in `returns_pack/rules.csv`.

### 3. Tool 2: `mutate_return_case`

Implement a deterministic mutation function for:
- `approve_request`
- `decline_request`
- `create_return`
- `process_return`
- `cancel_return`
- `open_return`
- `close_return`

You do not need every edge case in Shopify.
You do need the state transitions and blockers used by the golden examples.

### 4. Golden examples -> traces

Turn the 16 records in `golden_examples.jsonl` into seeded traces.

Each trace should include:
- user message
- relevant state snapshot
- tool call
- tool result
- final assistant output
- metadata
- reviewer labels
- evaluator output placeholder

Use the practical field spec in `returns_pack/trace_schema.json`.

### 5. Review UI

Build a very simple review UI that shows:
- the user message
- the key order/store state
- the tool call and tool result
- the final assistant response
- reviewer controls for pass/fail + failure type + notes

Keep this ugly if needed. Clarity beats polish.

### 6. Eval artefacts

Include the deliberately bad eval artefacts from `returns_pack/roastable_artefacts.json`:
- bad headline metric
- bad judge prompt
- weak labeling setup
- missing analysis step
- misleading aggregation choice

These should be visible enough that a host can roast them on screen.

### 7. First evaluator

Implement the first evaluator target from `returns_pack/evaluator_candidate.json`:

- **F01 false return-action eligibility claim**

The evaluator should judge whether the assistant correctly stated if the requested action was actually allowed under the provided state and rules.

Make it binary.
Do not try to make it smart about everything.

### 8. Preload the best traces

Use `live_review_shortlist.json` to preload the best 6–8 traces into the review UI.

## Important product constraints to preserve

Do not simplify away these distinctions:
- current store rule template vs saved order snapshot
- self-serve vs staff admin path
- return request vs return case
- open vs canceled return states
- duties / label / refunded-item blockers
- customer accounts mode

## What good looks like

A viewer should be able to watch the demo and immediately grasp:
- why the app is stateful
- why the assistant can be politely wrong
- why the first evaluator is a crisp product-level check
- why the bad eval setup deserves to be roasted

## Nice-to-have, not required

- a toggle between “good answer” and “bad answer” for each golden example
- a small confusion matrix for the first evaluator
- a one-click way to seed the demo data
- a simple filter for failure type or rule family

## Deliverable style

Build for credibility, not polish.
Small repo.
Clear files.
Source-faithful behavior.
Visibly roastable eval setup.
