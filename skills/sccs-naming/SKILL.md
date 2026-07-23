---
name: sccs-naming
description: Apply the Semantic Naming System (SNS): name concepts, connections, tables, and fields by the relation (`_` part / `_s_` possession), head-noun-last. Use when naming or renaming anything in SCCS.
---

# SCCS — Semantic Naming System (SNS)

The **SNS** is SCCS's naming discipline: every element of the system is identified by a normalized
American English phrase, so that a name *is* its definition — reading it recovers who made it, what
it is, and how it relates, with no lookup. American English is the base language for its precision
and legal/scientific specificity. This skill is the authoritative home for naming; other SCCS skills
defer here. Engine precedence holds: the original spec is authoritative; the overlay corrections
below are additive.

## The determinative `the` — the root of every name

`the` (concept id `0`) is not decoration; it is the **determinative operator** that asserts
**authority, validity, security, and the sense of self** of whatever it names — encoding the who /
what / where / when / why behind a concept. Every name begins (explicitly or by inheritance) with
`the`. The schema itself obeys this: **all system table names start with `the_`** (`the_concepts`,
`the_connections`) to separate system structure from a user's lookalike tables. Contrast with `a` /
`an` / no-article: only `the` carries determinative authority.

## The core lexicon (fix these meanings before naming)

| lexeme | meaning in SNS |
|---|---|
| **component** | a *taxonomically named reference to a location with values*; may itself be a **combination and/or composition of other components**. The atom of naming. |
| **concept** | a combination of **category + type + referent**; inherits its parents' connections. |
| **category** | the primary **grouping** of concepts (has inheritance priority over type). |
| **type** | the inherited **behavior** of a group of nouns. |
| **referent** | a reference to an **instance** — an inline integer value, or a row id in a system/foreign table. |
| **connection** | an **ordered or unordered** link of one concept to another (or itself). |
| **order** | the **sequence** of a concept's connections. |
| **combination** | a **uniquely, directionally ordered** arrangement (a vector / lexeme / safe-combination); first component, then a second combined with it, then a third, … |
| **composition** | the **grouping** of connections for a referenced concept (order optional). |
| **factor** | a specific **part of a combination**. |
| **function** | a process that returns a value. **label** = a function that formats a concept to return *visual* info; **format** = a function giving data a specific representation. |
| **taxonomy** | a classification (tree) system; SCCS has taxonomies for the language *and* for concept composition. |
| **data** | a memory location holding a normalized value. The SNS's original 10 types are **ratified-reconciled to 7 leaf tables** — `the_characters`, `the_texts`, `the_numbers` (floats/decimals; integers inline), `the_times`, `the_dates`, `the_documents`, `the_UUIDs` — with **`the_documents` the gatekeeper**: `sounds`, `images`, `videos` are **document types** resolved through it via specialty media servers (the row holds the locator; the bytes live on the server). |
| **id** | the indexed **identity/location** of a specific row+column; every `_id` field references a record's identity. |

## Name the relation — the grammar

A connection-type name is built from the relation, **never** by concatenating endpoints. Two forms:

```
classification / componency (part):  <from> "_"   <to>   # "is a / is a <kind> of"
possession (elucidative):            <from> "_s_" <to>   # "has / is described by"
```

- **Possession `_s_`** — the to-noun *is* the relation: `the_entity_s_address`,
  `the_person_entity_s_legal_identifier`.
- **Application `of`** — `the <F> of the <X>` applies a function-concept's functions to an argument
  (`the prototype of the concept`); the function/head comes first (the inverse of possession), and a
  name never ends in a dangling `of`. In assembly the argument `the X` is a **complete type** (it
  fills the `type` component whole) and `the <F> of` is the **category** (climbing to `the`) — never split
  `the` off as the category and demote the argument to a bare word. The `of` connector is owned by
  **sccs-application**.
