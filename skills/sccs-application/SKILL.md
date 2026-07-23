---
name: sccs-application
description: The SNS `of` application connector: `the F of the X` applies a function-concept to an argument (e.g. the prototype of the concept). Use for `of` names, derived concepts, or function application.
---

# SCCS — The Application Connector (`of`)

**Precedence:** the original engine is authoritative; this is a non-destructive overlay on the
unchanged schema. This skill owns one SNS connector — **`of`** — and the grammar that makes it
well-formed. The lexicon and the L1–L15 lint live in `sccs-naming`; the prototype mechanics that *use*
`of` live in `sccs-prototype` / `sccs-trunk-prototype`; validation is gated by
`sccs-grammar-testing`.

## `of` is function application

SNS has four connectors. `the` roots a name; `_s_` marks **possession** ("the X's Y"); `_` marks a
**part** ("the X Y"); **`of`** marks **application** — it applies one concept's *functions* to
another. `the F of the X` reads "take the function-concept **F** (a contract — a set of available
functions) and **apply it to X**," producing a derived concept in which F's functions are now
available to X.

This is exactly what binds the prototype to what it governs: `the prototype of the concept` is the
prototype contract *applied to* concepts; `the prototype of the connection` applies it to
connections; `the prototype of the prototype` applies it to itself (the self-describing root).

## The grammar: `the F of the X`

- **Head/function FIRST, argument LAST.** Unlike possession (`the_x_s_y`, where the head noun Y is
  last and the possessor X is first), application puts the **function-concept first** and the
  **argument last**. `the prototype of the concept` — `prototype` (the function) governs;
  `the concept` (the argument) is what it is applied to.
- **Both operands keep `the`.** Token form: `the_F_of_the_X` (e.g.
  `the_prototype_of_the_concept`). Each operand is itself a determinative concept.
- **Reads back as English** (the L9 reads-as test): "the prototype of the concept" is a grammatical
  clause. The result is a **derived concept** — F bound to X.
- **Nesting is application of an application.** `the prototype of [the prototype of the prototype]`
  renders `the prototype of the prototype of the prototype` — valid, because each `of` has a real
  target. The self-application chain is `the prototype → the prototype of the prototype → … ∞`.

## The category/type decomposition (where the operands land)

A prototype enhances an **existing type**, and an existing type already **begins with `the`** — the
bare words (`prototype`, `concept`, `connection`) are just words until `the` advances each into a
type. `of` therefore applies one *advanced* type's functions to another *advanced* type, and the
operands land like this:

- The argument **`the X` is a complete, already-advanced type** → it occupies the **`type`** component
  whole.
- The whole function-phrase **`the <F> of`** is the **`category`** → it climbs
  `the <F> of` → `the <F>` → `the`.

So `the prototype of the concept` decomposes:

```
category = the prototype of        type = the concept        referent = NULL
```

— **never** `category = the`, `type = concept`. (Splitting `the` off as the category and leaving the
bare word `concept` as the type is the classic error: it demotes a complete type back to a word.)

Two consequences:

- **The no-dangling-`of` rule becomes structural, not conventional.** `the <F> of` is only ever an
  intermediate **category** that resolves up to `the`; it is never an emitted standalone name. By
  construction you cannot write a trailing `of`, so the rule below can't be violated rather than
  merely shouldn't be.
- **Self-application is visible in the schema, not just the wiring.** `the prototype of the
  prototype` decomposes `category = the prototype of`, `type = the prototype` — its **`type` component
  points back at `the prototype` itself**. The self-reference is readable straight off the
  category+type pair, not only off the `of`→`to` connection.

## The no-dangling-`of` rule

A name must **never end in `of`**. `of` is an operator that *requires* a target; a trailing `of` has
nothing to apply to and fails the reads-as test.

- ✗ `the prototype of the prototype of` — dangling; the second `of` applies to nothing.
- ✓ `the prototype of the prototype` — the operator applied to itself, complete.
- ✗ `the prototype of the trunk of` → ✓ `the prototype of the trunk`.

