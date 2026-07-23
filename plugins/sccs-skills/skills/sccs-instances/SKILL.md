---
name: sccs-instances
description: Create instances in SCCS (referent = value / 0 / target): referring, compositional, hybrid; referent chains; leaf resolution. Use to instantiate a type or bind a concrete referent.
---

# SCCS — Creating Instances

An **instance** binds a type to something concrete. In the geometry mnemonic it is the
*equilateral triangle* — `category` + `type` + `referent`. This is the SNS (sccs-naming) applied to the instance side: instances are named head-noun-last and run specific→general, the mirror of how types are built. A type is half a shape (referent NULL);
an instance completes it.

Always say **concept**, never "node." Exact fields, the decode/evaluation note, and the worked
example: `references/sccs-spec.md`.

## The instance shape

```
category_id  = The instance of      # the marker concept that turns a type into an instance
type_id      = <the typological concept being instantiated>
referent_id  = <one of the kinds below>
```

The secure-instance operator is written **`^`** — `^the dragon` = "the secure instance of the dragon."
In the engine `^` is itself a concept, **`the_secure_instance_of`** (id 4); and a name value is stored
as a `^the_name=<token>` instance resolving to `the_characters` — so a token is stored **once** and
read many (see **sccs-engine-core**). Reuse the existing `^the_name=` row; do not mint a synonym.

## The referent rule (decide this first)

| `referent_id` | Kind | Meaning |
|---|---|---|
| **a small integer** | **Inline value** | The integer **is the value itself** — integers need no leaf table. |
| **a row-id / target** | **Referring** | Delegates to another concept, a raw-data leaf record, or inline `character_value`. |
| **0** | **Compositional** | "My content is relational — read the connections fanning out from me." |
| **value + connections** | **Hybrid** | Carries a value *and* has its own defining connections. |

(`referent_id` NULL is a **type**, not an instance. `0` is the compositional sentinel; for an
ordinary scalar type a literal `0` can be a genuine inline value — read intent from context.)

> **Instantiation is a deliberate act, not a side-effect of resolution.** Resolving a type through its
> **generalized prototype yields a type** (referent stays NULL) — it does **not** instantiate. Creating
> an instance (binding a `referent`) happens **here**, and only a **special, overriding prototype**
> (an *update* to the generalized prototype) performs it. So you never get an instance merely by
> resolving a type's prototype; you instantiate explicitly.

**Leaf resolution.** Leaf tables are **extensible** — a value gets its own when it has a distinct
canonical format: `the_numbers` (floats/decimals; integers inline), `the_texts`, `the_characters`,
`the_times`, `the_dates`, `the_UUIDs`, `the_documents`, `the_hashs`, `the_ipv4`, `the_ipv6`, with
routing/communication primitives to follow. Media is **not** its own table — `sounds`, `images`,
and `videos` are **document types** resolved through `the_documents`, the gatekeeper, via specialty
servers (the document row holds the locator; the bytes live on the server). The category prototype's
**resolve + read** functions (see sccs-prototype) are what tell the engine which table/field a given
referent resolves to and how to interpret it.

## Build direction: anchor on the head noun, chain specific → general

Type names grow left → right (head noun last); instances **anchor on the head noun** and
generalize by peeling the **leftmost** modifier.

- Base instance = most general: `^the dragon`.
- `^the green dragon` refers up to `^the dragon`.
- `^the yellow green dragon` refers up to `^the green dragon`.

Each layer adds only its branch's data; the chain bottoms out at a raw value stored once at the
base:

```
^the cellular phone number  →  ^the phone number  →  the_numbers value "1235551212"
```

A referent is decoded by walking the **connections of its type and category concepts** — e.g.
`the phone number`'s connections supply formatting so the digits render as `(123) 555-1212`.

## Prerequisite rule

A specific instance requires its more-general instance to exist **first** — no `^the green dragon`
without `^the dragon`. The engine resolves and creates this generalization chain automatically; so
declare the chain from the head noun outward and let the `referent_id` links run specific → general
→ base value.

## Fill the type's component connections

Type-level connections define the component connections; **instance-level connections fill them** with concrete
value-concepts, using the relation-naming grammar (`_` classification/kind, `_s_` possession). A
compositional instance (referent `0`) is *defined entirely* by these outgoing connections.

## Derived / view instances (stored vs derived)

Some concepts are not **stored** data but a **derivation** — a query/filter/sort computed over other
concepts and evaluated **on read**. Views, "on this day" recaps, smart collections, search results,
and dashboards are of this kind: they present *other* concepts, hold no content of their own, and
must never be mistaken for stored objects (or they get double-counted, mis-edited, or stale-cached).

Model a derived concept as a **compositional instance** (`referent 0`) whose connections describe
the **query**, not data: a `_s_source` (what it draws from), one or more `_s_filter` predicates, an
`_s_order`, and a marker that it is **derived** (e.g. a `derived` flag in the category prototype, so
the prototype's read-method *runs* the query rather than formatting a stored value). Nothing is
duplicated — a smart collection of "favorites" stores the filter, not copies of the moments.

```
^the on-this-day recap   (referent 0, DERIVED)
  the_recap_s_source → ^the moment                         # population it queries
  the_recap_s_filter → "occurred_month_day = today"        (the_texts predicate)
  the_recap_s_order  → "occurred_time desc"
```

Keep the line sharp: **stored** instances own their referent/values; **derived** instances own only
a query and resolve to live results. The test: "if I deleted the sources, does this still have
content?" — if no, it's derived.

An **indexical** (`the current user`, `now`, `today`, `here`, `this`) is a special derived instance
whose `resolve` reads the **deictic center** (the origo: `user_id` / clock / tenant) at read time, not
a stored value — its `filter`/`anchor` points at the context, never a frozen value. Owned by
**sccs-deixis**.

## An instance as a connection target

In the branch/trunk/target connection model (see sccs-ontology), an instance is frequently the
**target** — the endpoint a branch points to via the trunk. Recorded on the unchanged schema, the
target is simply the `to_the_concepts_id` of the ordinary semantic connection, while a parallel
`the trunk`-typed row names the trunk the branch elaborates. Nothing about instance authoring
changes; the instance is just referenced as the `to`.

## Authoring procedure

1. Confirm the **type exists** (use sccs-types) — instances need their typological concept and its
   category prototype.
2. Choose the **referent kind**: inline integer value · referring (row-id/target) · compositional
   (`0`) · hybrid.
3. Set `category_id = The instance of`, `type_id = <the type>`, `referent_id` accordingly.
4. Wire the **referent chain** head-anchored, specific → general → raw value (let the engine
   auto-create intermediates).
5. **Fill instance connections** to populate the component connections; for compositional instances those connections
   *are* the content.
6. Inline values into `character_value` where a lookup should avoid a join; carry composite
   `(id, user_id)`; never hard-delete — terminate via `termination_datetime`.

## Required output format

Return a **Concepts** table (`local_ref`, `role` ∈ {instance-referring, instance-compositional,
instance-hybrid}, `category` = "The instance of", `type`, `referent` = value/0/target, `notes`); a
**Connections** table (`local_ref`, `of`, `type` generated-name, `order`, `to`, `kind`, `notes`);
and a 2–4 sentence **Summary** naming the instances and any referent chains. Keep names
self-documenting; state assumptions inline.
