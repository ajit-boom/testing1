---
name: sccs-noun-creation
description: Create and classify the head NOUNS concepts are built on: place a noun in the hypernymy, classify it, assign properties, run the noun lint. Use when introducing or vetting a noun or head-noun.
---

# SCCS — Noun Creation

Every SCCS type is a `the X` phrase whose **rightmost word is its head noun** — *what the thing is* —
with the category chain to its left narrowing it. This skill governs **the head noun itself**:
choosing it, placing it in the noun-hypernymy taxonomy, classifying it, and assigning its semantic
properties, so no concept is ever built on an ad-hoc, unplaced word. Assembly *around* the noun
(category + head) is `sccs-types`; naming the relations is `sccs-naming`; this skill tightens the
**noun**.

## A noun is created with four things

1. **A single lexeme.** A head noun is **one word** (`agreement`, `number`, `place`, `moment`).
   Multi-word meaning is built by category + head assembly (`the payment` + `agreement`), **never** by
   compounding into one noun (`payment_agreement` is not a noun — it is two concepts).
2. **A category** (how the noun refers): **common** (the default — the only kind that heads a type),
   **proper** (a named instance — *not* a type head; carry it as a possession / `character_value`),
   **pronoun**, **abbreviation**, **acronym** (references, handled as such, not as type heads).
3. **A hypernym placement** (mandatory). Attach the noun under an existing node of the being/thing
   tree; if the right node is missing, extend the tree off the nearest existing node — never leave a
   noun unplaced.
4. **Semantic properties** (the facets it carries — declare each):

   | property | values |
   |---|---|
   | countability | countable · uncountable · neither · invariable · collective |
   | plurality | singular · plural · both · dual |
   | gender | masculine · feminine · neuter · both · unknown · neither |
   | polarity | affirmative · negative · unknown · neutral · neither |
   | concreteness | concrete · abstract |
   | animacy | animate · inanimate |
   | hypernymy depth | which node in the tree (the depth/sense in which the word is used) |

## The noun-hypernymy taxonomy (where a noun may attach)

```
being / thing
├── fauna · flora ; life form (animal · plant · fungus ; human/primate ; life stage: embryo→deceased)
├── object        → natural · man-made ; mass · artifact · imaginary ; solid · liquid · gas · mineral · food · electronic
├── idea · feeling · quality · attitude · emotion · state · event · action · study · style
├── quantity      → amount (number · count noun) · dimension (weight·length·area·volume·time·temperature·…) · measure (unit → metric · English)
├── position      → titles (president · manager · employee · salesperson · …) ; professional/job title
├── place · date
└── grammar       → semantics · morphology · lexeme · word
```

The branch a noun lands on **determines its data sense** — see N5 below.

## Tighten the bounds — noun lint (reject before creating)

**The N-lint (N1–N7) is owned here** — this skill is its single source of truth, exactly as
`sccs-naming` owns the L1–L15 naming lint. Other skills (`sccs-hypernym-placement`, `sccs-entity-system`)
*cite* it (e.g. N7) but never restate it; the gate that **enforces** it is `sccs-grammar-testing`,
alongside the L-lint.

| # | Reject if… | Fix |
|---|---|---|
| N1 | the head is **multi-word / underscored** | split into category + single head noun |
| N2 | the noun **has no hypernym** | attach it under an existing node (or extend the tree off one, with reason) |
| N3 | **category or properties undeclared** | classify (common/…); assign the property facets |
| N4 | a **proper name is used as a type head** | demote to possession / `character_value`; the type head is the common noun |
| N5 | the noun's **leaf-sense contradicts its hypernym** | a `quantity` noun is numeric (`the_numbers`); an identifier/code/label is `idea`/`grammar` → **text** (`the_texts`); fix the placement or the leaf (this is the source of the "national identification number is numeric text, not a number" error) |
| N6 | the noun **duplicates an existing one** | reuse the existing noun; prefer the most specific existing hypernym |
| N7 | a **role / title / phase lexeme is placed as a permanent kind** in the being/thing tree (it fails the sortal test *"is a thing of this word a kind of substance, or a role a substance plays?"*) | route it to an **entity role-branch** (`the employee entity`, `the bridesmaid entity` — sccs-entity-system), not a substance-kind graft; the `position → titles` branch holds these **role lexemes**, realized as role-branches, not as sortal substance kinds |

## Interface to the prototype (forward hook)

A created noun is the **head of a type**, and a type is inert until its **category prototype** binds
it. Noun-creation produces exactly what that prototype will later consume:

- the **hypernym placement** → suggests the **leaf table / data sense** the prototype's
  location-statements resolve to (quantity→`the_numbers`, idea/grammar→`the_texts`, object→
  `the_documents`, …) — and a value with its **own canonical format** gets its **own** leaf, not
  text (`the_UUIDs`, `the_hashs`, `the_ipv4`, `the_ipv6`; the leaf set is extensible);
- the **semantic properties** → suggest the prototype's **5-stage rendering facets** (format, label,
  visibility — e.g. plurality drives singular/plural display, countability drives count vs mass).

For now this skill **stops at noun identity, placement, and properties** — it does not yet write the
prototype. The handshake is one-directional: *create the noun here so that, later, the prototype of
any type can bind to it cleanly*. When the prototype mechanism is wired per-type, it reads this
noun's placement + properties rather than re-deciding them.

## Procedure

1. Take the intended head noun; **strip any modifiers** (those become the category chain, not the
   noun).
2. Run the **noun lint** N1–N7.
3. **Classify** the noun (common for a type head) and **place** it under the nearest hypernym.
4. **Assign** the semantic-property facets.
5. Record the **intended leaf sense** implied by the hypernym (the prototype hook), but do not write
   the prototype.
6. Hand the placed noun to `sccs-types` to assemble the type and `sccs-naming` to name relations.

## Required output

Return a **Nouns** table (`noun`, `category`, `hypernym-path`, `countability`, `plurality`, `gender`,
`polarity`, `concreteness`, `animacy`, `intended-leaf`, `notes`) and a 2–4 sentence **Summary** of
what was created, where each attached, and any tree extensions. State assumptions inline; never emit
an unplaced or multi-word noun.
