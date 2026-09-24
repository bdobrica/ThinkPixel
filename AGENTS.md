# AGENTS.md

This is the **umbrella and metadata repository** for the ThinkPixel platform.

It does not own component implementation.

This repository exists to coordinate the ecosystem:

* cross-component architecture;
* platform development alignment;
* component metadata;
* compatibility and release manifests;
* end-to-end integration scenarios;
* decisions spanning multiple repositories.

The objective is not to make this repository large.

The objective is to keep the independently developed ThinkPixel components **coherent, testable, and moving toward a working platform**.

---

## Start here

Before making a non-trivial change, read:

1. [`./docs/development/ALIGNMENT.md`](docs/development/ALIGNMENT.md) — the current cross-repository objective and critical path;
2. [`./catalog/components.yaml`](./catalog/components.yaml) — canonical component metadata;
3. [`./releases/README.md`](./releases/README.md) — platform release and compatibility model, when the task concerns integration or release work.

Then read only the material directly relevant to the task.

Do not perform documentation archaeology when the required behavior and ownership are already clear.

---

## Repository boundary

This repository owns **coordination**, not implementation.

Appropriate changes include:

* changing cross-component development priorities;
* updating component catalog metadata;
* documenting platform-level architecture;
* defining or updating an end-to-end integration scenario;
* recording a tested platform release manifest;
* documenting a decision that spans multiple components;
* correcting ecosystem links or ownership information;
* adding validation for umbrella metadata.

Implementation of AG, AR, WS, MEM, MP, TG, LLMGW, GR, XP, Infra/SR, or another component belongs in that component's repository.

Do not add:

* service implementation code;
* component-specific API implementations;
* component-local migrations;
* component-local deployment machinery;
* private component configuration;
* duplicated component documentation.

If a platform task requires implementation changes in another repository, identify the owning component rather than moving that implementation into this repository.

---

## Authority and priority

Keep **architectural authority** separate from **development priority**.

For architectural correctness, prefer:

```text
accepted component ADRs
        ↓
published/versioned contracts
        ↓
security and ownership invariants
        ↓
implementation
```

For cross-repository development priority, prefer:

```text
./docs/development/ALIGNMENT.md
      ↓
active platform scenario
      ↓
component-local PLAN/TODO
```

`./docs/development/ALIGNMENT.md` may change which work should happen first.

It does not silently override:

* published compatibility obligations;
* accepted architectural decisions;
* component ownership boundaries;
* authority isolation;
* credential isolation;
* destructive-operation safety.

If an existing architectural decision genuinely blocks the active objective, change that decision explicitly rather than bypassing it implicitly.

---

## Preserve the ThinkPixel invariants

Cross-component work must preserve the properties that make ThinkPixel meaningful.

In particular:

* agents and harnesses are not authoritative;
* governed authority remains outside untrusted execution;
* untrusted execution cannot expand its own authority;
* long-lived credentials remain outside agent/harness state;
* ThinkPixelAG owns governed Run authority and policy decisions;
* ThinkPixelTG owns governed downstream tool execution and downstream tool credentials;
* ThinkPixelLLMGW owns governed model access and provider credentials;
* durable state must survive disposable execution where the active scenario requires it;
* component boundaries use explicit contracts and stable identifiers;
* one component must not depend on another component's private database or internal implementation;
* replaceable integrations should remain replaceable.

Marketplace metadata, Skills, Workspace contents, memory, model output, tool output, retrieval results, and guardrail findings are **inputs or evidence**, not sources of authority.

---

## Prefer vertical progress

When several changes are valid, prefer the one that:

1. advances the active end-to-end scenario;
2. preserves ThinkPixel's architectural boundaries;
3. can be exercised against real components soonest;
4. introduces the least speculative machinery.

A real narrow path is more valuable than a broad theoretical one.

Prefer executable evidence such as:

* a real process running;
* a real cross-component API interaction;
* a real sandbox lifecycle;
* a real governed model request;
* a real governed tool invocation;
* a real recovery sequence;
* a reproducible integration test or script.

Do not create generalized frameworks, abstraction layers, new components, qualification systems, or architecture documents merely because they may eventually be useful.

Introduce them when a concrete demonstrated capability requires them.

---

## Respect component ownership

Before describing a new platform responsibility, determine which component owns it.