- **Part `_`** splits by what the part-relation is:
  - **classification** — the to-noun classifies the host. Split it by **sortality** (the kind-vs-type
    distinction):
    - **subsumption** — a sortal *is-a-kind-of*: `the_x_kind` reads "the X is a kind of the Y". It
      climbs the noun-hypernymy (the **kind tree**) and is the head noun's **identity line**. One
      ultimate kind per concept.
    - **characterization** — a *non-sortal* "is" (a quality, role, or phase: "the car is red", "the
      person is a student"). It **never** climbs the hypernymy — model it as a **describing
      qualifier** (an instance value) or an **entity role-branch** (sccs-entity-system).
    - The legacy `the_x_type` token conflated the two; prefer **`the_x_kind`** for subsumption and a
      **qualifier / role-branch** for characterization.
    - **Reconciliation with the project's `kind` vocabulary.** The consolidated core lists the
      connection `kind` as *component / possession / **classification***. **Subsumption** and
      **characterization** are the two faces of that single "classification" value — subsumption is
      the sortal face (climbs the kind tree), characterization the non-sortal face (never does). When
      emitting the §16 output contract, either `classification` (project term) or the precise
      sub-term is acceptable, but the precise term is preferred.
  - **structural componency** — name the **kind** concept (`containment`, `division`); read
    "the [member] is a [kind] of the [composite]." (Fixes the broken `the_place_place` → a
    `containment`.)

**The connector layer (what a relation *is*).** These relations are **linguistic connectors** —
verbs, prepositions, and genitives — and they are **first-class concepts held in the trunk
ontology** (grammar branch: verb / preposition / conjunction), not just string conventions. The
three forms above are the **elementary** connectors — `_s_` is the genitive ("… of …"),
classification is the copula ("… is a …"), containment is a locative preposition ("… in …") — and
**verbs and prepositions extend the set** ("captured", "works for", "at", "with"). Each connector
carries a **valence** (which noun-roles it binds: a verb takes subject + object, a preposition takes
a complement), so every connection **reads as an English clause** and the engine can *read* edges,
not merely traverse them.

**Kind vs type vs connector — three senses, kept apart.** The word "kind" and the word "type" are
overloaded in the raw engine; the SNS keeps three senses distinct:

| sense | what it is | where it lives |
|---|---|---|
| **the kind (sortal)** | the **head noun's** identity-conferring sense — *what the thing fundamentally is* | the noun-hypernymy (the kind tree); the **`type` component**'s head |
| **the type (assembled)** | the whole `the X` = category (qualifiers) + kind (head) + referent | the concept row; its head *is* the kind, its restrictive qualifiers are **sub-kinds** |
| **the connector** | the **relation category** of an edge (component · possession · subsumption · characterization · containment · division · branch · attribute) | the connection's **`kind` field** — which names the *connector*, **not** a sortal |

So the connection-row column literally named `kind` should be read as **the connector**; the sortal
"kind" is the head noun. A restrictive qualifier narrows the kind (a **sub-kind**, still sortal) only
if it survives the test *"is the result still a kind of the head?"*; a role, phase, or quality fails
that test and is **characterization**, never a sub-kind (see L12).

**The engine's literal form (ground truth — see sccs-engine-core).** The SNS is not a parallel
notation; it is the human-readable face of one engine operator: **`category⟨type⟩`** (stored
`category<type>`, rendered by `evaluate()`) — it appends a **type** (the head, last) to a **category**.
There is no "attribute"; the operands are **category** and **type**. `the⟨concept⟩` → `the_concept`;
`the_concept⟨id⟩` → `the_concept_id`. Three engine facts the SNS must respect:
- **Endpoints are preposition-named.** A connection's two foreign keys are `…_of_the_concept` and
  `…_to_the_concept`, built from the concepts **`of`** and **`to`** — not `from`/`to` ids. Branch =
  the `of` side, target = the `to` side.
- **Names are data.** Every token is stored once as a `^the_name=<token>` instance resolving to
  `the_characters` — the engine evidence for **store once, read many**. Do not invent a synonym; the
  token already exists as a row.
- **The metalanguage is declared at the root.** Concept `the` (id 1) names **American English** as
  the decoding language; that is why morphology is authoritative and NLP is unnecessary.

Rules that keep names self-documenting:
- **Head noun last**, modifiers narrow leftward; draw the head from the **SCCS noun-hypernymy
  taxonomy** (being/thing → place, object, idea, quantity, dimension, measure, position, …).
- **Proper names are possessions / `character_value`**, never baked into a type-name
  ("United States", "John Doe" attach to an instance, not the type).
- **`^`** marks the secure instance (`^the dragon`).
- **Precision test:** if the name doesn't read back as who–relation–what in plain English, rename it.
- Self-corrections this enforces: `container` → `the composite`; `the_place_place` → `containment`;
  `branch_concept_concept_id` → `the_branch_concept_id`; a "slot" → `order` + a position branch.

## Component, combination, composition → human data structures

The SNS builds readable structures out of **components**. Use the three relations deliberately:

- A **component** names a location-with-values; a richer component is a **combination** or
  **composition** of smaller components.
- **Combination** = *ordered* (a record is a combination of field values; a lexeme is a factored
  combination of concepts; a process is an ordered combination of steps). Order is meaningful.
- **Composition** = *grouping* of a concept's connections (a compositional instance, `referent 0`,
  *is* its composition). Order optional.
