---
name: sccs-units-of-measure
description: Model units of measure in metric and English systems for a dimension (length, mass, time, money/currency), converting via a canonical base. Use for measured quantities, conversion, or money.
---

# SCCS — Units of Measure

Create units of measure as SCCS concepts, in **both** the English (US/imperial) and metric (SI)
systems, generated automatically from a **dimension**. Anchored on the noun-hypernymy diagram and named by the **SNS** (sccs-naming) — each unit is a head-noun-last `category⟨type⟩` concept, its system and dimension carried as qualifiers. Build
the underlying concepts with sccs-types/sccs-instances; name and wire per sccs-ontology.

## Where units live (the hypernymy)

From the hypernymy, under **`the quantity`**:

```
quantity ─┬─ amount      → number, count noun
          ├─ dimension   → length, weight(mass), area, volume, time, temperature, electric, …
          └─ measure     → unit → { metric, English } ;  also bit, decimal
```

- A **dimension** is *what is measured* (length, mass, …) — the physical kind.
- A **unit** is a kind of **measure**; it is typed **metric** or **English**, and it **measures a
  dimension**. So a concrete unit is the composition *system × dimension* (e.g. a *metric length
  unit* = metre; an *English length unit* = foot).

## Model: dimension, unit branch, measured value

Three concept families, all "The X" types branching from `the quantity`:

1. **The dimension** — `the length dimension`, `the mass dimension`, … (category `the … dimension`,
   head noun `dimension`, branch off `the quantity`). Each dimension names its **canonical base
   unit** (SI) in its prototype: length→metre, mass→kilogram, time→second, temperature→kelvin, etc.
2. **The unit** — head noun `unit`, branched two ways and composed:
   - by **system**: `the metric unit`, `the English unit` (branches of `the unit`);
   - by **dimension**: `the length unit`, `the mass unit`, …;
   - composed: `the metric length unit`, `the English length unit`.
   Concrete units are the leaves: `the metre`, `the kilometre`, `the foot`, `the mile`, … each a
   branch carrying **(a)** a **factor to the dimension's base** (a possession
   `the_unit_s_factor` → a `the_numbers` value) and **(b)** its **symbol/format** in the category
   **prototype** (5-stage evaluation: `format` = e.g. `km`, `ft`).
3. **The measured value** — an instance: a `the_numbers` value bound to a unit branch. Express it as
   `^the <unit>` whose `referent` is the value (see below).

## Store once, read many — conversion is re-reading

Store each quantity **once, in its dimension's base unit**, as a `the_numbers` value; every unit is
just a different **lens** (factor + symbol) on that one stored value. This is the ordinary SCCS
referent chain (`specific unit → base value`), so **conversion needs no recomputation-and-restore —
it is reading the same base value through another unit branch:**

```
value_in(unit) = base_value / unit.factor            # linear units
base_value     = value_in(unit) * unit.factor
```

Example — a length stored as `the_numbers = 5000` (metres, the base):
`^the kilometre` renders `5000 / 1000 = 5 km`; `^the mile` renders `5000 / 1609.344 = 3.107 mi`.
Same referent, two branches. Metric and English units are **sibling branches over one base**, which
is exactly why both systems coexist for free.

## Auto-generate a dimension's unit families (procedure)

Given a dimension, emit: the dimension type (+ base unit in its prototype), the
`metric <dim> unit` and `English <dim> unit` branches, and each concrete unit as a leaf branch with
its `the_unit_s_factor` (to base) and prototype `format` (symbol). Pull factors from the table below.

## Reference factors (built-in knowledge; factor = base units per 1 of the unit)

| dimension | base (SI) | metric units (factor) | English/US units (factor) |
|---|---|---|---|
| length | metre `m` | km 1000, m 1, cm 0.01, mm 0.001 | mi 1609.344, yd 0.9144, ft 0.3048, in 0.0254 |
| mass (weight) | kilogram `kg` | t 1000, kg 1, g 0.001, mg 1e-6 | lb 0.45359237, oz 0.028349523125, st 6.35029318 |
| area | square metre `m²` | km² 1e6, m² 1, cm² 1e-4 | acre 4046.8564224, sq mi 2589988.110336, sq ft 0.09290304, sq in 0.00064516 |
| volume | litre `L` | kL 1000, L 1, mL 0.001 (m³ = 1000 L) | gal(US) 3.785411784, qt 0.946352946, pt 0.473176473, cup 0.2365882365, fl oz(US) 0.0295735295625 |
| time | second `s` | — (SI prefixes rare in use) | shared: s 1, min 60, h 3600, day 86400 |
| speed | metre/second `m/s` | km/h 0.2777778, m/s 1 | mph 0.44704, ft/s 0.3048 |
| temperature | kelvin `K` | **affine — see below** | **affine — see below** |

`time` and `speed` are largely **system-shared** (not split metric/English); model them as units of
the dimension without forcing a system branch. SI prefixes (k, c, m, µ, …) are themselves a small
metric sub-branch you can apply to any metric base, rather than re-listing every prefixed unit.

## Temperature and other affine units (special)

Temperature is **affine**, not a pure ratio — a unit needs a **factor and an offset**. Carry both
(`the_unit_s_factor`, `the_unit_s_offset`) and convert through the base (kelvin):

```
base_K = value * factor + offset            # to base
value  = (base_K - offset) / factor         # from base
```

