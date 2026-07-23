---
name: sccs-types
description: Create typological concepts (types, referent NULL) in SCCS: 'The X' roots, qualifier+base subtypes, the category prototype. Use to define the kinds/schema in a domain before instantiating.
---

# SCCS — Creating Types

A **type** (typological concept) is a recipe: a "kind of thing" with nothing bound yet — `referent
NULL`. A type is **compositional by definition**: its meaning *is* its set of component connections
(its schema). Note this is the type/instance axis, not a compositionality axis — a type (NULL) is a
compositional *schema*; a `referent 0` concept is a compositional *instance*. A type's **category
prototype is itself a type** (`referent NULL`), never a `referent 0` instance. In the
geometry mnemonic it is a *right triangle* — `category` + `type` only, half a shape. The
completing third vertex (the `referent`) is what an *instance* adds later.

Always say **concept**, never "node." Exact fields and the worked example: `references/sccs-spec.md`.

## The one rule that defines a type

Set `referent_id` = **NULL**. A concept is identified by **category + type + referent**, stored as
`category_id`, `type_id`, `referent_id` (each with a `_user_id`). For a type:

- `category_id` → a **qualifier** concept (modifiers left of the head noun); has inheritance
  priority over the type.
- `type_id` → a **base** concept (the head noun).
- `referent_id` → **NULL**.

## A type is inert until its category prototype binds it

A type is a **grouping of two or more concepts** — and the grouping alone says nothing about where
its instances' data lives or how to read it. The **category concept carries a prototype** that
supplies exactly that (its full **function contract** — resolve, render, connect, compose, inherit,
derive, convert — is in **sccs-prototype**):

- **Location-statements:** `host → schema → table → data-field → instance-id-references-field`
  (+ access credentials) — *where to look for the referent*.
- **5-stage evaluation:** order, visibility, punctuation-before, punctuation-after, format —
  *how to express it*.

The prototype **inherits down the category chain** to the root **creation-string default** (the
most general type, the default location/format). Defining a type therefore means defining both its
name *and* its prototype; without the prototype the type cannot resolve a referent.

## `the` is the root

`the` is the single root concept and the determinative carrier of authority. Every type is a
"The X" construction descending from it; the schema tables themselves are "The X" concepts made
real. A type's `category_id` chain climbs back to `the`.

## Build a simple type

```
category = The big      (qualifier concept)
type     = cat          (base concept)
referent = NULL
⇒ the type  "The big cat"
```

Draw the **head noun** from the noun-hypernymy vocabulary (being/thing → place, object, idea,
quantity, dimension, measure, position, …) — the controlled set of "kinds" — so the name is precise.
**Create or vet the head noun via `sccs-noun-creation`** (placement + classification + properties)
before assembling the type around it.

## Build a richer type: left-to-right assembly

Names assemble **left → right**; each new token becomes the new `type_id`, everything to its left
becomes the `category_id`; the head noun is appended **last**.

```
the yellow              → the yellow              (category=the,             type=yellow)
the yellow + green      → the yellow green        (category=the yellow,      type=green)
the yellow green + dragon → the yellow green dragon (category=the yellow green, type=dragon)
```

The rightmost adjective binds tightest; the head noun is *what the thing is*. This matters because
**instances run the opposite way** — they anchor on the head noun and generalize by peeling the
leftmost modifier (see sccs-instances). `category_id` carries this name assembly; `referent_id`
(added by instances) carries the head-anchored subsumption.

The **head noun is the concept's kind** (its sortal — its identity-conferring sense, drawn from the
noun-hypernymy). A restrictive qualifier yields a **sub-kind** only when it survives the sortal test
*"is the result still a kind of the head?"* (`the cellular phone` is a kind of `the phone` ✓). A
qualifier that names a **role, phase, or quality** fails that test — it is **characterization**, not
a sub-kind, and belongs on an instance (describing qualifier) or an entity role-branch, never baked
into the type (sccs-naming L12).

**Proper names are not part of the type-name.** "United States", "John Doe" attach later as a
possession / `character_value` on an instance — never baked into the type.

## Type-level connections are the schema

A type's connections define its **component connections** (its schema). Name connection types by the **relation**, never by
concatenating endpoints:

```
classification (part):     <from_type> "_"   <to_type>   # "is a / is classified as"  e.g. the_entity_type
possession (elucidative):  <from_type> "_s_" <to_type>   # "has / is described by"    e.g. the_entity_s_address
```

For genuine structural belonging/containment, name the **kind** concept (`containment`,
`division`), not a doubled noun — `the_place_place` is wrong; a place's parent is a structural
componency of kind `containment`. (Role/kind specializations like student or employee are **branches
of the entity**, not these structural kinds — see sccs-entity-system.) Defining these on the
typological concept *is* defining the schema; instances later fill them.

## Authoring procedure

1. Identify **root types** as "The X", `referent_id` NULL; pick head nouns from the hypernymy set.
2. **Compose richer types**: qualifier-category + base-type, assembled left → right, head last.
3. Verify each `category_id` chain climbs cleanly to `the`.
4. Give each type its **category prototype** (location-statements + read-method); confirm it
   inherits to the creation-string default.
5. Define **type-level connections** (the component connections), **naming the relation** (classification/kind or
   possession).
6. Keep names self-documenting; carry composite `(id, user_id)`; inline generated names into
   `character_value` where text-filtering should avoid a join.

## Required output format

Return a **Concepts** table (`local_ref`, `role` = `type`, `category`, `type`, `referent` = NULL,
`notes` — note the prototype/location where relevant); if you defined schema, a **Connections**
table (`local_ref`, `of`, `type` generated-name, `order`, `to`, `kind`, `notes`); and a 2–4
sentence **Summary** naming the root types and the domain. Use composite `(id, user_id)`; state
assumptions inline.
