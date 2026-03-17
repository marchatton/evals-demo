# ReturnFlow research prompt: original + changes + consolidated current version

## 1) Original research prompt from this chat

```text
You are doing a **grounding research pass** for a possible demo-first podcast episode about **product-level LLM evals**.

This is **not** a coding task.
Do **not** build the app.
Do **not** invent policy details.
Your output will be **fed into a later coding agent**. That later agent should be able to build a toy app, dataset, traces, and roastable eval artefacts from your output **without needing to do its own research**.

## Core goal

Choose one public domain and produce a **self-contained research pack** that is credible enough for a sceptical expert and structured enough for a later agent to build from directly.

The research pack must support a possible demo arc like this:

1. `eval-audit`
2. `error-analysis`
3. `write-judge-prompt`
4. `validate-evaluator`

Important:
- This arc is **proposed, not approved**
- Your job is to support it **or** recommend a better arc if the domain does not fit it well

## What success looks like

Your output should make it possible for a later coding agent to create:
- a small example app
- supporting docs / tables / JSON state
- a believable seed dataset
- realistic traces
- one strong candidate evaluator
- a small labelled validation slice
- intentionally bad eval artefacts that feel realistic enough for a roast report

The later agent should not need to guess:
- what the domain rules are
- what the user tasks are
- what stateful objects exist
- what tools should exist
- what failure modes matter
- what examples to include
- what trace schema to use
- what bad artefacts to plant for the audit

## Constraints

Bias towards domains that are:
- public and sourceable
- narrow enough to explain in under 90 seconds
- rich in rules, thresholds, and exceptions
- realistic at product level
- easy to simulate with 1–2 lightweight tools or lookups
- easy to turn into 6–8 visually distinct traces for live labelling

Avoid:
- medical
- legal
- financial advice needing live market data
- highly regulated workflows
- domains where most judgement is subjective
- anything that depends heavily on live APIs or private data

## Hard gates

Only recommend a domain if **all** of these are true:

1. Public primary sources are available without login
2. Rules / thresholds / exceptions are clear enough to normalise
3. At least **15 seed examples** can be grounded directly in public material
4. At least **1 lightweight stateful lookup or action** can be simulated
5. At least **1 crisp binary failure mode** can be labelled by humans and validated
6. A broad builder audience could understand the domain quickly
7. The domain can generate **6–8 traces worth labelling live**
8. The demo repo can contain **roastable but realistic** eval mistakes

If no candidate passes all hard gates, return **NO-GO** and explain why.

## Research method

### Step 1: Find and rank candidate domains
Find **3 candidate domains**.

For each candidate, score 1–5 on:
- source quality
- rule clarity
- edge-case richness
- realism of user tasks
- suitability for believable traces
- suitability for `error-analysis`
- suitability for one validated evaluator
- on-screen clarity for a general builder audience
- fit for the proposed demo arc

Then choose **one winner** or return **NO-GO**.

### Step 2: Use source discipline
Use **primary sources first**:
- official docs
- help centre / support centre articles
- pricing / plan pages
- policy pages
- changelogs
- public issue trackers
- public forums / community discussions where useful

Use secondary sources only if they add real signal.

For every non-trivial claim:
- cite it inline
- attach `source_ids` in structured outputs

### Step 3: Use grounding discipline
For every rule, task, and example, assign exactly one:
- `direct` = directly grounded in a public example, article, workflow, or rule
- `source_adjacent` = paraphrased / recombined from public material, but still clearly anchored
- `synthetic` = invented only to improve coverage, while staying consistent with the rules

Do not blur these categories.

### Step 4: Optimise for buildability
Assume the later coding agent will build a **small local demo repo** with:
- simple docs
- CSV / JSON tables
- one or two lightweight tools / lookups
- a reviewable trace format
- a small labelled set
- intentionally bad eval artefacts

So your outputs must be:
- concrete
- normalised
- machine-readable
- internally consistent

## Output requirements

Write the output in **exactly** this order.

---

# 1. Executive recommendation

Provide a concise decision memo covering:
- chosen domain
- why it is the best fit
- whether the proposed demo arc fits this domain **well / partly / poorly**
- whether this is a **GO** or **NO-GO**
- whether the arc should stay as-is or be adjusted
- what the eventual toy app should roughly be

End with:
- **Recommended next step**
- **Biggest risk**

---

# 2. Candidate scorecard

Create a table for the 3 candidate domains with:
- domain name
- one-line description
- scores for each criterion
- hard-gate pass/fail
- short verdict

Then clearly state why the winner won.

---

# 3. Source map

List the strongest sources with:
- `source_id`
- title
- source type
- URL
- why it matters
- likely stability (`stable`, `possibly_changing`, `time_sensitive`)

Group by:
- official docs
- help / policy / pricing
- community / issue discussions
- other

---

# 4. Distilled rulebook

Create a structured, normalised rulebook.

For each rule include:
- `rule_id`
- rule statement
- entity / object involved
- condition
- outcome
- exception
- threshold_or_window
- `source_ids`
- grounding label
- confidence (`high`, `medium`, `low`)

Do not paste source prose.
Normalise it into usable rules.

---

# 5. Domain model

Describe the minimum domain model the later coding agent should implement.

Include:
- main entities
- stateful objects
- important fields for each object
- relationships between objects
- any plan / tier / region differences
- what can stay static vs what should be stateful

This should be practical enough to become CSV / JSON files later.

---

# 6. Tool opportunities

Identify **1–2 lightweight tools or lookups** the later app should simulate.

For each tool include:
- `tool_name`
- what it does
- why it matters
- input fields
- output fields
- required backing state
- which tasks depend on it
- what failure modes it introduces
- whether the tool is read-only or action-taking

Keep it small and believable.

---

# 7. User task catalogue

Create **20–25 realistic user task patterns**.

For each task include:
- `task_id`
- task name
- user goal
- 2–3 example user phrasings
- required information
- commonly missing information
- relevant `rule_ids`
- relevant `source_ids`
- what a good answer must do
- grounding label

These should sound like real product requests, not benchmark prompts.

---

# 8. Failure taxonomy

Create a taxonomy of **8–10 product-level failure modes**.

Do **not** use vague buckets like “hallucination” unless split into meaningful product-level types.

For each failure mode include:
- `failure_id`
- name
- plain-English definition
- why it matters
- realistic example
- best check method (`code`, `human`, `judge`)
- level (`trace`, `output`, `tool`, `retrieval`)
- demo usefulness (`high`, `medium`, `low`)

Then identify:
- the best **5** for a roast report
- the best **3** for live trace labelling
- the **single best candidate** for a first validated evaluator

---

# 9. Trace schema

Recommend the trace schema the later coding agent should implement.

Include:
- essential fields
- optional fields
- data types
- what is shown in the review UI
- what is needed for evaluator validation

At minimum include fields for:
- `trace_id`
- input
- relevant evidence / retrieved context
- tool calls
- final output
- metadata
- reviewer annotations
- evaluator outputs

Also explain what makes a trace useful for:
- `eval-audit`
- `error-analysis`
- live labelling
- `validate-evaluator`

---

# 10. Single evaluator recommendation

Recommend exactly **one** failure mode as the first evaluator to build and validate.

Explain:
- why it is the strongest choice for the live demo
- why humans can label it reliably
- why it works as binary pass/fail
- where disagreement is likely to happen
- why it is better than the other candidates for this demo

Then provide:
- a draft binary rubric
- 5 pass examples
- 5 fail examples
- 5 borderline examples

For each example include:
- short example text
- relevant `rule_ids`
- relevant `source_ids`
- grounding label

---

# 11. Seed dataset plan

Design a small but believable seed dataset for the later toy app.

Include:
- recommended total dataset size
- task mix
- easy / medium / hard split
- how many traces should be human-labelled
- how many should be balanced for evaluator validation
- what metadata each example should carry

Then propose a seed set of **25 examples** with this target mix:
- at least **10 `direct`**
- at least **10 `source_adjacent`**
- at most **5 `synthetic`**

For each seed example include:
- `example_id`
- `task_id`
- user message
- required state / context
- relevant `rule_ids`
- relevant `source_ids`
- expected answer contract:
  - `must_include`
  - `must_ask_if_missing`
  - `must_not_do`
  - `tool_expectation` (`required`, `optional`, `forbidden`)
  - `citation_expectation`
- likely failure modes
- difficulty
- grounding label

Important:
Do **not** provide only task patterns here.
Provide actual seed examples that a later agent can turn into test cases.

---

# 12. Validation slice plan

Design a small labelled slice specifically for the first evaluator.

Provide **20 examples**, balanced pass/fail where possible.

For each example include:
- `validation_id`
- linked `example_id` or standalone case
- user message
- relevant context / state
- criterion being judged
- label (`pass` or `fail`)
- short rationale
- relevant `rule_ids`
- relevant `source_ids`
- grounding label

This section exists so the later coding agent can create a believable labelled subset for `validate-evaluator`.

---

# 13. Roastable artefacts plan

Recommend what intentionally bad artefacts the later demo repo should include so `eval-audit` has something real to criticise.

Include:
- one bad headline metric
- one bad judge prompt
- one weak / incomplete labelling setup
- one missing or inadequate analysis step
- one misleading aggregation choice

For each artefact explain:
- why teams actually do this
- why it is wrong
- how it would likely show up in the roast report

Make these feel realistic, not cartoonish.

---

# 14. Demo-arc fit assessment

Assess the proposed run of show against this domain:

- intro
- `eval-audit`
- `error-analysis`
- `write-judge-prompt`
- `validate-evaluator`
- wrap

For each stage, explain:
- what would work well
- what might drag
- what must be pre-baked
- what would be hard to explain live

Then answer clearly:
- Should this arc be used?
- If not, what is the better arc?
- What is the minimum pre-work needed?

---

# 15. Pre-work recommendation

Recommend what should be prepared before recording.

Include:
- what should be built in advance
- what should be labelled in advance
- what traces should be preloaded into the review UI
- what 6–8 traces would be best to label live
- what would make the demo feel most credible to a sceptical audience

Frame this as a recommendation, not an assumption.

---

# 16. Risks and simplifications

List:
- ambiguous source areas
- where the later app will need simplification
- licensing / reuse concerns
- where realism may break if over-simulated
- what the demo should avoid claiming

Be concrete.

---

# 17. Build-ready appendix

End with the following artefacts in fenced code blocks, using the exact filenames below.

## A. `sources.csv`
Columns:
`source_id,title,type,url,why_it_matters,stability`

## B. `rules.csv`
Columns:
`rule_id,entity,condition,outcome,exception,threshold_or_window,source_ids,grounding_label,confidence`

## C. `domain_model.json`
Valid JSON describing:
- entities
- stateful objects
- fields
- relationships
- static vs dynamic data

## D. `tool_specs.json`
Valid JSON describing the simulated tools

## E. `task_catalogue.json`
Valid JSON list of tasks

## F. `failure_taxonomy.json`
Valid JSON list of failure modes

## G. `seed_examples.jsonl`
One valid JSON object per line

## H. `validation_slice.jsonl`
One valid JSON object per line

## I. `trace_schema.json`
Valid JSON schema or practical field spec

## J. `evaluator_candidate.json`
Valid JSON brief for the single recommended evaluator

## K. `roastable_artefacts.json`
Valid JSON spec for the intentionally bad artefacts

## L. `toy_app_spec.json`
Valid JSON build brief including:
- app concept
- user persona
- inputs
- outputs
- docs / tables / JSON files to create
- stateful objects
- tools
- trace schema
- evaluator target
- roastable artefacts
- minimum viable demo flow

## M. `build_notes.md`
A concise markdown note to the later coding agent explaining:
- what to build first
- what to fake / simplify
- what must stay source-faithful
- what should be pre-baked for the demo

## Quality bar

- Prefer grounded public artefacts over imagination
- Prefer crisp product rules over vague domains
- Prefer realistic user language over benchmark language
- Prefer one strong evaluator over many weak ones
- Prefer source-backed simplification over fake realism
- Clearly separate:
  - sourced facts
  - inferred conclusions
  - source-adjacent cases
  - synthetic coverage cases

Write for a technically literate builder who needs to decide whether this domain is credible enough for a podcast demo and, if so, hand it directly to a build agent next.

Use citations throughout the narrative sections.
```