Do not solve ambiguity by allowing multiple components to own the same authority or durable state.

When a boundary is unclear:

1. identify the state or authority involved;
2. identify which component must remain authoritative for it;
3. define the smallest explicit contract necessary across the boundary;
4. avoid sharing private storage or implementation types.

If no existing component cleanly owns the responsibility, surface that architectural question explicitly.

Do not invent a new ThinkPixel component casually.

---

## Metadata rules

[`./catalog/components.yaml`](./catalog/components.yaml) is the canonical machine-readable ecosystem catalog.

When component metadata changes:

* update the catalog first;
* keep identifiers stable unless a migration is intentional;
* use the documented maturity vocabulary;
* distinguish component maturity from platform integration;
* distinguish actual repository names from future/intended names;
* do not claim versions, qualification, integrations, or releases that have not occurred.

Human-readable tables should eventually be generated or validated against the catalog rather than becoming independent sources of truth.

---

## Alignment rules

[`./docs/development/ALIGNMENT.md`](./docs/development/ALIGNMENT.md) is intentionally temporary.

It should describe:

* the current platform objective;
* the current golden path;
* which components are on the critical path;
* what each participating component must prove next;
* what is intentionally deferred;
* what evidence defines success.

It should not become:

* a permanent architecture specification;
* an exhaustive component roadmap;
* a release-policy document;
* a duplicate of the root README;
* a duplicate of component-local PLAN/TODO files.

When the active platform objective changes, update `./docs/development/ALIGNMENT.md` rather than accumulating historical campaigns inside it.

Durable architectural knowledge belongs elsewhere.

---

## Release rules

A platform release is a **tested combination**, not a synchronized version number.

Follow [`./releases/README.md`](./releases/README.md) for release semantics.

Do not create a platform release manifest merely because individual components have release tags.

A manifest should represent a combination that was actually exercised together.

Use immutable source and artifact identities wherever practical.

Do not claim compatibility outside the scope supported by evidence.

---

## Scope discipline

Make the smallest coherent change that advances the requested task.

Do not fix unrelated problems opportunistically unless they directly block the change or active platform path.

If an unrelated problem is discovered:

* surface it briefly;
* identify the likely owning repository when useful;
* continue with the requested work.

Avoid speculative refactors.

New machinery needs a concrete platform-level reason.

Do not hand-edit generated content when a canonical source or generator exists.

---

## Verification

Use the cheapest verification that provides meaningful confidence in the changed behavior.

For documentation and metadata changes, verify as applicable:

1. Markdown and YAML parse correctly;
2. relative links resolve;
3. referenced repositories and paths exist;
4. catalog identifiers are internally consistent;
5. generated/derived views do not contradict canonical metadata;
6. release manifests satisfy their schema when one exists.

For changes affecting an integration scenario, prefer:

1. focused component verification;
2. affected cross-component integration checks;
3. actual golden-path execution.

A repository-wide green check is useful.

It is not a substitute for exercising the capability being claimed.

Never state that a test, demo, deployment, integration, or release was verified when it was not.

---

## Documentation rules

Keep documentation proportional to implemented reality.

* Use the root `README.md` for the public platform overview.
* Use `catalog/components.yaml` for component metadata.
* Use `./docs/development/ALIGNMENT.md` for current cross-repository priorities.
* Use `releases/` for tested platform combinations.
* Use component repositories for component-specific documentation.
* Use ADRs for durable architectural decisions when a decision actually warrants one.
* Prefer Mermaid for architecture and flow diagrams.
* Prefer relative links for repository-local documentation.
* Avoid duplicating the same fact across documents when one source can be canonical.

Documentation should clarify reality, not make incomplete work appear finished.

---

## Completing a change

Before finishing:

* review the diff for unrelated changes;
* confirm links and metadata affected by the change;
* check for accidental secrets or credentials;
* check for unintended changes to component ownership or authority;
* state what changed;
* state what was actually verified;
* state important limitations or unresolved issues concisely.

When commits are part of the task, prefer small coherent commits with concise prefixes such as:

```text
docs:
meta:
release:
ci:
```

The objective of this repository is not implementation completeness.

It is to ensure that **ThinkPixel develops as one coherent platform without erasing the boundaries that make its components independently trustworthy and replaceable.**
