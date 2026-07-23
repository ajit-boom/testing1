---
name: sccs-engine-core
description: The live two-table engine as it describes itself: self-describing schema, category-type assembly, instance marker, name grounding, leaf set, composite key. Load first to anchor modeling in real rows.
---

# SCCS — Engine Core (the system as it describes itself)

This is the **ground truth** layer: what the live `the_concepts` / `the_connections` engine actually
contains, read from the stage-13 export. Every other SCCS skill is an overlay on *this*. Load it
first so names, types, and instances are anchored to real rows rather than reconstructed. The
overlay is non-destructive; the engine row stays authoritative.

## The two tables, and the one operator

A concept is a row with `id · category · type · referent · (evaluate) · full_value · leaf`. The
**`category` and `type` columns hold concept-ids that point back into the same table.** Names are
built by a single operator — **`category⟨type⟩`** (stored literally as `category<type>`) — applied
recursively; `evaluate()` renders the assembly into an English token:

```
the⟨concept⟩            → the_concept
the_concept⟨id⟩         → the_concept_id
the_connection⟨order⟩   → the_connection_order
```

There is no second mechanism. Fields, classifications, compositions, and prototypes are all
`category⟨type⟩` resolved recursively. The SNS morphology (`sccs-naming`) is this operator made
human-readable; rendering (`sccs-prototype`, the 5 stages + `evaluate()`) is this operator read
left-to-right.

## The self-describing core (the schema is rows of itself)

The table describes its own schema **as rows inside itself**:

```
id 1   the                    ^⟨the_category⟩, referent→"the"   ← root; declares its metalanguage
id 5   the_category           the⟨category⟩
id 6   the_concept            the⟨concept⟩          ← what a row of the_concepts IS
id 7   the_concept_id         the_concept⟨id⟩       ┐ the 4 columns of the_concepts,
id 8   the_concept_category   the_concept⟨category⟩ │ each itself a concept row
id 9   the_concept_type       the_concept⟨type⟩     │
id 10  the_concept_referent   the_concept⟨referent⟩ ┘
id 14  of_the_concept         of⟨the_concept⟩       ← preposition-bound FK
id 15  to_the_concept         to⟨the_concept⟩       ← preposition-bound FK
id 16  the_connection         the⟨connection⟩       ← what a row of the_connections IS
id 17  the_connection_id              the_connection⟨id⟩              ┐ the 5 columns of
id 18  the_connection_of_the_concept  the_connection⟨of_the_concept⟩  │ the_connections
id 19  the_connection_type            the_connection⟨type⟩            │
id 20  the_connection_order           the_connection⟨order⟩           │
id 21  the_connection_to_the_concept  the_connection⟨to_the_concept⟩  ┘
id 22–26  the_composition · the_order · the_combination · the_composition_of_the_concept · the_prototype_of_the_concept
id 96  the_prototype
```

**Three fixed points make it self-describing** — these are load-bearing, not trivia:

1. **The determinative grounds itself.** `the` (1) is *typed by* `the_category` (5), and
   `the_category` is *categorised by* `the` (1). The loop is the root; there is no external axiom.
2. **`the_concept` is a `the_concept`.** Row 6 defines what a concept is and is itself a concept row
   whose columns (7–10) are concepts. The schema lives inside the data it governs.
3. **Names are data.** Every structural token has a `^the_name=<token>` instance (the
   grounding block, ids ~10,000+) resolving to the `the_characters` leaf — the metalanguage is
   stored in the object language. This is the engine evidence for **store once, read many**.

## The instance marker `^`

`^` is **`the_secure_instance_of`** (id 4 = `the_secure_instance⟨of⟩`). So every `^the X` row reads
**"the secure instance of the X"** — an instance, as opposed to the type `the X` (`sccs-instances`).
The caret is not punctuation; it is a resolved concept.

## Language connectivity (why every row reads as English)

- **The metalanguage is declared at id 1.** `the` carries a taxonomy statement naming **American
  English**; the engine decodes morphology because it declares the language it is written in.
- **Endpoints are preposition-named.** A connection's two foreign keys are not `from`/`to` ids —
  they are `the_connection_of_the_concept` and `the_connection_to_the_concept`, built from the
  preposition concepts **`of`** and **`to`**. So an edge reads "the connection *of* the [subject]
  *to* the [object]." These are the concrete names behind branch/trunk/target (`sccs-trunk-prototype`).
- **Field vs classification share `⟨⟩`**, split only by whether the inner token is a field-name or a
  sub-concept — the engine reads structure off the name.

## The confirmed value set (leaves)

`the_characters` (51) · `the_text` (52) · `the_number` (53) · `the_time` (54) · `the_date` (55) ·
**`the_document` (56) — the gatekeeper** → `the_sound` (57) / `the_image` (58) / `the_video` (59) ·
`the_uuid` (60) · `the_ipv6` (67) · `the_password` (69) · `the_users` (91) · `the_url` (141).
Integers are inline (`the_integer`, 48); identifiers/codes are **text**, not numbers.

