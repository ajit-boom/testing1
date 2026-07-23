---
name: sccs-postal-address
description: Model country-parametric postal/physical addresses: place hierarchy, address lines, thoroughfare classifications, per-country order/obligation/postcode. Use for any address, ZIP, or postcode.
---

# SCCS — Postal Address

Create addresses as a **country-parametric composition**. One shared trunk (`the postal address`)
defines the universal components, their order, obligation, cardinality, and render stages **once**;
each **country is a branch** that overrides only its junctions (which admin levels exist/are
required, the line-1 order, the postal-code mask, the local level names, punctuation/format). This
is the postal analogue of `sccs-units-of-measure`: country is to the address what *metric/English*
is to a quantity. Build the concepts with sccs-types/sccs-instances; name and wire per
sccs-ontology/sccs-naming; render and inherit per sccs-prototype/sccs-trunk-prototype.

## Where it lives (the place hypernymy)

From the hypernymy, under **`the place`** (a position), the geographic actors are **place entities**
(the `the place entity` branch of `sccs-entity-system`), arranged as a trunk so each level inherits
its parent's connections:

```
place ─ country ─ administrative area ─ (sub administrative area) ─ locality ─ sub locality ─ (neighborhood unit)
  │        │            │                                              │
  │        └ parametric branch root (the USA / France / Japan entity)  └ the city/town/village LEAF (identity = parent path)
  └ all geographic places
```

The **address** itself is *not* a place — the **type** `the postal address` is a composition
(`referent NULL`), and a concrete filled address is a compositional **instance** (`referent 0`)
that **references** the place chain and is attached to an entity (`the_entity_postal_address` as a
**part** for the identity/authentication address; `the_entity_s_postal_address` as a **possession**
for others — the same part/possession split as phone).

## Model: place hierarchy, address composition, country branch

Three concept families:

1. **The place entities** (actors, `referent 0`): `the USA entity`, `the California entity`,
   `the Los Angeles entity`, … Each `administrative area` and `locality` is **branch-lensed** per
   country (state / province / governorate / prefecture / oblast; city / commune / kota / post town).
   These are entities. The per-country **address-format branch** below is **not** an entity.

2. **The address types** (`referent NULL` — schemas/recipes):
   - `the postal address` — the trunk composition; branches `the mailing address` (routing) and
     `the physical address` (geographic place; may carry coordinates).
   - `the address line one / two / three` — ordered sub-compositions.
   - line-1 parts: `the house number` (→`the_texts`), `the thoroughfare` (= `the_thoroughfare_name`
     proper name + `the_thoroughfare_type` **classification**: avenue·street·road·boulevard·rue·via·
     jalan·straße…), or `the post office box` (the street ⊕ PO-box exclusivity pair).
   - line-2/3 parts: `the sub premise` (`the_sub_premise_type` classification + value),
     `the building name`, `the mail stop`.
   - `the postal code` (→`the_texts`, masked per country); plus country specials
     (`the secondary number`, `the short address` for Saudi; `the block`/`the lot` for Japan).
   - **`the <country> postal address`** — the parametric branch (e.g. `the usa postal address`).

3. **The address instances** (`referent 0`): a filled address — clean values on line parts, with
   `locality`, `administrative area`, `country` pointing at the **place entities**.

## Store once, read many — rendering is re-reading

Store an address **once** as clean values; the country branch's prototype is the **lens**.
Transliteration (RTL Arabic/Persian, CJK) and endianness are `format`/`order` concerns over the
**same stored values**, never a second record — exactly the units-of-measure property that lets
metric and English coexist for free. **Coordinates** on `the physical address` are a measured angle
value (lat/long → magnitude + angle unit), owned by `sccs-units-of-measure`.

## The line composition (and the delimiter rule)

`the address line one` is an **ordered composition**: `house/building number` → `thoroughfare`
(`name` then `type`) **or** `post office box`. The street ⊕ PO-box pair is `conditional`/`prohibited`
(present one ⇒ the other prohibited). **The delimiter is never a stored concept** — the space
between number and street, the comma before the city, the newline before the country are emitted by
the prototype's `punctuation-before`/`punctuation-after` stages on read. The street **type**
(avenue/street/…) and sub-premise **type** (apt/suite/…) are `kind = classification`, not head nouns
and not names.

## The country branch (parametric junctions) — built-in reference

Each `the <country> postal address` overrides `order · obligation · postal-code format · level names`
at its junctions. `endian`: little = specific→general; big = general→specific (the `order` axis).

