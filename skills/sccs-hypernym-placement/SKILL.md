---
name: sccs-hypernym-placement
description: Place a new noun at its correct node in the live SCCS noun-hypernymy — find the subsuming hypernym, test the is-a fit, reuse not duplicate, graft it. Use when situating or grafting a new noun.
---

# SCCS — Hypernym Placement (the organic-growth function)

`sccs-noun-creation` makes a single well-formed noun. **This function situates it in the live tree.**
It looks at the *existing* system and grafts the new noun at the one node where it belongs — using
the SNS as the central base — so the ontology grows by **extension off what is already there**,
never by inventing a floating root or a duplicate sense.

> This is the **operational** side of **hypernymy** — the placement act. The relation discipline
> over it (hypernymy as the head of the lexical-semantic relation family, and how it relates to
> meronymy / synonymy / antonymy / polysemy) is **sccs-lexical-relations**.

## The placement function (algorithm)

Given a new noun (sense + intended leaf, from `sccs-noun-creation`):

1. **Resolve the sense.** What *kind* of thing is it, and what leaf does it read to? (A quantity →
   numbers; an identifier → text; a distinct-format value → its own leaf.)
2. **Search the existing hypernymy, general → specific.** Descend from `being/thing` through the
   live tree, applying the **is-a test** at each node: "is the new noun *a kind of* this node?" Keep
   the **most specific** node that still subsumes it. The is-a test is **subsumption** (sortal
   *is-a-kind-of*) only: the hypernymy is the **kind tree**. A word that names a **role, phase, or
   quality** fails this test (a bridesmaid is not a *kind of substance* — it is a role a person
   plays); it does **not** graft here but routes to an entity role-branch or a qualifier
   (sccs-naming L12, sccs-noun-creation N7).
3. **SNS fit test.** Does the new noun read as a valid `the [parent…] [noun]` under the SNS —
   head-noun-last, reading as who/what in plain English — **and** does its leaf sense match the
   parent's branch (one-noun-one-leaf)? If the reading or the leaf clashes, the candidate parent is
   wrong; back up.
4. **Reuse before create.** If an equivalent noun already exists, **reuse it** — do not duplicate. A
   near-synonym attaches as a **sibling/specialization**, not a copy.
5. **Choose the graft point (organic extension).** Attach at the nearest subsuming node by extending
   the phrase the only two legal ways: a **qualifier after "the"** (front — narrows the kind) or a
   **word at the end** (tail — specializes the head). If the exact parent is missing, extend off the
   **nearest** existing node; never create an orphan.
6. **Propagate.** The new node **inherits** its parent's semantic properties and **prototype**;
   record only what it *adds* or *overrides*.

## What it guarantees (why it grows organically)

- **No orphans** — every noun hangs off an existing node.
- **No duplicate senses** — one meaning, one node (reuse enforced).
- **No leaf clashes** — a node's leaf sense agrees with its hypernym (the national-identifier-is-text
  rule).
- **Readable by construction** — every graft passes the SNS reading, so the tree stays
  English-legible as it grows.

The SNS is the **central base**: every placement decision is justified in SNS terms (the name reads,
head-noun-last, organic extension), which is what lets the system grow continuously without periodic
re-design.

## Worked example

New noun **"bridesmaid"** (a role a person plays at a wedding):
- sense → a *person in a role* → leaf resolves through the person/role branch;
- search → `being/thing → … → person → role/position`; the most specific subsumer is the role
  branch a person carries;
- SNS fit → `the bridesmaid entity` reads as "the entity that is a bridesmaid"; leaf agrees (an
  entity identity);
- reuse → no existing "bridesmaid"; near-kin "wedding guest" exists as a sibling role;
- graft → extend at the front: `the` + **bridesmaid** + `entity` (a branch of the entity trunk),
  sibling to `the wedding guest entity`;
- propagate → inherits the entity prototype + the role-branch behavior; adds only the wedding-context
  tie.

## Procedure

1. Take the new noun and its sense/leaf from `sccs-noun-creation`.
2. Run the algorithm (resolve → search → SNS-fit → reuse → graft → propagate).
3. If you must extend the tree, name the new intermediate node by the SNS and show it reads.
4. Hand the placed noun back for type assembly (`sccs-types`) and naming (`sccs-naming`).

## Required output

A **placement record**: the new noun, its **parent node** (the path from `being/thing`), the **graft
type** (front-qualifier / tail-specialization / reuse / new-intermediate), the **leaf sense**, what
it **inherits vs adds**, and the **SNS reading** that justifies it. State reuse decisions explicitly;
never emit an orphan or a duplicate sense.
