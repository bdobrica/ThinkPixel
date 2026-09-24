# ThinkPixel Development Guidance

This directory contains the **cross-repository development guidance** used to keep ThinkPixel implementation focused on useful, demonstrable platform capabilities.

It is intentionally separate from component-local architecture, contracts, plans, and TODO lists.

## Files

### [`COMPONENT_AGENTS.md`](COMPONENT_AGENTS.md)

Shared instructions for coding agents and development harnesses working on ThinkPixel repositories.

It defines how implementation tasks should be approached, including:

* demo-first engineering;
* scope discipline;
* architectural and security invariants;
* verification expectations;
* what may be deferred during demo/RC development;
* how platform-level priorities interact with local plans and TODOs.

Component repositories may reuse this file directly or carry a local copy that points back here.

### [`ALIGNMENT.md`](ALIGNMENT.md)

The current **platform-level development priority**.

It defines:

* the active ThinkPixel demo / release-candidate objective;
* the current golden-path scenario;
* which components are on the critical path;
* which work should be prioritized or deferred;
* what constitutes success for the current platform milestone.

This file is expected to change as ThinkPixel moves from one demonstrated capability to the next.

## How to use these documents

ThinkPixel distinguishes between **architectural authority** and **development priority**.

For architectural correctness, component-local authoritative sources still apply:

```text
accepted ADRs
→ published/versioned contracts
→ security and ownership invariants
→ implementation
```

For development priority:

```text
docs/development/ALIGNMENT.md
→ current demo / RC objective
→ component PLAN.md
→ component TODO.md
```

`ALIGNMENT.md` may override local sequencing, but it does not silently override published contracts, security boundaries, component ownership, or accepted architectural decisions.

## Updating this guidance

Update `ALIGNMENT.md` when the active cross-component milestone changes.

Update `COMPONENT_AGENTS.md` only when the general development behavior expected across ThinkPixel repositories changes.

Avoid turning either file into:

* a changelog;
* a duplicate project plan;
* a repository-specific TODO list;
* a replacement for ADRs or contracts.

The purpose of this directory is simple:

> Keep ThinkPixel development focused on making the platform work end-to-end, while preserving the architectural boundaries that make the result meaningful.
