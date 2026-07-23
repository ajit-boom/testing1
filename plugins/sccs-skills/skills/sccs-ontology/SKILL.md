---
name: sccs-ontology
description: SCCS graph mechanics: the_concepts/the_connections, the referent rule, branch/trunk/target overlay, qualifiers (modification/tense/modality). Use to model a domain as concepts and connections.
---

# SCCS Ontology Authoring

Model a domain as **concepts** (units of meaning) and **connections** (typed, ordered edges) in
the Semantic Concept Connection System. Always say **concept**, never "node" — a concept can be a
type, a category, a referent, and carry behavior through its connections.

**Precedence:** the original spec is the authoritative engine; the upgrade here (the prototype,
the branch/trunk/target connection, the naming corrections) is a **non-destructive overlay** on
the unchanged schema. Engine wins on conflict; use the new language. Full field-by-field
dictionary and worked example: `references/sccs-spec.md`.

## `the` — the determinative

`the` (concept id `0`) is not a prefix; it is the **determinative operator**. It encodes
who/what/where/when/why made a concept, and so carries **authority, validity, security, access**,
and each system's **deictic sense of self** (resolved by the indexical layer — see **sccs-deixis**). Every "The X" name is an authority claim. The schema
obeys it too (`the_concepts`, `the_connections`).

> **Engine grounding (sccs-engine-core).** In the live export `the` is the first row and *declares
> its metalanguage* (American English); its instance counterpart `^` is the concept
> **`the_secure_instance_of`**. The determinative is self-grounding: `the` is typed by `the_category`,
> which is categorised by `the`. *(The export numbers `the` at row id 1 while the overlay calls it
> concept 0 — a numbering convention to confirm against the live DDL, not a semantic difference.)*

## The concept model

- A concept is **category + type + referent**, stored as `category_id`, `type_id`, `referent_id`
  (each with a `_user_id` for tenant scoping). `category` has **inheritance priority** over `type`.
  The tenant key `(id, user_id)` is itself modelled as concepts (`the_id_combination`), so ownership
  is intrinsic to the key, never a separate `the_*_owner` connection.
- `category_id`/`type_id` reference other concepts; `referent_id` is polymorphic; `character_value`
  inlines a value to avoid a join.
- **Combination** = an *ordered* arrangement (a vector / recipe). **Composition** = a *grouping*
  (ordered or unordered) of a concept's connections.

## The referent rule (decide first)

| `referent_id` | Meaning |
|---|---|
| **NULL** | **Typological concept** — a pure type/recipe. |
| **0** | **Compositional instance** — "read the connections fanning out from me." |
| **a small integer** | **The value itself**, inline — integers need no leaf table. |
| **a row-id** | **Referring instance** — a pointer into a leaf table (or another concept), or inline `character_value`. |

Leaf tables are **extensible**: a value earns its own leaf when it has a distinct canonical format,
validation, and operations (even if it could be stuffed into text). Current set: `the_characters`,
`the_texts`, `the_numbers` (floats/decimals; integers inline), `the_times`, `the_dates`,
`the_documents`, `the_UUIDs`, `the_hashs`, `the_ipv4`, `the_ipv6` — with routing/communication
primitives (port, MAC, protocol) to follow as cloud instances arrive (a port is a leaf; a message,
session, or route is a **composition**). **`the_documents` is the gatekeeper** — `sounds`, `images`,
and `videos` are **document types** resolved through it via specialty media servers (the document row
holds the locator; the bytes live on the server).

## A type is inert without its prototype

A type is a **grouping of two or more concepts** (the category/type chain). The grouping alone is
inert — ids pointing at ids. What makes it a definition is the **prototype carried on the category
concept**: it tells the engine **where the referent lives** (location-statements: host → schema →
table → data-field → instance-id-field) and **how to read it** (the 5-stage evaluation: order,
visibility, punctuation-before, punctuation-after, format). The prototype inherits down the
category chain to the root creation-string default. It is best understood as the **function
contract** for all upward functionality — resolve, read, the five render stages, connect, compose,
inherit, derive, convert — modeled in full in **sccs-prototype** (start there for the concept +
prototype). (See also sccs-types.)