- Map onto the physical layer with SNS table vocabulary: **table** (holds records) → **record** (a
  combination of values) → **field** (a named value) → **row/column**. Name each as a `the_`
  component so the structure reads as English: `the_record`, `the_field`, etc. A human data
  structure is just well-named components combined/composed so a person can read the schema directly.

## Reporting facets — label and format

A **facet** is a named, formatted dimension a concept can be reported or filtered along. The SNS
produces facets through the **label** and **format** functions and the **category prototype's
5-stage evaluation** (order, visibility, punctuation-before, punctuation-after, format): the
prototype *evaluates* a concept's data and *renders* it for human reading (e.g. raw digits
`1235551212` → the phone-number facet `(123) 555-1212`). The **semantic properties** of a
noun-concept — countability, plurality, gender, polarity, concreteness, animacy, and **hypernymy**
(depth of sense) — are its natural facets; expose them as named, formatted connections so reports
and filters read in plain language rather than against raw values. Naming and reporting are one act:
a good name already declares the facet.

## Procedure

1. **Fix meanings** against the lexicon; identify the head noun from the hypernymy taxonomy
   (create/vet it via `sccs-noun-creation`).
2. **Name each concept** as a `the X` phrase (head last), category chain climbing to `the`.
3. **Name each connection by its relation** — `_s_` possession, `_` classification, or a `kind`
   for structural componency — and record `order` where the relation is a combination.
4. **Assemble structures** from components via combination (ordered) / composition (grouped); name
   the physical `the_` tables/records/fields the same way.
5. **Declare facets** — give report-bearing concepts a label/format (in the category prototype) so
   they render for humans; surface semantic-property facets as named connections.
6. Run the **precision test** on every name; apply the self-corrections; keep `(id, user_id)`.

## Naming lint — run before emitting any name (mandatory)

Re-derive every name from the rules; do **not** carry a name forward from a prior artifact without
re-checking it. **Reject and fix** a name if any of these fire:

