---
name: sccs-prototype
description: Define the concept and its PROTOTYPE — the contract (resolve, 5 render stages, obligation, cardinality) turning a raw referent into readable output. Use for what a concept does or how it renders.
---
> **Terminology guard.** SCCS **prototype** = the **qualia / function contract** (Pustejovsky's Generative Lexicon: formal · constitutive · telic · agentive). It is **not** the cognitive-science **prototype** (Rosch: graded category membership / best exemplar). Never import graded-membership semantics from the word.


# SCCS — The Concept and Its Prototype

Start here. Before a type is named or an instance is filled, settle **what a concept is** and **what
its prototype makes available** — because every higher capability in the system is built out of the
prototype's functions.

## The concept

A concept is **category + type + referent** (each carrying its `user_id`). The **referent** is raw:
`NULL` (a **type** — a compositional schema; a prototype is a type, so **every prototype is `NULL`**),
`0` (a compositional **instance**), a small **integer** inline, or a **pointer** into a leaf
table. Raw storage, by itself, *does nothing* — it is ids pointing at ids and values sitting in
tables.

The prototype is **bound to what it governs by the application connector `of`** — `the prototype of
the concept` / `…connection` / `…composition` / `…trunk`, and the self-applied `the prototype of the
prototype` (see **sccs-application**). Each decomposes `category = the prototype of`,
`type = the <advanced type>`, `referent = NULL` — the argument is an **already-complete `the X`
type** in the `type` component, never a bare word; the self-applied root's `type` component is `the prototype`
itself.

## The prototype is the function layer

What makes a concept **do** anything is its **prototype** — carried on the **category** concept and
inheriting down the chain. The prototype answers exactly one question, and it is the organizing
question of the whole system:

> **What functions must be available so that, from a concept's raw referent, ALL upward
> functionality can be produced?**

Everything above the referent — a readable string, a composed card, a secured field, a derived
count, a converted amount, a shareable branch — must **reduce to a fixed set of prototype
functions**. If those functions exist for every concept, nothing higher up ever needs a new
primitive. That is the prototype's promise: it is the **complete function contract** for upward
functionality.

## The function contract (the complete set, bottom-up)

The prototype guarantees these functions, in tiers from the referent upward:

**Tier 0 — Resolution (get the value)**
- `resolve` — follow the referent rule via **location-statements** (host → schema → table → field):
  inline integer → the value itself; leaf pointer → read the leaf; compositional (`0`) → gather the
  connections. For an **indexical concept** (`the current user`, `now`, `here`), resolve reads the
  **deictic center** (origo: `user_id` / clock / tenant) at read time rather than a stored referent —
  see **sccs-deixis**.
- `read` — interpret the leaf value in its **leaf-table sense** (number, text, date, hash, ipv4, …).

> **Resolution preserves the type/instance axis — the generalized prototype is non-instantiating.**
> The prototype of any **type** concept **resolves to a type** (referent stays **NULL**); it does
> **not** instantiate itself. Resolving a type through the generalized prototype computes/renders the
> type's contract and yields a **type**, never a referent-`0`/bound **instance**. Instantiation is a
> separate act (see **sccs-instances**), performed only by a **special, overriding prototype** — and
> such a special prototype is an **update to (an extension/override of) the generalized prototype**,
> not the default behaviour. So `the prototype of the prototype` resolves to a type (the prototype is
> a type); it self-terminates without self-instantiating.

> **Engine grounding (sccs-engine-core).** Rendering is the assembly operator `category⟨type⟩` read
> left-to-right by the engine's **`evaluate()`**; the same resolve-and-compose path renders markup,
> since the engine models its **HTML tags/attributes as concepts** too. There is no separate render
> engine — output is concepts resolved through their prototypes.

**Tier 1 — Rendering (the five stages — make one element human-readable)**
- `order` (stage 1) — position the element in the output.
- `visibility` (stage 2) — visible / hidden / restricted / conditional. *This is also the security gate.*
- `punctuate-before` (stage 3) and `punctuate-after` (stage 4) — affixes.
- `format` (stage 5) — the pattern: grouping, decimals, masks, locale.

**Tier 2 — Connection (read the relation as language)**
- `connect` — apply the concept's **linguistic connector** (verb / preposition / genitive) and its
  **valence**, so the element reads inside an English clause rather than as a bare edge.

**Tier 3 — Composition (assemble wholes from parts)**
- `compose` — render each child connection through **its own** prototype, in order, into the parent
  output.
- `inherit` — use the **category** prototype unless the type overrides it (category has inheritance
  priority over type).

**Tier 4 — Derivation & conversion (computed / transformed values)**
- `derive` — compute on read (counts, views, smart collections) instead of reading a stored value.
- `convert` — transform a measured magnitude across **units** (a constant factor; a **live rate** for
  currency).

## Why this set is complete

Every higher behavior is a *composition* of these primitives — none introduces a new one:

- a **formatted price** = resolve + format + punctuate (on the currency unit's prototype);
- a **masked identifier** = visibility(restricted) + format(mask), gated by a `see` grant;
- a **moment card** = compose (date + location + content + tags, each via its prototype);
- a **derived collection / view** = derive;
- a **converted amount** = convert;
- a **readable relation** = connect;
- a **shared or transferred branch** = resolve + compose over a branch subtree.

If some required output does *not* reduce to these functions, the gap is **in the prototype**, not
above it — that is the diagnostic this skill exists to apply.

## How the prototype is modeled (concepts + connections)

The prototype is itself concepts. `the prototype` is an **ordered composition** of
`the render rule`s; each rule targets one element (a connection part or the concept's own leaf) and
supplies stages 2–5 — the rule's **order** in the composition *is* stage 1. The wiring:
`the_concept_s_prototype`; `the_prototype_s_rule` (ordered); `the_render_rule_s_target`,
`_s_visibility`, `_s_punctuation_before`, `_s_punctuation_after`, `_s_format`. Resolution
(location-statements) and conversion (units) hang off the same prototype.

## Properties

- **Inherits** down the category chain — override only where a subtype resolves or renders
  differently (a company moment vs a personal memory).
- **Composes** — a parent renders its children through their prototypes; the moment card is the
  canonical case.
- **Secures** — `visibility = restricted` masks a sensitive field (an identifier → `•••-••-1234`,
  surfaced only under a `see` grant). Security *is* the visibility function, evaluated against the
  viewer's grants at render time.
- **Derives** — counts and views render derived, never stored.

## Procedure

1. Take the concept (category + type + referent); name its referent state.
2. Give its category a prototype (or inherit one); state **resolution** — where the referent lives.
3. Walk the function tiers in order — resolution → the five render stages → connector → composition
   → derivation/conversion — declaring each that applies.
4. For a composite, list child render rules **in order**, each deferring to the child's prototype.
5. **Closure check:** confirm every upward output the concept must produce reduces to these
   functions. If one doesn't, fix the prototype.

## Required output

A **prototype table** per concept (`element · order · visibility · before · after · format`), plus
its **resolution** (location) and **conversion** (unit) where relevant, and a one-line note of which
upward outputs it produces. Defer naming to `sccs-naming`, conversion specifics to
`sccs-units-of-measure`, and graph wiring to `sccs-ontology`.
