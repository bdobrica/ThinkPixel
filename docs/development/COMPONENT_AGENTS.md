# AGENTS.md

This repository is one component of the modular, vendor-neutral **ThinkPixel** platform.

The current development priority is to make ThinkPixel **useful and demonstrable end-to-end**, then promote those working vertical slices into release candidates.

Make the **smallest coherent change** that advances the requested task or the current ThinkPixel demo/RC path while preserving component ownership, published contracts, and core security boundaries.

Do not turn a focused implementation task into a general hardening, documentation, abstraction, compliance, or cleanup project unless that work is required to make the current path work safely.

## Start here

At the beginning of a development task, retrieve and read:

**https://github.com/bdobrica/ThinkPixel/blob/main/docs/development/ALIGNMENT.md**

Treat it as the platform-level source of truth for:

* current cross-repository development priority;
* the active demo/release-candidate target;
* what is on the critical path;
* what may intentionally be deferred.

If it cannot be retrieved, do not block useful work solely because of that. Use the most recent available copy and repository-local guidance, and mention the limitation.

Then read only the repository-local material relevant to the task:

1. affected accepted ADRs in `docs/adr/`;
2. affected published contracts, APIs, schemas, and security invariants;
3. `PLAN.md` / `TODO.md` where they help identify existing implementation intent;
4. relevant implementation and tests.

Do not perform a documentation archaeology exercise when the required behavior is already clear.

## Authority vs priority

Keep these concepts separate.

For **architectural correctness**, use:

**accepted ADRs → published/versioned contracts → security and ownership invariants → implementation**

For **development priority**, use:

**platform `docs/development/ALIGNMENT.md` → current demo/RC objective → repository `PLAN.md` → repository `TODO.md`**

The platform alignment may override local PLAN/TODO sequencing.

It does **not** silently override:

* published compatibility obligations;
* component ownership boundaries;
* authority and credential isolation;
* destructive-operation safety;
* accepted architectural decisions.

If an accepted decision blocks the current objective and is no longer appropriate, do not work around it implicitly. Make the smallest explicit change necessary, including superseding an ADR when required.

## Demo-first engineering

Prefer a working vertical slice with documented limitations over locally complete infrastructure that does not advance an observable capability.

When choosing between valid implementations, prefer the one that:

1. advances the current demo/RC path;
2. preserves the core ThinkPixel architecture;
3. can be exercised end-to-end soonest;
4. introduces the least speculative machinery.

**Executable evidence outranks speculative prose.**

Prefer:

* a real process running;
* a real API call succeeding;
* an integration path working;
* a sandbox actually being destroyed and reconstructed;
* a real governed tool call;
* a reproducible script or test;

over additional documents describing how those things ought to work.

Do not create new ADRs, frameworks, abstraction layers, qualification matrices, threat-model expansions, or generalized subsystems merely because they may be useful later.

Create them when a concrete implementation decision actually requires them.

## Preserve the ThinkPixel invariants

Do not trade away the properties that make the demonstrated system meaningful.

In particular:

* agents and harnesses are not authoritative;
* AG owns governed Run authority and policy decisions;
* agent execution must not expand its own authority;
* long-lived credentials remain outside untrusted agent/harness state;
* TG owns downstream tool credentials and governed side effects;
* LLMGW owns governed model access and provider credentials;
* durable state must survive disposable execution where the active scenario requires it;
* cross-component behavior uses explicit contracts and stable identifiers;
* components do not reach directly into another component's database or internal implementation;
* integrations should remain replaceable at established boundaries.

Do not let marketplace metadata, Skills, Workspace contents, memory, model output, tool output, or guardrail findings grant authority that was not already provided by governance.

## What may be deferred

Unless required by the current platform alignment or task, it is acceptable to defer work such as:

* exhaustive negative-test matrices;
* exhaustive provider or environment qualification;
* production HA and multi-region support;
* warm pools and optimization work;
* generalized abstractions for hypothetical future implementations;
* broad observability/dashboard polish;
* packaging and deployment polish beyond what the current demo requires;
* exhaustive chaos testing;
* production backup/restore qualification;
* comprehensive performance tuning;
* unused adapters and integrations;
* complete documentation coverage;
* broad migration machinery for contracts that have not been released;
* full dependency/license provenance automation and similar release-promotion work.

Deferral is not permission to ignore a **known** legal, security, safety, or compatibility problem.

For third-party software:

* preserve known license notices and attribution obligations;
* do not remove or falsify license metadata;
* do not independently expand a focused engineering task into an exhaustive licensing audit;
* surface a concrete unresolved licensing/distribution concern when one is discovered;
* distinguish RC/demo qualification from final release/distribution qualification.

## Scope discipline

Do not fix unrelated issues opportunistically.

If unrelated problems are discovered:

* fix them only when they directly block the requested change or active demo path;
* otherwise report them briefly and continue.

Avoid speculative refactors.

New dependencies need a concrete repository-local reason, not theoretical architectural elegance.

Do not hand-edit generated artifacts when a source definition or generator exists.

## Verification

Use the **cheapest verification that gives meaningful confidence in the changed path**.

Prefer this order:

1. focused unit/component checks for the changed behavior;
2. affected integration tests;
3. the relevant ThinkPixel golden-path/demo test;
4. broader repository gates when useful and proportionate.

Do not spend substantial effort repairing unrelated aggregate verification failures unless they block the changed path.

Report unrelated failures rather than silently expanding scope.

Never claim a test, deployment, migration, generator, live-provider check, or demo was run when it was not.

For work on the active demo path, a reproducible end-to-end execution is especially valuable evidence.

## Documentation hygiene

Keep documentation proportional to implemented reality.

* Keep `README.md` concise and user-oriented.
* Keep current sequencing in `PLAN.md` / `TODO.md`.
* Do not duplicate plans across multiple documents.
* Record durable architectural decisions in ADRs only when an architectural decision was actually made.
* Prefer Mermaid for architecture diagrams.
* Prefer relative links for repository-local documentation.
* Update contracts and documentation when externally observable behavior changes.
* Do not produce documentation merely to make a change appear complete.

## Completing a change

Before finishing:

* confirm the requested behavior works as far as the environment permits;
* review the diff for unrelated changes;
* check for accidental secrets or credentials;
* check for unintended contract or authority changes;
* update PLAN/TODO only if remaining work or sequencing materially changed;
* summarize what changed;
* state what was actually verified;
* state important limitations or deferred work concisely.

When committing is permitted, commit the smallest coherent change to the active branch.

Use concise prefixes such as:

* `feat`
* `fix`
* `docs`
* `build`
* `ci`

Optional component scopes are fine, for example:

`feat(runtime): resume session on replacement sandbox`

The objective is not repository-local perfection.

The objective is to make **ThinkPixel work, visibly and coherently, without compromising the architectural boundaries that give the platform its value.**