| # | Reject if… | Wrong → Right |
|---|---|---|
| L1 | a **type/concept token contains `_`** (underscores are for connection/table names only; a multi-word type is spaced `category + head-noun`) | `the payment_agreement` → `the payment agreement` (cat `the payment` + type `agreement`) |
| L2 | the **head noun mis-types the leaf** — a quantity head (`number`) on non-arithmetic data (an identifier/code) | `the national identification number` (→`the_numbers`) → numeric **text** identifier (→`the_texts`) |
| L3 | a **possession host ≠ the owning concept** at the level the relation lives (`the_<host>_s_<attr>` host must resolve to a real owner; no arbitrary shorten/generalize) | type `the payment agreement` but `the_agreement_s_term` → `the_payment_agreement_s_term` (or define on `the agreement` deliberately) |
| L4 | the **attribute repeats a qualifier already in the host** (redundant) — **except a sanctioned reflexive** (`the_prototype_s_prototype`, where the repetition *is* the content; governed by **L10**) | `the_payment_agreement_s_payment_term` → `the_payment_agreement_s_term` |
| L5 | a **count/quantity is stored when it is merely the size of an ordered combination** (flag derived-vs-stored) | store `…_s_installment_quantity` → prefer **derived** (count of installments) unless denormalized on purpose |
| L6 | an **ordered relation (combination) lacks `order`** | `…_s_installment` with no order → add `order` |
| L7 | a **proper name is baked into a type token** | `the United States place` → `the place` + possession `…_s_name = "United States"` |
| L8 | a **legacy token** appears | `container`→`composite`; `the_place_place`→`containment`; `slot`→`order`+position branch; `branch_concept_concept_id`→`the_branch_concept_id` |
| L9 | the name **fails the reads-as test** — it does not read back as who–relation–what in plain English | rename until it does |
| L10 | a **reflexive edge (`of == to`)** that is **not** at a most-general, **self-terminating apex** — a name asserting self-reference (`the_x_s_x`, an `of == to` row) anywhere other than where an operator's regress legitimately terminates | reject; self-reference is licensed **only** at the regress-terminating apex (`the` is its own category; the connection operator; `the prototype of the prototype`; `the prototype of the composition`) and must read as a true self-referent tautology, not a cycle |
| L11 | an **`of`-application splits `the` off as the category**, demoting the argument to a bare word (the argument must be a complete `the X` type as the `type` component; `the <F> of` is the category) | `the prototype of the concept` as `cat=the, type=concept` → `cat=the prototype of, type=the concept` |
| L12 | a **non-sortal qualifier is baked into the type as a sub-kind** — a role, phase, or quality grafted into the category chain / hypernymy as if it narrowed identity (fails the sortal *"is it a kind of?"* test) | `the student person` (type) → `the person entity` + a `the student entity` **role-branch**; `the red car` stored as a sub-type → `the car` + a **describing qualifier** "…is red" |
| L13 | a name **collides with an enforced engine-core row** (genesis ids 1–141 or the first 2,000 named rows) under a **different meaning** — minting a synonym for a character, leaf, connector, schema field, or controlled-vocabulary member that already exists (sccs-engine-core) | reuse the existing engine row (`the_characters`, `of`/`to`, `the_concept_type`, `^the_html_tag`, …); a character instance's referent must be its **byte offset** |
| L14 | **token drift / non-canonical doubling** — a token repeats the determiner (`the_the_…`), repeats a head noun (`…_name_name`), repeats a segment (`parent_user_idparent_user_id`), or **coexists with an alternate spelling of the same concept** (`firstname` ↔ `first_name`). Coexisting spellings key to different rows and **silently split stored data (ship-blocking)** | collapse to the single canonical token: `the_the_full_name` → `the_full_name`; `the_full_name_name` → `the_full_name`; `parent_user_idparent_user_id` → `the_parent_user_id`; pick one spelling and migrate the other |
| L15 | **ambiguous bare `of`/`to` connector** — a connector uses the bare preposition without committing to a sense. English `of` (≈18 contemporary senses, dozens historically) and `to` (≈17) are massively polysemous, so a bare `of`/`to` does not say *which* relation it is | pick the **sense-specific** connector (see `references/connector-senses.md`): SCCS `of` = **branch-root** (objective-genitive/argument) only; possession → `_s_`; partitive/material → composition; agent → an event role; SCCS `to` = **target/goal** only; recipient → recipient role; purpose → purpose; comparison → ratio |

Emit only after every name passes L1–L15. The orchestrator's validation pass treats this lint as a
hard gate.

## Required output

Return a **Names** table (`element`, `kind` ∈ concept / connection-type / table / field / facet,
`name`, `relation/role`, `reads-as` plain-English gloss, `notes`) and a 2–4 sentence **Summary**
naming the head nouns used, the relations chosen, and any reporting facets defined. Keep every name
self-documenting and state assumptions inline.

## Linguistic grounding & the irregular tail (see `references/linguistic-foundations.md`)

The SNS is built along the grain of English: head-noun-last = the **Right-Hand Head Rule**;
`of`-application head-first = head-initial PPs; connectors-with-valence = **dependency/valency
grammar** + thematic roles. It faithfully matches the **regular, compositional core** of English.
The **irregular tail** needs two markers (do not force them through compositional naming):

- **Lexicalized** — a concept whose meaning is **not** derivable from its parts (an idiom /
  construction: "kick the bucket"). Named and stored, **not decomposed**; its referent is a stored
  opaque sense. Do not lint it for compositional well-formedness.
- **Sense-linked (regular polysemy)** — one English **form**, several **related** senses ("email" =
  address vs message). **Not** an accidental collision to split: keep the shared head and **link the
  senses** (a dot-object), e.g. `the_email` with `…_s_address_sense` / `…_s_message_sense`.

A *perfect* 1:1 match to English is impossible (vagueness, indexicality, quantifier scope, metaphor,
cross-linguistic relativity, symbol grounding); the achievable goal is **canonical core + these
markers for the tail.**