## Naming — name the relation, never concatenate endpoints

*The full naming discipline — the SNS lexicon, naming data structures, and reporting facets — lives
in **sccs-naming**. The essentials needed to wire connections are summarized here.*

A name must read as English that recovers *who–relation–what* with no lookup (the precision test).

- **Possession (`_s_`)** → "the [host]'s [attribute]"; the attribute *is* the relation:
  `the_entity_s_address`, `the_person_entity_s_legal_identifier`.
- **Part (`_`)** splits by what the part-relation is:
  - **classification** — the to-noun classifies the host: `the_entity_type`.
  - **structural componency** — for a part *contained in* or *dividing* a composite, name the
    **kind** concept (`containment`, `division`); read "the [member] is a [kind] of the
    [composite]." This fixes the broken `the_place_place` (→ a containment). Note: role/kind
    specializations (student, employee, company) are **branches of the entity**, not componency —
    see sccs-entity-system.
- **Head noun last**; modifiers narrow leftward; draw the head from the noun-hypernymy vocabulary.
- **Proper names are possessions / `character_value`**, never baked into a type-name.
- **`^`** = the secure instance operator.
- Self-corrections this enforces: `container` → `the composite`; `slot` → `order` + a position
  branch; `branch_concept_concept_id` → `the_branch_concept_id`.

## Connections — the branch/trunk/target model

A connection is a directed edge. The legacy form `of_the_concepts_id → (type_id, order) →
to_the_concepts_id` is **kept and authoritative**. The upgrade generalizes the single `type` into:

- **branch** — the full conceptual phrase carrying the semantics (a type or an instance of one);
- **of the trunk** — the core concept the branch elaborates;
- **to the target** — the endpoint.

It reads as an English sentence and is **prototype-driven**: the **branch prototype** declares
which trunks are valid, **where each trunk sits in the branch tree**, and the access control for
the connection; the **trunk prototype** declares valid targets, the verb, and constraints.

**Trunk placement (inheritance):** the trunk attaches at the most-general node where the shared
semantics live, so specific branches inherit rather than copy (e.g. `the toyota model z car`
anchors its trunk at `^the model z car`). Don't pile every trunk on the instance's own root.

**The overlay (how it lands without touching code):** do **not** add columns. Introduce `the trunk`
as a new **connection-type** and record the trunk as an ordinary row — `of_the_concepts_id` = the
branch, `type_id` = `the trunk`, `to_the_concepts_id` = the trunk. The target is the `to` of the
ordinary semantic row. `of_the_concepts_id` is **retained** (the trunk sits on the `to` side — no
rename). The overlay is **additive and read-only**; the original stays authoritative; promotion
later is a flip, not a rebuild.

## Referent chains (specialization)

A referring instance's `referent_id` climbs **specific → general → raw value**, each layer adding
only its branch's data/behavior:
`^the cellular phone number → ^the phone number → the_numbers value`. Store the raw value once at
the base; specialization layers decorate it. A referent is decoded by walking the connections of
its type and category concepts.

## Build direction

Type names assemble **left → right** (head noun appended last; `category_id` chain climbs to
`the`). Instances **anchor on the head noun** and generalize by peeling the **leftmost** modifier.
The two live on different fields — `category_id` carries the name assembly, `referent_id` carries
the head-anchored subsumption. A specific instance requires its more-general instance first.

## Entities (the actor pattern)

An **entity** is a concept that can change data. There are **three** types: **individual** (the
atom), **the composite** (company/place/group are *lenses*, not separate types), and **program**.
The subtype is set by a single `the_entity_type` discriminator, and it only distinguishes
**individual vs composite**. Role/kind specializations (`the student entity`, `the patient
entity`, `the company entity`, `the employee entity`, …) **stand as branches of the entity trunk**
that an identity carries — the organic way new understanding grows — **never components of other
concepts**. Full model and the access system: **sccs-entity-system**.

