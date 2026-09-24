# ThinkPixel Development Alignment

This document defines the current **cross-repository development priority** for ThinkPixel.

It exists to prevent individual component plans from becoming more important than proving that the platform works.

It controls **what we prioritize**, not the meaning of published contracts, accepted ADRs, or core security boundaries.

---

## Current objective

The immediate objective is to produce a **useful, reproducible ThinkPixel vertical slice** that can serve both as:

1. a compelling platform demo; and
2. the basis of release-candidate builds for the participating components.

For this phase, system-level proof is more important than advancing every repository toward independent completeness.

A component should receive substantial development effort when it directly advances the current vertical slice or fixes a problem discovered while exercising it.

---

## North Star scenario

The current target scenario is:

> Give an approved Codex agent a GitHub repository, let it inspect and modify work through isolated execution, route its model access through ThinkPixelLLMGW, route a governed GitHub operation through ThinkPixelTG, destroy its execution sandbox completely, reconstruct execution on fresh compute, and continue the same durable session/workspace under ThinkPixelAG authority.

The desired demonstration should make these properties visible:

**Agents are untrusted.
Authority lives outside the agent.
Credentials live outside the agent.
Compute is disposable.
State is durable.
Side effects are governed.**

The audience should see the architecture working rather than being shown a diagram describing how it might work.

---

## Current critical path

Development priority is currently:

### 1. ThinkPixelAR — primary critical path

AR should receive the majority of implementation effort until the complete scenario works.

The important milestones are:

* start, stop, and supervise a real harness through `agentd`;
* run Codex through the real sandbox path;
* create a Session/Execution and stream observable events;
* persist the state needed for continuation;
* destroy the execution sandbox;
* create replacement compute;
* resume the same logical Session/workspace;
* consume AG authority rather than relying on local authority;
* supply governed LLMGW and TG access to the runtime.

Do not delay these milestones for unrelated AR completeness work.

### 2. ThinkPixelAG — integrate, do not broaden

AG is used for the demo as the authority/control-plane component.

Implement integration fixes required for:

* governed Run authority;
* resource/runtime authorization;
* leases/fencing where required;
* cancellation/revocation;
* authorization required by TG/AR.

Avoid expanding AG into unrelated new capabilities until the vertical slice works.

### 3. ThinkPixelLLMGW — integrate, do not re-qualify everything

Use the existing gateway path for model access.

The target is a real Codex/model interaction routed through LLMGW with observable request/accounting identity.

Do not block this path on exhaustive provider qualification, production deployment qualification, or support for every model/provider.

One working, reproducible route is enough for the current RC path.

### 4. ThinkPixelTG — integrate the GitHub operation

Use TG for at least one visible governed GitHub side effect.

Prefer something easy to understand during a demo, such as:

* posting a PR review comment;
* creating or updating a review;
* another bounded operation that visibly proves governed tool execution.

The agent must not receive the long-lived GitHub credential.

TG should resolve/use the downstream credential and produce evidence linking the invocation to platform authority.

---

## Supporting components

The following components remain part of the ThinkPixel architecture but should not block the first integrated RC.

### ThinkPixelWS

Do not wait for complete ThinkPixelWS product maturity.

Use the smallest durable Workspace implementation or adapter necessary to prove:

* persistent work context;
* state surviving sandbox destruction;
* reconstruction on replacement compute;
* safe single-writer behavior needed by the demonstrated scenario.

Keep the boundary replaceable so the proper WS integration can replace the temporary implementation later.

### ThinkPixelMP

Do not require dynamic marketplace resolution for the first demo.

It is acceptable to pin immutable runtime/agent artifacts directly in demo configuration, preferably by digest.

Integrate MP when artifact qualification/resolution becomes necessary to the next demonstrated capability.

### ThinkPixelMEM

Not on the current critical path.

The first platform demo does not require long-term learned memory.

Do not add MEM merely because an agent platform is expected to have memory.

### ThinkPixelGR

Not on the current critical path unless a concrete demonstrated operation requires it.

Do not block the first vertical slice on broad guardrail integration or detector coverage.

Guardrails become valuable when there is a real model/tool/retrieval path to evaluate.

### ThinkPixelXP

Not required for the first integrated demo.

Add experimentation/evaluation once there is a stable execution path whose variants or outcomes are worth comparing.

### ThinkPixelSR

ThinkPixelSR currently represents an existing search/RAG lineage and use case.

Its future integration with the wider agent platform should be driven by a concrete use case rather than forced into the first agent-runtime demo.

---

## Demo / RC definition

For the current phase, **release candidate** does not mean:

> production-ready for every intended environment and configuration.

It means:

> a coherent, reproducible implementation of a clearly defined capability, with known limitations documented and with the demonstrated path working reliably enough for others to exercise.

A narrow RC is acceptable.

For example, the first integrated ThinkPixel RC may intentionally support:

* one harness: Codex;
* one sandbox path;
* one Kubernetes/runtime configuration;
* one model-provider route through LLMGW;
* one TG connector: GitHub;
* one persistence strategy;
* one scripted golden-path scenario.

That is sufficient if the path is real, reproducible, and architecturally meaningful.

Broader qualification belongs to release promotion.

---

## Priority rules

When deciding what to work on next, use this order.

### P0 — Make the golden path move

Anything preventing the current end-to-end scenario from executing.

Examples:

* harness cannot start;
* sandbox cannot be created;
* events cannot flow;
* state cannot survive sandbox replacement;
* AG authority cannot be consumed;
* LLM calls cannot route through LLMGW;
* GitHub operation cannot route through TG.

Fix these first.

### P1 — Make the golden path trustworthy

Problems that make the scenario unsafe, misleading, or unreproducible.

