---
name: sccs-construction
description: Model constructions — learned form-meaning pairings on the schematic-to-substantive continuum — with open positions that bind typed fillers, meaning carried by the pattern (not its fillers), placed in a construction inheritance network. Use for productive patterns, argument-structure constructions, partially-filled templates ("the X-er the Y-er"), or pattern-level meaning.
---

# SCCS — Construction Grammar (the construction concept)

**Precedence:** the original engine is authoritative; this is a non-destructive overlay. A construction
is assembled from machinery SCCS already has — it adds no columns.

A **construction** is a learned **form ↔ meaning pairing** treated as a first-class concept.
Construction Grammar's claim (Fillmore · Goldberg · Croft): *everything from a morpheme to an abstract
argument-structure pattern is a construction*, arranged on **one continuum from substantive (fixed) to
schematic (open)**. SCCS already supplies every piece — the **composition** (the form), the
**prototype** (the meaning), **valence** (the open positions), and **subsumption** (the network) — so a
construction is built, not bolted on.

## A construction = form + meaning, as one concept

- **the form** — an **ordered composition** (a combination, sccs-naming) of component **positions**;
  each position is either **fixed** (a specific concept) or **open** (a typed variable that binds any
  filler of a kind).
- **the meaning** — carried by the construction's **prototype** (sccs-prototype): the pattern means
  something **independent of its fillers**. (Goldberg: "she sneezed the napkin off the table" — the
  caused-motion construction supplies the motion, not the verb "sneeze".)
- the construction is a **type** (`referent NULL` — a schema); a concrete utterance that fills it is an
  **instance**.

## The schematic ↔ substantive continuum

How **open** the positions are is the only variable along it:

- fully **substantive** (every position fixed) → a frozen expression → hand off to **sccs-phraseology**.
- **partially filled** (some fixed, some open) → a template, e.g. *"the X-er the Y-er"* (the more the
  merrier): the fixed parts are concepts, the open parts are typed variables.
- fully **schematic** (all positions open) → an abstract pattern, e.g. the ditransitive *"the agent
  verbs the recipient the theme"*.

## Open positions = typed argument positions (reuse valence)

A construction's open positions **are** the **valence** of its connectors (sccs-naming): each declares
which **noun-role** it binds (agent / recipient / theme / location). An **argument-structure
construction** = a connector frame (the roles) + a meaning (the prototype). The ditransitive declares
three positions (agent, recipient, theme) and the meaning "X causes Y to receive Z".

## Meaning lives on the pattern (the override rule)

When a filler and the construction disagree, the **construction's prototype augments/overrides** the
fillers — the **non-compositional residue is stored on the construction**, computed by `compose` /
`derive` over the bound fillers. A bare edge cannot do this; the construction can, because its meaning
is its own prototype. This is the productive counterpart to a stored idiom (sccs-phraseology).

## The construction network (inheritance)

Constructions form an **inheritance hierarchy** — a specific construction **is a kind of** a more
schematic one (**subsumption**; the kind tree, sccs-hypernym-placement). It inherits the parent's
positions and meaning through the prototype's `inherit` (the category chain) and adds only its own. The
ditransitive inherits from a transitive argument-structure construction. **Reuse before create; graft,
don't duplicate.**

## Construction lint

- **CX1** — pattern meaning attributed only to the fillers → put the non-compositional residue on the
  construction's prototype.
- **CX2** — an open position with no declared binding type/role → declare its filler-kind and valence
  role.
- **CX3** — a **productive** pattern modeled as a one-off instance → if it licenses many fillings,
  model it as a **schematic construction** (a type), not a single instance.
- **CX4** — a construction left outside the network → graft it under its more-schematic parent
  (subsumption).
- **CX5** — a **fully-fixed** construction modeled here → it is a frozen expression; hand off to
  **sccs-phraseology** (the continuum boundary).

## Procedure

1. Identify the **form** (the ordered positions) and which are **fixed** vs **open** (typed variables).
2. Place it on the continuum (substantive → schematic); if fully fixed, route to **sccs-phraseology**.
3. Give the construction a **prototype** carrying the **pattern-level meaning** (the residue not in the
   fillers).
4. Declare each open position's **filler-kind + valence role** (the argument structure).
5. **Graft** it into the construction network under its more-schematic parent (subsumption); run the
   construction lint.

## Required output

The standard SCCS triple — **Concepts**, **Connections** (with `kind`), and a 2–4 sentence **Summary**
naming the construction, its open positions (filler-kind + role), its pattern-level meaning, and its
parent in the network. Composite `(id, user_id)`; names self-documenting.
