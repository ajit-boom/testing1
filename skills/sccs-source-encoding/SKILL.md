---
name: sccs-source-encoding
description: Encode an authoritative source — a dictionary, business/management reference, industry glossary, or a client's product catalog — into well-structured, linkable, expandable SCCS concepts. Two modes: backbone (general sources → the reusable ~99% spine) and client/domain (a client's products + industry semantics → thin grafts). Reuse before create; never fork the spine. Use to ingest or graft a body of domain knowledge.
---

# SCCS — Source Encoding (build the data solid, from authoritative sources)

**Precedence:** the original engine is authoritative; this is a non-destructive overlay. This is a
**workflow** skill — it is an **outgrowth of the SNS**: every concept it extracts is named by sccs-naming (head-noun-last `category⟨type⟩`) and must pass the L1–L15 lint before commit. It orchestrates the others (noun-creation, hypernym-placement, lexical-relations,
grammar-testing, event, instances) to turn a *source* into clean concepts. It creates no new engine
mechanism.

The goal is **solid from the beginning**: encode the general structures once, from authoritative
sources, so the backbone answers ~99% of what any client needs; then each client adds only the thin
remainder. Two modes, same pipeline:

- **Backbone mode** — general sources (a **dictionary**, a **business/management reference**, an upper
  ontology, a standards body) → the **reusable spine** every client shares.
- **Client / domain mode** — a client's **product catalog** + **industry glossary** → **thin grafts**
  onto the spine, scoped to that client (`user_id`), never forking it.

## Why a dictionary is the ideal backbone source

A dictionary definition is **genus–differentia**: *"an X is a [genus] that [differentia]."* That maps
straight onto SCCS:

- the **genus** → the **hypernym** (subsumption; place under it — sccs-hypernym-placement);
- the **differentia** → the **qualifiers / components** that narrow the kind (the category chain, or
  component connections).

So a dictionary is a **pre-structured hypernymy source**: every entry already states where the noun
goes and what distinguishes it. Business/management references add the other half — the **processes**
(workflows = ordered event compositions) and the **standard business objects** (customer, order,
invoice, account, ledger, …) that recur across every client.

## The encoding pipeline (run per source term)

1. **Extract** — pull the head **noun**, its **sense (definition)**, the **genus** (hypernym), the
   **differentia** (qualifiers/parts), stated **relations** (is-a, part-of, has, opposite, same), and
   any **processes** (verbs).
2. **Resolve sense first** — disambiguate before placing: related senses → **sense-link** (polysemy);
   unrelated senses sharing a form → **split** (homonymy) (sccs-lexical-relations / sccs-grammar-testing).
3. **Decide kind vs role/quality (the gate)** — apply the **sortal test**: a *kind* (sortal) is placed
   in the tree; a *role/quality* (characterization) is **kept out** (a describing qualifier or an
   entity role-branch). Never graft a role into the kind tree.
4. **Reuse before create** — search the existing backbone; if the sense already exists, **link to it**,
   do not mint a second. Only create when the source introduces a genuinely new sense.
5. **Place** — **vet the head noun** (`sccs-noun-creation`: single lexeme, classification, properties),
   then graft the new sortal noun at its **genus** node (sccs-hypernym-placement); add the differentia
   as qualifiers/components.
6. **Decide type vs instance (the behavior test)** — a kind that **encompasses behavior** → a **type**
   (its prototype carries the behavior); a bare **value** → an **instance** (built with
   `sccs-instances`); a **fixed term/idiom** → **lexicalized** (sccs-phraseology). (This is the
   html-tag lesson: a defined term with behavior is a sub-kind, not a flat instance.)
7. **Relate** — wire the source's stated relations via **sccs-lexical-relations**: is-a → subsumption,
   part-of → meronymy (name the subtype), has → possession, opposite → antonymy (polarity/converse),
   same → synonymy (one concept + alias).
8. **Encode processes** — a defined **process/workflow** → an ordered **event composition**
   (sccs-event): each step a reified verb with its participant roles (valence).
9. **Validate** — run the gate (**sccs-grammar-testing** L1–L15 + the discipline checks) before commit.
10. **Mark the layer** — tag each concept **backbone** (shared, the spine) or **client/domain** (scoped
    by `user_id`), so the spine stays shared and grafts stay client-local.

## Linkable & expandable — the two guarantees

- **Linkable** — because every term **reuses** the shared backbone sense (no duplicate senses), a
  client's *invoice* resolves to the **same** `the invoice` backbone type as every other client's —
  so cross-client and cross-domain links/joins just work. Duplication is what breaks linkability.
- **Expandable** — growth is **organic and additive**: a new client term **grafts** onto the spine at
  the one right node (reuse before create); the spine is **never forked**, only extended; grafts ride
  on top (overlay precedence) and the backbone stays authoritative. Adding a client never edits the
  spine.

## The 99 / 1 discipline

The backbone (general sources) answers ~99% of structures and processes; a client contributes ~1% of
genuinely proprietary terms as thin grafts. **Always try the backbone first** (reuse); create only when
the source introduces a sense the spine genuinely lacks — and if a client keeps needing the same new
term, **promote it to the backbone** so the next client reuses it.

## Source-encoding lint

- **SE1 — create-before-reuse:** a source term minted without checking the backbone first → reuse the
  existing sense; create only a genuinely new one.
- **SE2 — ignored genus:** a dictionary noun placed without its stated **genus (hypernym)** → place it
  under the genus; add the differentia as qualifiers.
- **SE3 — forked spine:** a client graft that **redefines** a shared backbone concept → graft onto the
  shared concept; client specifics live on the **client layer**, never by forking the spine.
- **SE4 — unresolved sense:** a polysemous/homonymous term encoded without sense-resolution → resolve
  sense first (sense-link vs split).
- **SE5 — unmarked layer:** a concept with no **backbone / client** layer tag → tag it, so the spine
  stays shared and grafts stay scoped.
- **SE6 — flat defined-term:** a defined term that encompasses behavior stored as a **flat instance** →
  it is a **sub-kind (type)** with a prototype (the behavior test).

## Procedure

1. Classify the source (**backbone** vs **client/domain**) and tag the layer for everything it yields.
2. For each term, run the **pipeline** (extract → resolve sense → kind-vs-role gate → reuse → place →
   type-vs-instance → relate → processes → validate).
3. Keep **reuse before create** absolute; **never fork** the spine; promote recurring client terms to
   the backbone.
4. Emit only after the gate (L1–L15 + discipline checks) passes.

## Required output

The standard SCCS triple — **Concepts** (with a **layer** column: backbone / client), **Connections**
(with `kind`), and a 2–4 sentence **Summary** naming the source, the mode (backbone / client), the head
nouns reused vs created, the relations wired, and any processes encoded. Composite `(id, user_id)`;
names self-documenting; reuse-before-create stated where it fired.