Examples:

* credentials leaking into the sandbox;
* authority expansion;
* stale/fencing violations;
* corruption or loss of demonstrated durable state;
* non-idempotent destructive retries;
* inability to associate operations with stable IDs;
* obvious security flaws in the demonstrated path.

### P2 — Make the golden path repeatable

Work that lets another developer reproduce the scenario reliably.

Examples:

* deterministic setup/reset;
* small deployment scripts;
* useful diagnostics;
* fixture/sample repository;
* one-command or short-sequence demo startup;
* documented required configuration.

### P3 — Promote the RC

Do this after the working vertical slice exists.

Examples:

* wider test matrices;
* packaging polish;
* expanded compatibility;
* broader provider support;
* SBOM/license/compliance automation;
* deployment hardening;
* HA/backup/restore;
* comprehensive observability;
* performance qualification.

### P4 — Future platform work

Do not prioritize unless it becomes necessary for an active use case.

---

## What must not be traded away

Demo-first does not mean architecture-last.

The following remain non-negotiable unless explicitly redesigned:

* governed authority remains outside the agent;
* untrusted execution cannot grant itself additional authority;
* long-lived credentials remain outside untrusted harness state;
* TG retains responsibility for governed downstream side effects and credentials;
* LLMGW retains responsibility for governed provider/model access and credentials;
* component ownership boundaries remain explicit;
* components do not couple through each other's private databases or internal types;
* published cross-component contracts are not silently broken;
* disposable compute must actually be disposable where the demo claims it is;
* durable state must actually survive that disposal where the demo claims it does.

If the shortcut destroys the property being demonstrated, it is not an acceptable shortcut.

---

## What may be intentionally deferred

The following should normally **not block** the first useful RC unless a concrete issue makes them necessary:

* complete feature coverage;
* exhaustive failure-mode testing;
* exhaustive security qualification outside the demonstrated path;
* exhaustive model/provider matrices;
* unused adapters;
* generalized plugin frameworks;
* warm pools;
* multi-cluster or multi-region support;
* production HA;
* sophisticated autoscaling;
* broad dashboards;
* extensive chaos testing;
* complete backup/restore qualification;
* polished Helm/operator experience;
* broad performance tuning;
* comprehensive migration support for unreleased schemas;
* complete documentation coverage;
* exhaustive software-supply-chain automation;
* comprehensive license/provenance auditing beyond known obligations;
* speculative abstractions for future components.

Known security, safety, legal, or compatibility problems must still be surfaced and handled proportionately.

The goal is to defer **qualification breadth**, not conceal known defects.

---

## Licensing and third-party software

Licensing matters, but it should be handled proportionately to the current stage.

For demo/RC work:

* prefer permissively licensed dependencies when practical;
* preserve required copyright/license notices;
* preserve known attribution and redistribution requirements;
* avoid dependencies with known terms incompatible with the intended distribution model;
* record concrete unresolved issues.

Do not make exhaustive license research, dependency provenance machinery, or final distribution qualification a prerequisite for exercising an otherwise valid RC path unless a real legal/distribution blocker has been identified.

Final release promotion may impose stricter qualification requirements.

---

## Verification philosophy

Verification should answer:

> **Does the capability we claim actually work?**

For the current phase, prioritize:

1. focused tests around changed behavior;
2. integration tests across affected boundaries;
3. actual golden-path execution;
4. captured evidence from real sandbox/model/tool interactions.

A passing repository-wide test matrix is useful.

It is not a substitute for a platform scenario that actually runs.

Likewise, an unrelated aggregate test failure should be reported but should not automatically consume implementation effort unless it invalidates the current RC path.

---

## Golden-path evidence

The integrated scenario should eventually make it possible to correlate at least:

```text
AG Run
  ↓
AR Session / Execution
  ↓
LLMGW model request(s)
  ↓
TG governed invocation
  ↓
GitHub side effect
```

and then demonstrate:

```text
running sandbox
  ↓
checkpoint / durable state
  ↓
sandbox destroyed
  ↓
fresh sandbox created
  ↓
same logical Session/workspace resumed
```

The exact observability mechanism may evolve.

Do not delay the demo to build a perfect distributed tracing platform.

Stable IDs and readable logs/output are sufficient initially.

---

## Demo ergonomics

Do not block the first demonstration on a graphical UI.

A terminal-driven demonstration is sufficient and may be preferable because it exposes what the platform is actually doing.

Aim for:

* deterministic sample repository/task;
* predictable setup/reset;
* minimal manual preparation;
* obvious Run/Session/Execution identifiers;
* visible sandbox destruction;
* visible continuation after reconstruction;
* visible governed GitHub side effect;
* concise output rather than noisy internal logs.

---

## Definition of success

The immediate milestone is reached when a person can observe this sequence:

1. AG authorizes a Run.
2. AR starts the agent in isolated disposable compute.
3. Codex receives and works on a repository task.
4. Its model access goes through LLMGW.
5. A GitHub side effect goes through TG without exposing the long-lived credential to the agent.
6. The sandbox is deliberately destroyed.
7. AR reconstructs execution using fresh compute.
8. The same logical work context continues.
9. Revocation/cancellation can stop further governed work if included in the demonstrated slice.

Once this works reproducibly, stop and treat it as a milestone.

Do not immediately expand the implementation until the working capability has been packaged, documented succinctly, and tagged as the appropriate demo/RC baseline.

---

## Guiding rule

When there is a choice between:

> making one repository more theoretically complete

and

> making the ThinkPixel platform visibly do something useful,

prefer the second **unless doing so would violate the security, authority, compatibility, or ownership boundaries that make the result genuinely ThinkPixel**.