| unit | factor | offset | note |
|---|---|---|---|
| K | 1 | 0 | base |
| °C | 1 | 273.15 | base_K = °C + 273.15 |
| °F | 5/9 | 459.67·5/9 = 255.372… | base_K = (°F + 459.67)·5/9 |

Flag any dimension whose units are affine so the linear shortcut above is not applied.

## Encoding on the unchanged schema (overlay)

Each unit is a branch declared via the branch/trunk form: `of` = the unit (branch), `type` =
`the trunk`, `to` = `the <dimension> unit` (or `the unit`) — trunk placed at the most-general shared
node so all units of a dimension inherit. The factor/offset and the value ride as ordinary
possession rows; the target of a measured value is its `the_numbers` base value. No schema change.

## Authoring procedure

1. Identify the **dimension(s)**; create each as a branch of `the quantity` and set its **base
   unit** in the prototype.
2. **Auto-generate** the `metric` and `English` unit families from the table — each concrete unit a
   leaf branch with `the_unit_s_factor` (and `_offset` if affine) and a prototype `format` symbol.
3. Express a **measured value** by storing the `the_numbers` value **in base units once**, then
   referencing it through whichever unit branch the user wants displayed.
4. **Convert** by re-reading the base value through the target unit branch (linear) or the affine
   formula (temperature); never store a second copy.
5. Keep names self-documenting (`the metric length unit`, `the foot`); composite `(id, user_id)`;
   never hard-delete.

## Money is a measured quantity (currency)

Money is not a number — it is a **measured quantity in the currency dimension**, so it follows the
same magnitude + unit shape as length or time:

- **the currency dimension** sits under `quantity → dimension`; its **units are the world's
  currencies**.
- **the currency unit** is the denomination — *the US dollar*, *the Thai baht* — carrying its symbol
  (`$`, `฿`), ISO code, decimal places, and its **issuer** (the national origin) as a possession to a
  place: `the_currency_unit_s_issuer → the place` (the United States, Thailand).
- **the monetary amount** = a measured value whose unit is a currency unit: `magnitude (the_numbers)
  + currency unit`. This is the structural concept.
- **price is a ROLE, not a structure** — a monetary amount in an offer/asking context; cost, fee,
  salary, balance are the same structure in other roles. One structure, many role-names.

So `$3` = magnitude `3` + `^the US dollar`; `฿10` = magnitude `10` + `^the Thai baht`. **Syntax comes
from the currency unit's prototype** (symbol = punctuation-before; grouping + decimals = format), so
`1250` + `^US dollar` renders `$1,250.00`. **Conversion is the one exception to re-reading by a
constant factor:** a currency's factor to a base is a **live exchange rate** looked up at read time —
storage and rendering are identical to any measured quantity; only the conversion *function* is a
market lookup, so a currency unit carries **no stored factor**.

**Canonical vs the composition framing.** `the monetary amount` (a measured value: magnitude +
currency unit) is the **canonical** structure — it unifies money with every other measured quantity
under R1, and `price` / `cost` / `fee` / `salary` are roles over it. A second framing models a price
as a **composition** of two parts (`the_price_s_quantity` + `the_price_s_currency`); treat that as the
*same two facts presented as explicit parts*, not a separate structure. Prefer the measured-value form
(`the_<good>_s_price → the monetary amount`); reach for the explicit composition only when a domain
needs the quantity and currency to be independently addressable parts. Either way the currency unit is
the carrier and the prototype renders it. (Commerce roles live in **sccs-commerce**.)

## The bare-number rule (R1)

A **bare `the_numbers` is only ever a dimensionless count**; every quantity that is meaningless
without a unit is `magnitude + unit`. Price needs a currency, a duration needs a time unit, a size
needs a data unit, **coordinates need an angle unit** (degrees) — all measured. The *only* legitimate
bare numbers are genuine **counts, ordinals, and booleans** (a favorite flag, an installment count, a
position ordinal). When a stored `the_numbers` is meaningless without a unit, convert it to a
measured value.

## Qualifier dimensions (gradation, time, modality)

Qualification (adjectives, adverbs, tense, modality) reuses this same dimension machinery — it is not
a separate system:

- **graded qualifiers are measured quantities** — `very red` is a high magnitude on a *redness*
  dimension; a comparative (`larger`) is a comparison on the *size* dimension; structurally identical
  to `5 kg`.
- **tense and aspect** are values on the existing **time dimension** (and its date/time leaves) — a
  past-valued time-qualifier on a reified connection.
- **modality** is **three** new **modal dimensions**, each an ordered scale:
  - **epistemic** — certainty: impossible … possible … certain (`might`);
  - **deontic** — obligation: forbidden … permitted … obligatory (`must`);
  - **dynamic** — ability: cannot … can (`can`).
  Kept separate because something can be obligatory yet uncertain, or possible yet forbidden — one
  scale would lose those distinctions. The qualifier operator that attaches these to a head lives in
  **sccs-ontology**.

## Required output format

Return the standard SCCS triple — **Concepts** (the dimension, the system/dimension unit branches,
the concrete unit leaves with factor + symbol, and any value instances), **Connections** (the
`the trunk` declarations, `the_unit_s_factor`/`_offset` possessions, and the value's base reference),
and a 2–4 sentence **Summary** naming the dimension, its base unit, and the metric + English units
generated. State the base unit and any affine handling inline.
