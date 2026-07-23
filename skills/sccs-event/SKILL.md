---
name: sccs-event
description: Model events/actions as reified concepts: a verb that happens, with an actor, participants, time, and tense/aspect/modality. Use for events, actions, occurrences, or transactions.
---

# SCCS — Events & Actions

**Precedence:** the original engine is authoritative; this is a non-destructive overlay on the
unchanged schema. This skill models a **happening** — a verb made into a concept. It assembles from
pieces other skills own: participant roles ride **connection valence** (`sccs-naming`); time and
tense/aspect/modality come from the **qualifier operator** (`sccs-ontology`) over the **time and modal
dimensions** (`sccs-units-of-measure`); the doer is an **entity** (`sccs-entity-system`); the whole is
a **composition** (`sccs-composition`).

## A state describes; an event happens

SCCS already models *static* facts — possessions ("the order's total"), parts, classifications. An
**event/action** is different: it **occurs at a time** and usually has a **doer**. "The price" is a
state; **"the act of pricing"** is an event with an actor and a moment. Model it as an event whenever
the thing you are capturing is a *verb* — something that happened or was done — not an attribute.

Test: can it be dated and attributed to an actor ("X did Y at T")? → event. Is it a standing
attribute of something ("X's Y")? → a possession/part (not this skill).

## The event is a reified predicate

A verb becomes a **head noun** (`the payment`, `the booking`, `the sale`, `the shipment`, `the
pricing`) placed on the noun hypernymy under an *event/act* hypernym. The **type** is `referent NULL`;
a specific occurrence is a **compositional instance** (`referent 0`) whose value is its participant +
time connections.

## Participants — roles via connection valence

Each participant is a connection off the event, named by its **role** (the valence the predicate
takes). Use possession/part per the relation:

- **agent** (who acts) → `the_<event>_s_agent` → an **entity** (individual/composite/program).
- **object / patient** (what is acted on) → `the_<event>_s_object` → the affected concept.
- **recipient / beneficiary**, **instrument**, **source/destination** → the same pattern, one
  connection each, named for its role.

Example — *the payment*: `the_payment_s_agent → ^the buyer entity`, `the_payment_s_recipient → ^the
seller entity`, `the_payment_s_amount → the monetary amount` (see `sccs-units-of-measure` /
`sccs-commerce`).

## When — time, tense, aspect

- **The clock time / date** the event occurs is a possession to a leaf:
  `the_<event>_s_time → the_times` / `the_<event>_s_date → the_dates` (a measured instant; durations
  are measured quantities).
- **Tense & aspect** (did happen / is happening / will happen; ongoing vs completed) are the
  **qualifier operator on the reified connection** — a `(time-dimension, value)` on the event row, not
  a separate field. Owned by `sccs-ontology`; the time dimension is `sccs-units-of-measure`.

## With what force — modality

Whether the event is asserted, permitted/obligated, or possible is **modality**, resolved to the
three modal dimensions — **epistemic** (known/believed), **deontic** (permitted/obliged), **dynamic**
(able/disposed) — again via the qualifier operator. A *booking* is deontic (a held right to a time
slot); a *forecast* is epistemic.

## Rendering & ordering

The event's prototype renders it (a payment → a timeline row: actor, amount, timestamp), composing
each child through *its* prototype. Ordered sequences of events (a history, an audit trail) are an
**ordered composition** — `order` on each event connection — read newest-first or oldest-first by the
prototype.

## Authoring procedure

1. Confirm it is a **verb/happening**, not an attribute (the date-and-actor test).
2. Create the event **type** (`referent NULL`) under an event/act hypernym; name it by the verb's head
   noun.
3. Add one connection per **participant role** (agent/object/recipient/…), each to its concept;
   the agent is an **entity**.
4. Add the **time** (`the_times`/`the_dates`); set **tense/aspect** and **modality** as qualifiers on
   the reified connection.
5. A concrete occurrence is a compositional instance (`referent 0`); order sequences with `order`.
6. Validate with `sccs-grammar-testing` (names read as clauses; completeness against the event's
   prototype obligation/cardinality).

## Required output format

Return the standard SCCS triple — a **Concepts** table (`local_ref`, `role`, `category`, `type`,
`referent`, `notes`), a **Connections** table (`local_ref`, `of`, `type`, `order`, `to`, `kind`,
`notes`), and a 2–4 sentence **Summary** naming the event, its agent and object, its time, and any
tense/aspect/modality qualifiers.
