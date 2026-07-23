---
name: sccs-composition
description: The COMPOSITION operator: build a whole from ordered component connections; required parts bound the share/transfer unit. Use when assembling parts into a whole or moving a structure as one unit.
---

# SCCS — Composition (the heart)

> **SCCS bundle map:** cross-skill coordination, the skill graph, and the global terminology
> reconciliations live in [references/SCCS-INTEGRATION.md](references/SCCS-INTEGRATION.md).

**Outgrowth of the SNS.** Composition *is* an SNS connector: the `_` **component** and the **combination** of sccs-naming. This skill is that connector made into a discipline; every part it assembles is a head-noun-last name under `category⟨type⟩`.

**Precedence:** the original engine is authoritative; this is a non-destructive overlay.

Composition is the central operation of SCCS. A **composition** is a whole assembled from **ordered
component connections** and **defined by its prototype**. It is not a loose set (that is a
*combination*) and not a single part (that is a *component*) — it is the ordered, prototype-defined
whole. Everything the system stores and moves is a composition or a part of one, which is why this
skill sits at the heart and is consulted first.

## Two faces — one operator

```
the prototype of the concept   (genesis: defines every composition's order · obligation · cardinality)
├─ concept-side composition     → a referent-0 concept built from its component connections
│                                  e.g. the price = the price quantity + the price currency unit
└─ connection-side composition  → THE PRIMARY CASE: the composition of the connection (the trunk)
                                   — a trunk + the component connections its prototype defines
```

The connection side is primary because **relations are what bind concepts into structures**: to move
a structure you move its connection-composition, and the concepts it points at come along by
reference. Concept-side composition is its dual.

## The prototype defines the composition

A composition's parts, their **order**, which are **required** (obligation:
required · requested · optional · conditional · prohibited), and **how many** (cardinality:
minimum/maximum occurrence) are all set by the prototype — `sccs-prototype` on the concept side,
`sccs-trunk-prototype` on the connection side. So the prototype is not only a renderer; it is the
**definition of the composition**, and the **required components are the boundary** of the whole.

## Travel together — the unit of share / transfer

A composition is the atomic unit of **selective sharing and transfer** (this operationalizes
"branch = the unit of share/transfer" in `sccs-entity-system`). When a trunk-composition travels:

- the **connection rows travel by value** — copied/moved to the recipient — **bounded by the parts
  the prototype marks required** (optional parts may be left behind);
- the **target concepts are referenced, not copied** — they are shared, so the same employer/employee
  is not duplicated into the recipient;
- the **reserved security branch never travels** (auth, grant primitives/rules, audit, root-of-trust).

The required-parts boundary is what keeps the bundle coherent: a recipient receives a whole that is
complete by its own prototype, or the transfer is incomplete and is flagged.

## Grammar is a connection under composition

`sccs-grammar-testing` is **a connection beneath composition**, not a peer above it: checking grammar
*is* checking that a composition is well-formed (names via the L1–L15 lint) and **complete against its
prototype** (every required component present, cardinality satisfied). Composition is defined first;
the grammar relation operates within it, validating the whole the composition declares. The
orchestrator therefore composes first, then runs the grammar connection over the result.

## Rooting

A composition roots where its branch directs it — the **branch-directed trunk root** (peel qualifiers
to the chosen generalization), owned by `sccs-ontology` and applied on the connection side by
`sccs-trunk-prototype`. A branch may sit in several cross-cutting compositions by planting trunks at
several roots.

## Authoring procedure

1. Identify the **whole** and whether it is concept-side or connection-side (the latter is primary).
2. Pull its **prototype**; read the ordered components, each one's **obligation** and **cardinality**.
3. Mark the **required set** — this is the boundary that must travel together.
4. For the connection side, fix the **trunk root** the branch directs (sccs-ontology).
5. State **what travels by value** (the connection-composition) vs **by reference** (its targets);
   exclude the security branch.
6. Run the grammar connection (`sccs-grammar-testing`) over the assembled whole; emit only when
   complete by its prototype and the naming lint passes.

## Required output format

Return the standard SCCS triple — a **Concepts** table (`local_ref`, `role`, `category`, `type`,
`referent` ∈ NULL/0/self/target, `notes`), a **Connections** table (`local_ref`, `of`, `type`
generated-name, `order`, `to`, `kind` = component/possession/classification, `notes`), and a 2–4
sentence **Summary** naming the composition, its prototype, the required-parts boundary, and what
travels by value vs by reference. Use composite `(id, user_id)`; keep names self-documenting.