| country | endian | line-1 | postal code (mask) | admin-1 lens | locality lens | distinctive |
|---|---|---|---|---|---|---|
| USA | little | number-first | `NNNNN[-NNNN]` | state (2-ltr) | city | ZIP+4 |
| UK (England) | little | number-first | `AANN NAA` | (county, deprecated) | post town `UPPER` | inward/outward |
| Australia | little | number-first | `NNNN` | state/terr (abbrev) | suburb `UPPER` | suburb+state+code one line |
| France | little | number · **type · name** | `NNNNN` | (département) | commune `UPPER` | CEDEX |
| Italy | little | **number-last** | `NNNNN` (CAP) | provincia (2-ltr) | comune | province code |
| Switzerland | little | **number-last** | `NNNN` | canton (opt) | town (multiling.) | de/fr/it names |
| Netherlands | little | **number-last** | `NNNN AA` | (province) | city | alphanumeric code |
| Japan | **big** | block-lot-gō | `〒NNN-NNNN` | prefecture | city/ward | **no street; chōme-banchi-gō** |
| China | **big** | prov→…→number | `NNNNNN` | province/region | city→district | big-endian native |
| Saudi Arabia | little | building-no first | `NNNNN-NNNN`; Short `AAAA NNNN` | (region) | city | secondary/geo number; short address |
| Iran | little | building-no first | `NNNNNNNNNN` (10) | province (ostan) | city | 10-digit |
| Russia | little | street-house-apt | `NNNNNN` | oblast/krai/republic | city | dom/korpus/kvartira |
| Philippines | little | number-first | `NNNN` | province (∅ NCR) | city/municipality | barangay |
| Indonesia | little | Jalan+No. | `NNNNN` | provinsi | kota/kabupaten | RT/RW; kelurahan/kecamatan |
| Egypt | little | building-no first | `NNNNN` (partial) | governorate | city | postal code `requested` |

## The prototype (5-stage render; trunk → country overrides)

`read(the postal address)` = `resolve` clean values → run the five stages in the **country branch's
order** → `compose` lines, each child deferring to its own prototype:

```
resolve  → order (endian, number-first/last)     # country override
         → visibility (visible | restricted)      # restricted masks a private address under a `see` grant
         → punctuation-before / -after            # spaces, commas, newlines emitted HERE
         → format (ZIP+4, AANN NAA, 〒, UPPERCASE post town, …)   # country mask
         → compose (line1 ∘ line2 ∘ locality/area/code ∘ country)
```

Carry the prototype on the trunk; each country branch overrides only the cells it must. Obligation
`required · requested · optional · conditional · prohibited`; cardinality `min..max`.

## Place-leaf identity by parent path (the three-Parises rule)

A locality is identified by its **resolved parent chain**, never its character value:
`Paris→New York→USA`, `Paris→Florida→USA`, `Paris→France` are **three** distinct compositional
instances, each inheriting its parent's address prototype. **Get-or-create keys on the chain**, not
the bare name — the direct repair for the `(characterValue, typeId)` merge hazard.

## Encoding on the unchanged schema (overlay)

Each `the <country> postal address` is a branch via the branch/trunk form: `of` = the country
address (branch), `type` = `the trunk`, `to` = `the postal address` (trunk at the shared node so all
countries inherit). Line parts, postal code, and the place references ride as ordinary
component/possession rows; the `locality`/`administrative area`/`country` connections **target** the
place entities. No schema change.

## Authoring procedure

1. Resolve or create the **place chain** (country → admin area → locality) as **place entities**;
   key the locality on its **parent path**.
2. Select the **country branch** `the <country> postal address`; pull its `order · obligation ·
   postal-code mask · level lenses` from the table.
3. Build the **line composition**: number + thoroughfare(name + **type classification**) **or** PO
   box (exclusivity), plus optional sub-premise/building name/mail stop; store **clean values only**.
4. Attach to the entity as a **part** (`the_entity_postal_address`, the identity address) or
   **possession** (`the_entity_s_postal_address`).
5. **Render** by reading values through the country prototype (order + format + punctuation);
   **convert/transliterate** by re-reading the same values through another lens — never store a copy.
6. **Validate** with `sccs-grammar-testing`: classifications named `the_x_type`, no stored
   delimiters, street ⊕ PO-box satisfied, required components present.

## Required output format

Return the standard SCCS triple — **Concepts** (the place entities, the country address branch, the
line sub-compositions, the thoroughfare/sub-premise **classifications**, the postal code, and any
address instance), **Connections** (`the trunk` branch declarations, the line/locality/area/code/
country component connections with `kind` and order, the place-entity targets), and a 2–4 sentence **Summary**
naming the country branch, its endianness, postal-code mask, and the required-component boundary.
State the country, base order, and any per-country override inline.
