# SCCS-INTEGRATION — the bundle map

Shared reference for the skills that defer cross-skill coordination to this file
(`sccs-composition`, `sccs-trunk-prototype`). Assembled from the project's authoritative sources:
the orchestrator's coordination role, the inter-skill dependency graph, and the global terminology
reconciliations (the L1–L15 lint's retired-token table and the SNS "retire `slot`" directive).

> **Precedence.** The original 2018 SCCS engine is **authoritative**; this is a non-destructive
> overlay. Where this map and the engine differ, the engine governs. Always say **concept**, never
> "node." Field-level detail lives in `references/sccs-spec.md`.

---

## 1. How the skills coordinate

`sccs-orchestrator` is the entry point for any multi-step SCCS task. It routes to the skill that owns
each sub-task, sequences them, and runs **one shared validation pass** before anything is emitted:

1. **Naming lint (hard gate)** — every name re-derived from the rules and checked against **L1–L15**,
   including the retired-token table in §3. Owned by `sccs-naming`; never carry a prior artifact's
   token forward unchecked.
2. **Reading normalization** — determiner, number, subject–verb agreement, **one consistent term per
   concept**.
3. **Completeness** — each component connection checked against its **obligation**
   (required · requested · conditional · prohibited) and **cardinality** (min/max), per the prototype.
4. **Render** — the prototype's five stages (`order` · `visibility` · `punctuation-before` ·
   `punctuation-after` · `format`).

   Realized as `the grammar program entity`; owned by `sccs-grammar-testing`.

> **Non-destruction rule.** The gate may **silently** fix anything that changes only grammar (an
> article, a plural, agreement, drift to a non-canonical synonym). It must **flag, never rewrite**,
> anything that changes **which relation a name asserts** — part ↔ possession, a different head noun,
> a changed obligation. When unsure, flag.

---

## 2. The skill graph (who depends on whom)

Foundations are read first; domain skills compose them; the orchestrator and grammar gate wrap every
task. Edges below are "reads / defers to."

| Skill | Role | Depends on |
|---|---|---|
| **sccs-orchestrator** | route · sequence · validate | all skills below |
| **sccs-engine-core** | ground truth: the live two-table engine as it describes itself (self-describing schema, `category⟨type⟩`, `^`, `^the_name=`, leaf set, `(id,user_id)`) | consulted first by orchestrator; anchors naming, ontology, instances, prototype |
| **sccs-grammar-testing** | the validation gate (L1–L15 + completeness) | naming, prototype, trunk-prototype, entity-system |
| **sccs-naming** | the Semantic Naming System (SNS) | ontology, types, instances, noun-creation |
| **sccs-ontology** | graph mechanics: `the_concepts` / `the_connections`, branch/trunk/target | types, instances, prototype, trunk-prototype, entity-system, units-of-measure |
| **sccs-prototype** | concept-side prototype (resolve · render · obligation · cardinality) | naming, ontology, units-of-measure |
| **sccs-trunk-prototype** | connection-side prototype; kind→reads-as | prototype, ontology, naming, grammar-testing |
| **sccs-types** | typological concepts (referent NULL) | ontology, instances, noun-creation, prototype, entity-system |
| **sccs-instances** | referents (value / 0 / target) | ontology, types, prototype, entity-system |
| **sccs-noun-creation** | head-noun creation & classification | naming, types |
| **sccs-hypernym-placement** | grafting a noun into the live hypernymy | naming, noun-creation, types |
| **sccs-composition** | whole-from-ordered-components; the share/transfer unit | prototype, trunk-prototype, ontology, entity-system, grammar-testing |
| **sccs-application** | the `of` application connector | prototype, trunk-prototype, naming, grammar-testing |
| **sccs-entity-system** | actors, roles, access control | ontology, types, instances |
| **sccs-units-of-measure** | dimensions, canonical base, conversion | instances, types, ontology |
| **sccs-event** | reified events/actions (actor · participants · time) | composition, entity-system, units-of-measure, commerce, naming, ontology |
| **sccs-commerce** | offers, price-as-role, orders, payments, inventory | composition, entity-system, event, units-of-measure, postal-address |
| **sccs-postal-address** | country-parametric addresses | composition, entity-system, instances, types, prototype, trunk-prototype, units-of-measure |

`references/sccs-spec.md` is the shared field-level data dictionary (carried by the skills that need
it); `references/inheritance-and-rooting.md` is the connection-prototype inheritance/rooting detail
(carried by `sccs-ontology` and `sccs-trunk-prototype`).

---

## 3. Global terminology reconciliations

One canonical term per relation, across **every** skill, type, connection, table, field, identifier,
comment, and prose. Re-derive names from the relation (who–relation–what); never emit a retired token.

**Retired tokens (lint L8) — never emit; use the SNS term for the relation:**

| retired | use instead |
|---|---|
| **slot** | **the component connection** / **the connection of the `<concept/type>`** / **order** (for an ordered position). **Exception: `the time slot`** — a window on the time dimension, modeled as a `the time` composition (a start position + a duration). |
| **facet** | the **label** and **format** semantic concepts (the rendering functions) |
| **property** | **the component connection** (the named connection of the concept) |
| **container** | **the composite** |
| `the_place_place` | a **`containment`** |
| `branch_concept_concept_id` | `the_branch_concept_id` |

**The `slot` directive (strengthens L8).** `slot` names furniture ("a place where something goes"),
not a relation, so it hides whether the concept *owns*, *contains*, or *classifies* the part. Replace
with the precise relation: a part of a composition → **the component connection**; the thing the part
points at → **the component**; an ordered position → **order** (+ a position branch); a relation in
general → name it by its kind (possession `_s_` · classification `_` · component / containment /
division). The single sanctioned use is **`the time slot`**.

**Other standing reconciliations.**
- Say **concept**, never "node."
- Containment is **not** possession: name the `containment` kind, don't dress it as `_s_`.
- Money/measured values are **magnitude + unit** (R1), never a bare number; currency is a dimension.
- A reflexive edge (`of == to`) is licensed **only** at a regress-terminating apex (L10); a doubled
  head like `the_email_email` is not — name the value-leaf binding by what it resolves.

---

*This map is the overlay's current-understanding summary of how the SCCS skills fit together. If a
canonical bundle map exists elsewhere in the workspace, prefer it and replace this file.*