This rule is incorporated into the naming lint (extends L9 / the reads-as test) and is checked by
`sccs-grammar-testing`.

## `of` vs possession vs part — disambiguation

| You mean… | Connector | Form | Reads |
|---|---|---|---|
| apply F's functions to X (a derived concept) | **`of`** | `the F of the X` | "the F of the X" |
| X has / owns Y | `_s_` | `the_x_s_y` | "the X's Y" |
| Y is a part of X | `_` | `the_x_y` | "the X Y" |

Test: does the left concept **provide functions made available to** the right? → `of`. Does the left
**own** the right? → `_s_`. Is the right a **constituent of** the left? → `_`. Picking the wrong one
changes the meaning, so the grammar gate **flags** a mis-pick rather than silently rewriting.

## What it produces — a derived concept

The result of `the F of the X` is a concept whose `referent` follows the normal rule:

- The **prototype applications** (`the prototype of the concept/connection/composition/trunk`, and
  `the prototype of the prototype`) are **types** — `referent NULL` (a prototype is a type).
- A **derived-on-read** value (`the price of the order`, `the capital of the country`) is evaluated
  through the relevant prototype on read — a derived/view concept, not a stored copy.

So `of` is the bridge between a function-concept and its argument; the prototype then says *how* the
derived concept resolves and renders.

## Canonical applications

- The prototype family: `the prototype of the concept`, `the prototype of the connection`,
  `the prototype of the composition`, `the prototype of the trunk`, `the prototype of the prototype`.
- General derived concepts: `the price of X`, `the colour of X`, `the capital of X`,
  `the length of X` — each a function-concept applied to an argument.

## Authoring procedure

1. Identify the **function-concept** F (the contract that provides functions) and the **argument** X
   (what F is applied to).
2. Confirm the relation is **application** — F's functions become available to X — not possession or
   part (use the disambiguation table).
3. Assemble **`the F of the X`** (function first, argument last; both keep `the`); never leave a
   trailing `of`. Land the operands by the **category/type decomposition**: the argument `the X` is a
   complete type → the **`type`** component; `the <F> of` is the **`category`** (climbing to `the`).
4. Set the derived concept's **referent** by the normal rule (prototype applications are types,
   `NULL`; derived-on-read values resolve through their prototype).
5. Validate with `sccs-grammar-testing`: the no-dangling-`of` rule, the reads-as test, and the
   one-consistent-term rule.

## `of` is one sense of English "of" — pick it deliberately (see `../sccs-naming/references/connector-senses.md`)

English **"of"** is the second-most-frequent word in the language and is massively polysemous
(≈18 contemporary senses, dozens historically). SCCS `of` is exactly **one** of them — the
**objective-genitive / argument / branch-root** sense: the complement is the argument the head roots
in (`the prototype of the concept` = the prototype rooted in the concept). Every other "of" sense
routes to a **different** connector — possession → `_s_`; partitive/material → composition; subjective
genitive (agent) → an event role; origin → source/`from`; quality → characterization; apposition →
identity; reference → about; measure → quantity; separation → without. Never let a bare ambiguous "of"
stand as a connector (lint **L15**).

**Storage vs reading (a duality, not a conflict).** The engine stores application as
**`head ⟨ of_the_X ⟩`** with a **reused** `of_the_X` branch-root row (the same `of_the_concept`, id 14,
serves `the_connection`, `the_composition`, and `the_prototype`). The SNS **reads** it as "the F of the
X". Both keep the argument `the X` intact, so both respect L11; they are two views of one row.

## Required output format

Return the standard SCCS triple — a **Concepts** table (`local_ref`, `role`, `category`, `type`,
`referent`, `notes`), a **Connections** table (`local_ref`, `of`, `type` generated-name, `order`,
`to`, `kind`, `notes`), and a 2–4 sentence **Summary** naming the function-concept, the argument, the
derived concept produced, and its referent state. Keep every name self-documenting and `of`-clean.