## The qualifier (one operator for modification and tense/aspect/modality)

A **qualifier** places a `(dimension, value)` on a **head** — and that one operator covers two
language layers at once:
- **head = a noun** → **modification**: an adjective, adverb, or degree (`red`, `quickly`,
  `very large`);
- **head = a (reified) connection** → **tense · aspect · modality**: when and with what force a
  relation holds (`happened`, `was happening`, `must`).

Because connections are reified as rows, qualifying a connection is the *same act* as qualifying a
noun — hang a qualifier on the row. A qualifier is **defining** or **describing**:
- **defining** (restrictive) → climbs into the **category chain** as a sub-type (`the red car` as a
  kind) — **but only if it survives the sortal test** *"is the result still a kind of the head?"* A
  qualifier that introduces a **role, phase, or quality** fails that test (it is **characterization**,
  not a sub-kind) and must instead become a describing qualifier or an entity role-branch (see
  sccs-naming L12). The noun-hypernymy is the **kind tree**: subsumption only.
- **describing** (non-restrictive) → an **instance value on an attribute**, reusing the
  measured-quantity pattern (`this car is red`). Tense, aspect, and modality are always describing.

Value types: **categorical** → a kind; **graded** → a measured quantity (`very red` = a magnitude on
a redness dimension); **temporal** → a value on the time dimension (tense/aspect); **modal** → a
value on one of the three modal dimensions. The dimensions (including the modal ones) live in
**sccs-units-of-measure**; the adjective/adverb reading is **sccs-naming**'s connector grammar.

## Authoring procedure

1. Identify root types as "The X" (`referent_id` NULL); give each its **category prototype**
   (location + read-method).
2. Compose richer types (qualifier-category + base-type), head noun last.
3. Set each concept's referent state: NULL / 0 / integer-inline / row-id pointer.
4. Define type-level connections (component connections), **naming the relation** (`_` classification or kind /
   `_s_` possession).
5. Create instances; wire referent chains; fill instance connections.
6. For relational meaning beyond a plain type, use the **branch/trunk/target** form via the
   `the trunk` overlay; place trunks at the right level.
7. Add control/security stubs (`entry_time_stamp`, `termination_datetime`, `(id, user_id)`); never
   hard-delete. **Ownership is intrinsic** — the owner is the `user_id` half of `(id, user_id)`, not
   a modeled connection. Mark any **derived** concept (a view/recap/smart-collection that is a query
   over other concepts, evaluated on read) as derived so it isn't treated as stored data — see
   sccs-instances.

## Branch-directed trunk roots

A trunk's root is **not** a single fixed node. **A branch may create a trunk at its own appropriate
root**, chosen by *peeling qualifiers from its own name* down to the generalization it wants to join —
the branch decides which qualifiers to drop. Example: `the red toyota model A car` peels `red`
(colour) and `toyota` (manufacturer) and directs a trunk at `the model A car`, so a view or query
rooted there ranges over **all** model A cars of any make or colour. A branch may plant several trunks
at several roots (`the model A car`, `the toyota car`, `the red car`) to sit in several cross-cutting
generalizations at once. The available roots are exactly the generalizations the **name already
encodes** — head-noun-last peel (sccs-naming) and referent-chain peel (sccs-instances) — so a branch
never invents a root, it selects one already implicit in its name. The cross-cutting query falls out
for free: the rooting *is* the index. How a connection branch then *resolves and renders* against its
chosen root is owned by **sccs-trunk-prototype** (`references/inheritance-and-rooting.md`).

## Required output format

ALWAYS return: **Concepts** (`local_ref`, `role` ∈ type / instance-referring /
instance-compositional / instance-hybrid, `category`, `type`, `referent`, `notes`);
**Connections** (`local_ref`, `of`, `type` generated name, `order`, `to`, `kind`, `notes`); and a
2–4 sentence **Summary** naming root types, the domain, and any referent chains. Keep names
self-documenting; use composite `(id, user_id)` throughout; state assumptions inline.