## The tenant key as concepts

The `(id, user_id)` boundary is modelled, not just enforced in DDL: `the_id_combination` (40) with
`id,user_id` (43), `id,parent_user_id` (46), `system_id,user_id` (186). Ownership is intrinsic to
the key, never a modelled `the_*_owner` connection.

## The eight build layers (id order = dependency order)

| layer | ids | establishes |
|---|---|---|
| 0 bootstrap | 1–5 | the determinative, `^`, the_category |
| 1 self-schema | 6–27 | the_concept/the_connection + columns; composition/prototype |
| 2 identity & tenancy | 28–47 | the_user / system / passcode / function; the composite key |
| 3 value/leaf | 48–141 | integer/character/datetime + the leaves + media + net/security |
| 4 prototype | 26, 96 | the render+obligation+cardinality contract |
| 5 domain & render | 142–9,999 | entities, contact/network domain, ~1,050 HTML tag/attribute concepts |
| 6 name grounding | 10,000–11,999 | `^the_name=` token + value instances → characters |
| 7 instances/data + reserve | 12,000–100,002 | leaf-backed value rows; reserved id space |

The full genesis row map (ids 1–141), the self-reference set, and the layer sizes are in
**`references/engine-genesis-rows.md`**.

## How to use this skill

- **Before modeling**, check whether a concept already exists in the engine core (reuse before
  create — the hypernymy and the schema are already seeded).
- **Anchor names** to `category⟨type⟩`; anchor endpoints to `of`/`to`; anchor instances to `^`.
- **Never propose a second schema.** A change is an additive overlay that resolves to these rows.
- Hand off: `sccs-naming` (read the morphology), `sccs-ontology` (graph mechanics), `sccs-instances`
  (`^` instances), `sccs-prototype` (render via `evaluate()`).

## Enforcement (genesis + the first 2,000 named rows are a hard block)

The **genesis core (ids 1–141)** and the **first 2,000 named rows** are bedrock and are
**hard-blocked** — a single tier, no warn region. The grammar gate **rejects** (does not silently
rewrite, does not merely warn) a proposed concept that:

1. **collides** with an existing engine row in this range under a different meaning — **reuse the
   existing row** instead of minting a synonym (lint **L13**, `sccs-naming`);
2. **contradicts the confirmed leaf set** — a value whose sense disagrees with its leaf
   (an identifier typed as `the_number`, a character outside `the_characters`, media not routed
   through `the_document`);
3. **mis-indexes the character set** — a `the_character` instance whose **referent is not its byte
   offset (1–255)**.

**What the block keys on.** Enforcement matches on the **`category⟨type⟩` assembly and the
`referent`**, *never* on the leaf `data` column (contaminated — D1). It runs against a **canonical
index** that already applies the discrepancy resolutions, so the block does not force new work to
replicate the export's mistakes:

- a duplicate row (D2: `the_tag`/`the_attribute` set B) resolves to its set-A twin — one block target,
  not two;
- a flat-instance vocabulary member (D3: `^the_html_tag=h1`) resolves to its canonical **sub-kind**
  (`the h1 tag`), so promoting it is **reconcile-in-place**, not a blocked re-mint;
- you may **never add a parallel row** to "fix" a drifted one — reconcile the existing row.

In short: reuse-before-create is absolute across 1–2000; the model's canonical reading defines the
block target; the contaminated leaf column is never a key.

### The first 2,000, grouped (full map in `references/engine-first-2000-groups.md`)

`1–5` bootstrap · `6–27` self-schema · `28–47` identity/tenancy · `48–63` leaves ·
`64–369` domain heads + id/data/name fields · `370–507` + `960–1003` HTML attributes ·
**`513–767` the character set (ASCII/Latin-1, referent = byte offset 1–255; 65=`A`, 97=`a`, 32=space)** ·
`768–959` HTML tags · `1130–1321` tags **set B (⚠ duplicate)** · `1323–1824` attributes **set B
(⚠ duplicate)** · `1881–1989` derived (`evaluate()`).

### The export is imperfect — known discrepancies (the model wins)

- **D1** the leaf `data` column is **contaminated** (ids ~370–1824) with an unrelated word-series;
  the **referent** is authoritative, not column 11.
- **D2** **duplicate** tag/attribute vocabularies (`^the_html_tag` vs `the_tag`; `^the_html_attribute`
  vs `the_attribute`) — reconcile to the instance form.
- **D3** tags (`h1`, …) are stored as **flat instances**; they should be **sub-kind types**
  (`the h1 tag` is-a-kind-of `the html tag`) with per-occurrence **instances** `^the h1 tag` — the
  type/token test decides (does a particular occurrence carry its own data?). See the reference.
- **D4/D5** stray `full_value`/codegen cells and the `the` = id 1 vs 0 numbering are noise/convention,
  not semantics.
