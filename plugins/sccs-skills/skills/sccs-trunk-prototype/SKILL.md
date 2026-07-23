---
name: sccs-trunk-prototype
description: The connection-side prototype: the prototype of the connection and of the trunk, branch/trunk/target, kind→reads-as, branch-directed trunk roots. Use for how a relation resolves and renders.
---

# SCCS — The Connection Prototype (and its trunk node)

> **SCCS bundle map:** cross-skill coordination, the skill graph, and the global terminology reconciliations live in [references/SCCS-INTEGRATION.md](references/SCCS-INTEGRATION.md) — read it when coordinating across SCCS skills.

**Precedence:** the original engine is authoritative; this is a non-destructive overlay.

SCCS has two core tables, so two master prototypes. `the prototype of the concept` (sccs-prototype)
governs `the_concepts` — where a *value* lives. **`the prototype of the connection`** governs
`the_connections` — how a *relation* resolves and renders. Beneath it sits the trunk node `the
prototype of the trunk`, the point branches
inherit from. Read sccs-prototype first; this is the connection-side dual and inherits the shared
methodology.

## The dual master

```
the prototype of the prototype   → the self-applied root
  the prototype of the concept     → the_concepts — the genesis; the connection master derives from it
  the prototype of the connection  → the_connections
```

> **The two endpoint connectors (`of` = branch, `to` = target).** A connection's two endpoints are the
> polysemous English prepositions narrowed to one sense each: **`of` = branch-root** (the source the
> relation roots in — the objective-genitive/argument sense of "of", reused as `of_the_X`), and
> **`to` = target** (the destination — the goal/dative sense of "to", the second endpoint of a binary
> connection only). All other senses route elsewhere (possession → `_s_`, recipient → recipient role,
> purpose → purpose, comparison → ratio). A unary head (application/composition) has only `of`. See
> `../sccs-naming/references/connector-senses.md` and lint L15.

Each of these is formed with the application connector **`of`** (see **sccs-application**): the
prototype applied to the prototype, to the concept, to the connection. Each line decomposes
`category = the prototype of`, `type = the <advanced type>` (the argument is a **complete type**, not
a bare word like "concept"); `the prototype of the prototype`'s `type` component points back at `the
prototype` itself, so the self-reference is **structural**. A prototype is a **type** (`referent NULL`) — compositional, defined by its render-rule components —
that derives from `the prototype of the concept` (the
genesis) and inherits evaluation, obligation, and cardinality. It self-terminates because "any
connection" includes the connection operator itself.

## Resolution components — branch · trunk · target · kind

| component | refers to | role |
|---|---|---|
| `the_prototype_branch` | `the branch` | the elaborating / `of` side |
| `the_prototype_trunk` | `the trunk` | the trunk-overlay |
| `the_prototype_target` | `the target` | the endpoint (the `to`) |
| `the_prototype_kind` | `the kind` | part / possession / classification |

The branch/trunk/target model is owned by sccs-ontology; this prototype says how those parts
**resolve and render**.

## Kind feeds format

`the_prototype_kind` (part / possession / classification) is read by the inherited **format** to pick
the reads-as grammar — possession → "the X's Y", part → "the X Y", classification → "the X is a Y".
A connection's reading is computed, never stored.

## Inheritance & rooting

Default chain, each component defined once at its most-general node:
`the prototype of the concept` (genesis) → `the prototype of the connection` → `the prototype of the
connection trunk` → `^the prototype of <a branch>`. The root is **not** fixed, though: a branch may
create a trunk at its **own appropriate root** by peeling qualifiers from its name (e.g. `the red
toyota model A car` → a trunk at `the model A car`, ranging over all model A cars of any make). Full
hierarchy, the populate-upward rule, and rooting examples: **references/inheritance-and-rooting.md**.

## The grammar gate

`sccs-grammar-testing` is incorporated **only at the branch** (the `of` side); it resolves upward via
the trunk to the root, so one attachment validates the whole chain.

## Authoring

1. Ensure the root exists (sccs-prototype).
2. `the prototype of the connection` (`referent NULL` — a type) → refers up to the root; holds what is true of
   any connection.
3. `the prototype of the trunk` → refers up to it; holds the trunk-overlay mechanics.
4. A branch prototype → refers up to its appropriate trunk root; carries only its overrides.
5. Hoist each component to its single most-general node; attach the gate at the branch; set
   obligation/cardinality at each defining node; emit only after the naming lint passes.

## Required output

The standard SCCS triple — **Concepts** (`local_ref`, `role`, `category`, `type`, `referent`,
`notes`), **Connections** (`local_ref`, `of`, `type`, `order`, `to`, `kind`, `notes`), and a 2–4
sentence **Summary**. Use composite `(id, user_id)`; keep names self-documenting.
