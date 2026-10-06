# Personal LikeC4 POC Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Publish a runnable, single-project LikeC4 template with generic bounded contexts and an approachable Portuguese README.

**Architecture:** One npm package at the root discovers all `.c4` files under `architecture/`. Shared context definitions and relationships live in `architecture/models/`; static and dynamic views live in `architecture/views/orders/`.

**Tech Stack:** LikeC4 1.59.4, npm, YAML, Markdown.

**Spec:** `docs/superpowers/specs/2026-10-06-likec4-personal-poc-design.md`

## Global Constraints

- One LikeC4 project and one root `package.json`; no Astro, per-domain packages or configuration generators.
- Use `architecture/views/` to group diagrams; no template subproject.
- Use generic fictional examples and Portuguese, conversational technical prose in the README.
- Keep model relationships in `architecture/models/relationships.c4` and diagrams outside `architecture/models/`.

## Review Focus

- A new reader must understand how `bounded-contexts.yaml` maps to `bounded-context-*.c4` files.
- Every view reference must resolve to a defined model element.
- `npm run validate` and `npm run build` must succeed from a fresh root installation.
- README commands and links must correspond to the delivered project and official documentation.
- The final tree must contain no Astro or per-domain project artifacts.

---

### Task 1: Root project and shared model

**Files:** Create `package.json`, `package-lock.json`, `.gitignore`, `architecture/bounded-contexts.yaml`, and all eight `.c4` files listed in the spec's `architecture/models/` tree.

**Interfaces:** Produces elements `customer`, `catalog.catalog_api`, `orders.orders_api`, `orders.orders_db`, `orders.checkout.checkout_web`, `orders.checkout.checkout_api`, `notifications.order_events`, and `notifications.notification_worker` for the views.

- [x] Step 1: Add the minimal root npm package and ignore generated output; run `npm install` to generate a lockfile.
- [x] Step 2: Define the catalogue, LikeC4 notation, actors, contexts, subcontext, and static relationships using the exact element paths above.
- [x] Step 3: Run `npm run validate`; fix syntax or model errors until it succeeds.

### Task 2: Example views

**Files:** Create `architecture/views/orders/orders-landscape.c4` and `architecture/views/orders/place-order-level-3.c4`.

**Interfaces:** Consumes the Task 1 element paths. Produces a static overview and one dynamic order flow in the same UI folder.

- [x] Step 1: Define the static view showing customer and the three contexts.
- [x] Step 2: Define the dynamic view tracing customer to checkout, orders database, event queue, and notification worker.
- [x] Step 3: Run `npm run validate` and `npm run build`; fix any broken references, syntax, or layout drift.

### Task 3: Open source README and final audit

**Files:** Create `README.md`; review `.gitignore` and the complete tree.

**Interfaces:** Documents the runnable project and how to replace the fictional example.

- [x] Step 1: Explain LikeC4, the model/view relationship, bounded contexts, each directory, local commands, adaptation, contribution, and license in conversational Portuguese with small code examples and official references.
- [x] Step 2: Verify README commands and links, and inspect the tree for unwanted Astro, per-domain package/config, source-specific content, and old grouping terminology.
- [x] Step 3: Run `npm run validate`, `npm run build`, `git diff --check`, and review the generated site artifacts and final diff.