## 2) What changed later in the chat

These are the material changes the user introduced after the original brief:

1. **Move away from existing branded products as the surface area of the toy app.**
   The app can be a mini-clone of an existing product shape, but it should feel like a generic local product rather than “a demo about Shopify/Notion/etc”.

2. **Optimise explicitly for product-level evals above all else.**
   The winning domain should be the one that best supports stateful product behaviour, crisp binary failures, believable traces, and evaluator validation.

3. **Returns & exchanges became the preferred domain.**
   The research should pressure-test that domain specifically and then fully ground it if it passes the hard gates.

4. **Bias toward one rich example per rule family / edge case.**
   It is not enough to have only task patterns or thin examples. The pack should include rich canonical examples that can be promoted into stateful test cases.

5. **Produce a Codex-ready build bundle.**
   The final outputs should not just be a research memo; they should be directly hand-offable to a coding agent.

6. **Add a fully materialised “golden examples” layer.**
   Each canonical example should include real-ish store config, order state, line items, requested action, expected tool result, and good/bad assistant outputs.

7. **Stay in the merchant/product-ops lane, not legal advice.**
   For returns, the assistant should reason about the store’s configured rules and current order state, not broader statutory consumer-rights advice.

## 3) Consolidated current prompt (the version I would reuse)

```text
You are doing a **grounding research pass** for a possible demo-first podcast episode about **product-level LLM evals**.

This is **not** a coding task.
Do **not** build the app.
Do **not** invent policy details.
Your output will be **fed into a later coding agent**. That later agent should be able to build a toy app, dataset, traces, and roastable eval artefacts from your output **without needing to do its own research**.

## Core goal

Choose and fully ground **one public, sourceable domain** for a toy local app that is excellent for **product-level LLM evals**.

The app should feel like a **generic mini-clone of a product shape**, not a branded demo about a specific company. You may use public docs from real products as grounding, but the resulting toy app should stand on its own.

The research pack must support a possible demo arc like this:

1. `eval-audit`
2. `error-analysis`
3. `write-judge-prompt`
4. `validate-evaluator`

Important:
- This arc is **proposed, not approved**
- Your job is to support it **or** recommend a better arc if the domain does not fit it well
- Optimise for the domain that is **best for product-level evals**, not just the easiest domain overall

## Show context

This research is for **Rough Magic**, a short demo-first podcast about how LLMs are changing tech work through real workflows shown on screen. The audience is broad by design: engineers, product managers, designers, marketers, and other builders. The show format is a roughly **20-minute video**, with a strong preference for a **screenshare-led demo** using realistic or mock data. The research should help produce something legible and interesting to a broad builder audience. Use that as a constraint when judging candidate domains.

## Hard preference for this run

Bias strongly toward a **Returns & Exchanges Desk** style domain unless it fails the hard gates.

Frame it as a **generic merchant-facing returns/exchanges operations app** backed by public rules from primary sources. Do **not** frame it as consumer legal advice. Stay in the lane of:
- store-configured rules
- order state
- return and exchange eligibility
- actionability in product

Avoid drifting into broader statutory rights or jurisdiction-specific legal interpretation.

## What success looks like

Your output should make it possible for a later coding agent to create:
- a small example app
- supporting docs / tables / JSON state
- a believable seed dataset
- realistic traces
- one strong candidate evaluator
- a small labelled validation slice
- intentionally bad eval artefacts that feel realistic enough for a roast report
- a **golden examples layer** with rich canonical examples that can be turned directly into stateful test cases
- a **Codex-ready handoff bundle** with enough structure that the builder does not need to guess

The later agent should not need to guess:
- what the domain rules are
- what the user tasks are
- what stateful objects exist
- what tools should exist
- what failure modes matter
- what examples to include
- what trace schema to use
- what bad artefacts to plant for the audit
- what the canonical edge cases are
- what concrete order/store/case states should back those examples

## Constraints

Bias towards domains that are:
- public and sourceable
- narrow enough to explain in under 90 seconds
- rich in rules, thresholds, and exceptions
- realistic at product level
- easy to simulate with 1–2 lightweight tools or lookups
- easy to turn into 6–8 visually distinct traces for live labelling
- capable of producing **one rich canonical example per rule family / edge case**

Avoid:
- medical
- legal advice
- financial advice needing live market data
- highly regulated workflows
- domains where most judgement is subjective
- anything that depends heavily on live APIs or private data

## Hard gates

Only recommend a domain if **all** of these are true:

1. Public primary sources are available without login
2. Rules / thresholds / exceptions are clear enough to normalise
3. At least **15 seed examples** can be grounded directly in public material
4. At least **1 lightweight stateful lookup or action** can be simulated
5. At least **1 crisp binary failure mode** can be labelled by humans and validated
6. A broad builder audience could understand the domain quickly
7. The domain can generate **6–8 traces worth labelling live**
8. The demo repo can contain **roastable but realistic** eval mistakes
9. The domain can support **12–16 fully materialised golden examples** with believable state

If no candidate passes all hard gates, return **NO-GO** and explain why.

## Research method

### Step 1: Find and rank candidate domains
Find **3 candidate domains**.

For each candidate, score 1–5 on:
- source quality
- rule clarity
- edge-case richness
- realism of user tasks
- suitability for believable traces
- suitability for `error-analysis`
- suitability for one validated evaluator
- on-screen clarity for a general builder audience
- fit for the proposed demo arc
- suitability for **rich golden examples**

Then choose **one winner** or return **NO-GO**.

### Step 2: Use source discipline
Use **primary sources first**:
- official docs
- help centre / support centre articles
- pricing / plan pages
- policy pages
- changelogs
- public issue trackers
- public forums / community discussions where useful

Use secondary sources only if they add real signal.

For every non-trivial claim:
- cite it inline
- attach `source_ids` in structured outputs

### Step 3: Use grounding discipline
For every rule, task, and example, assign exactly one:
- `direct` = directly grounded in a public example, article, workflow, or rule
- `source_adjacent` = paraphrased / recombined from public material, but still clearly anchored
- `synthetic` = invented only to improve coverage, while staying consistent with the rules

Do not blur these categories.

### Step 4: Optimise for buildability
Assume the later coding agent will build a **small local demo repo** with:
- simple docs
- CSV / JSON tables
- one or two lightweight tools / lookups
- a reviewable trace format
- a small labelled set
- intentionally bad eval artefacts
- a **golden examples file** with fully materialised state
- a **Codex-ready zip bundle**

So your outputs must be:
- concrete
- normalised
- machine-readable
- internally consistent
- directly reusable by a coding agent

## Output requirements

Write the output in **exactly** this order.

---

# 1. Executive recommendation

Provide a concise decision memo covering:
- chosen domain
- why it is the best fit
- whether the proposed demo arc fits this domain **well / partly / poorly**
- whether this is a **GO** or **NO-GO**
- whether the arc should stay as-is or be adjusted
- what the eventual toy app should roughly be

End with:
- **Recommended next step**
- **Biggest risk**

---

# 2. Candidate scorecard

Create a table for the 3 candidate domains with:
- domain name
- one-line description
- scores for each criterion
- hard-gate pass/fail
- short verdict

Then clearly state why the winner won.

---

# 3. Source map

List the strongest sources with:
- `source_id`
- title
- source type
- URL
- why it matters
- likely stability (`stable`, `possibly_changing`, `time_sensitive`)

Group by:
- official docs
- help / policy / pricing
- community / issue discussions
- other

---

# 4. Distilled rulebook

Create a structured, normalised rulebook.

For each rule include:
- `rule_id`
- rule statement
- entity / object involved
- condition
- outcome
- exception
- threshold_or_window
- `source_ids`
- grounding label
- confidence (`high`, `medium`, `low`)

Do not paste source prose.
Normalise it into usable rules.

---

# 5. Domain model

Describe the minimum domain model the later coding agent should implement.

Include:
- main entities
- stateful objects
- important fields for each object
- relationships between objects
- any plan / tier / region differences
- what can stay static vs what should be stateful

This should be practical enough to become CSV / JSON files later.

---

# 6. Tool opportunities

Identify **1–2 lightweight tools or lookups** the later app should simulate.

For each tool include:
- `tool_name`
- what it does
- why it matters
- input fields
- output fields
- required backing state
- which tasks depend on it
- what failure modes it introduces
- whether the tool is read-only or action-taking

Keep it small and believable.

---

# 7. User task catalogue

Create **20–25 realistic user task patterns**.

For each task include:
- `task_id`
- task name
- user goal
- 2–3 example user phrasings
- required information
- commonly missing information
- relevant `rule_ids`
- relevant `source_ids`
- what a good answer must do
- grounding label

These should sound like real product requests, not benchmark prompts.

---

# 8. Failure taxonomy

Create a taxonomy of **8–10 product-level failure modes**.

Do **not** use vague buckets like “hallucination” unless split into meaningful product-level types.

For each failure mode include:
- `failure_id`
- name
- plain-English definition
- why it matters
- realistic example
- best check method (`code`, `human`, `judge`)
- level (`trace`, `output`, `tool`, `retrieval`)
- demo usefulness (`high`, `medium`, `low`)

Then identify:
- the best **5** for a roast report
- the best **3** for live trace labelling
- the **single best candidate** for a first validated evaluator

---

# 9. Trace schema

Recommend the trace schema the later coding agent should implement.

Include:
- essential fields
- optional fields
- data types
- what is shown in the review UI
- what is needed for evaluator validation

At minimum include fields for:
- `trace_id`
- input
- relevant evidence / retrieved context
- tool calls
- final output
- metadata
- reviewer annotations
- evaluator outputs

Also explain what makes a trace useful for:
- `eval-audit`
- `error-analysis`
- live labelling
- `validate-evaluator`

---

# 10. Single evaluator recommendation

Recommend exactly **one** failure mode as the first evaluator to build and validate.

Bias toward a failure mode that is:
- highly product-critical
- labelable by humans from trace evidence
- binary enough for live validation
- roastable when the team gets it wrong

Explain:
- why it is the strongest choice for the live demo
- why humans can label it reliably
- why it works as binary pass/fail
- where disagreement is likely to happen
- why it is better than the other candidates for this demo

Then provide:
- a draft binary rubric
- 5 pass examples
- 5 fail examples
- 5 borderline examples

For each example include:
- short example text
- relevant `rule_ids`
- relevant `source_ids`
- grounding label

---

# 11. Seed dataset plan

Design a small but believable seed dataset for the later toy app.

Include:
- recommended total dataset size
- task mix
- easy / medium / hard split
- how many traces should be human-labelled
- how many should be balanced for evaluator validation
- what metadata each example should carry

Then propose a seed set of **25 examples** with this target mix:
- at least **10 `direct`**
- at least **10 `source_adjacent`**
- at most **5 `synthetic`**

For each seed example include:
- `example_id`
- `task_id`
- user message
- required state / context
- relevant `rule_ids`
- relevant `source_ids`
- expected answer contract:
  - `must_include`
  - `must_ask_if_missing`
  - `must_not_do`
  - `tool_expectation` (`required`, `optional`, `forbidden`)
  - `citation_expectation`
- likely failure modes
- difficulty
- grounding label

Important:
Do **not** provide only task patterns here.
Provide actual seed examples that a later agent can turn into test cases.

---

# 12. Validation slice plan

Design a small labelled slice specifically for the first evaluator.

Provide **20 examples**, balanced pass/fail where possible.

For each example include:
- `validation_id`
- linked `example_id` or standalone case
- user message
- relevant context / state
- criterion being judged
- label (`pass` or `fail`)
- short rationale
- relevant `rule_ids`
- relevant `source_ids`
- grounding label

This section exists so the later coding agent can create a believable labelled subset for `validate-evaluator`.

---

# 13. Roastable artefacts plan

Recommend what intentionally bad artefacts the later demo repo should include so `eval-audit` has something real to criticise.

Include:
- one bad headline metric
- one bad judge prompt
- one weak / incomplete labelling setup
- one missing or inadequate analysis step
- one misleading aggregation choice

For each artefact explain:
- why teams actually do this
- why it is wrong
- how it would likely show up in the roast report

Make these feel realistic, not cartoonish.

---

# 14. Demo-arc fit assessment

Assess the proposed run of show against this domain:

- intro
- `eval-audit`
- `error-analysis`
- `write-judge-prompt`
- `validate-evaluator`
- wrap

For each stage, explain:
- what would work well
- what might drag
- what must be pre-baked
- what would be hard to explain live

Then answer clearly:
- Should this arc be used?
- If not, what is the better arc?
- What is the minimum pre-work needed?

---

# 15. Pre-work recommendation

Recommend what should be prepared before recording.

Include:
- what should be built in advance
- what should be labelled in advance
- what traces should be preloaded into the review UI
- what 6–8 traces would be best to label live
- what would make the demo feel most credible to a sceptical audience

Frame this as a recommendation, not an assumption.

---

# 16. Risks and simplifications

List:
- ambiguous source areas
- where the later app will need simplification
- licensing / reuse concerns
- where realism may break if over-simulated
- what the demo should avoid claiming

Be concrete.

---

# 17. Golden examples plan

Design a **12–16 example “golden set”** that captures one rich canonical example per major rule family or edge case.

For each golden example include:
- `example_id`
- `rule_family`
- `user_message`
- `actor`
- `store_config`
- `order`
- `line_items`
- `return_rule_snapshot` or equivalent saved-rule state
- `requested_action`
- `expected_tool_result`
- `good_answer`
- `bad_answer`
- `label`
- `failure_mode`
- relevant `rule_ids`
- relevant `source_ids`
- grounding label

These should be rich enough that a later coding agent can turn them directly into:
- JSON state
- traces
- demo fixtures
- validation cases
- roastable examples

---

# 18. Build-ready appendix

End with the following artefacts in fenced code blocks, using the exact filenames below.

## A. `sources.csv`
Columns:
`source_id,title,type,url,why_it_matters,stability`

## B. `rules.csv`
Columns:
`rule_id,entity,condition,outcome,exception,threshold_or_window,source_ids,grounding_label,confidence`

## C. `domain_model.json`
Valid JSON describing:
- entities
- stateful objects
- fields
- relationships
- static vs dynamic data

## D. `tool_specs.json`
Valid JSON describing the simulated tools

## E. `task_catalogue.json`
Valid JSON list of tasks

## F. `failure_taxonomy.json`
Valid JSON list of failure modes

## G. `seed_examples.jsonl`
One valid JSON object per line

## H. `validation_slice.jsonl`
One valid JSON object per line

## I. `trace_schema.json`
Valid JSON schema or practical field spec

## J. `evaluator_candidate.json`
Valid JSON brief for the single recommended evaluator

## K. `roastable_artefacts.json`
Valid JSON spec for the intentionally bad artefacts

## L. `toy_app_spec.json`
Valid JSON build brief including:
- app concept
- user persona
- inputs
- outputs
- docs / tables / JSON files to create
- stateful objects
- tools
- trace schema
- evaluator target
- roastable artefacts
- minimum viable demo flow

## M. `build_notes.md`
A concise markdown note to the later coding agent explaining:
- what to build first
- what to fake / simplify
- what must stay source-faithful
- what should be pre-baked for the demo

## N. `golden_examples.jsonl`
One valid JSON object per line with fully materialised canonical examples.

## O. `build_state.json`
A combined JSON state file that includes the minimal store/order/case/inventory/staff state needed to power the golden examples and seed traces.

## P. `CODEX_BUILD_PROMPT.md`
A concise handoff prompt to the later coding agent explaining exactly what to build, what to fake, what to keep faithful, and what outputs to generate.

## Quality bar

- Prefer grounded public artefacts over imagination
- Prefer crisp product rules over vague domains
- Prefer realistic user language over benchmark language
- Prefer one strong evaluator over many weak ones
- Prefer source-backed simplification over fake realism
- Prefer one rich canonical example per edge-case family over lots of thin examples
- Clearly separate:
  - sourced facts
  - inferred conclusions
  - source-adjacent cases
  - synthetic coverage cases

Write for a technically literate builder who needs to decide whether this domain is credible enough for a podcast demo and, if so, hand it directly to a build agent next.

Use citations throughout the narrative sections.
```
