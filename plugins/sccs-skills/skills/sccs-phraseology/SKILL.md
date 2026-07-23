---
name: sccs-phraseology
description: Model multi-word expressions — idioms, collocations, light-verb/support constructions, fixed formulae — classified by compositionality (transparent to opaque) and fixedness (frozen to flexible). Idioms are stored lexicalized (not decomposed); collocations are usage preferences; the verb is bleached in light-verb units. Use for idioms, set phrases, collocations, or "stored not assembled" expressions.
---

# SCCS — Phraseology (the multi-word expression inventory)

**Precedence:** the original engine is authoritative; this is a non-destructive overlay.

Phraseology is the **lexicon side** of fixed language: the inventory of **multi-word expressions** that
are **stored, not freely assembled**. Where **sccs-construction** owns the **productive, schematic** end
of the form-meaning continuum, this skill owns the **substantive, fixed** end. Each unit is classified
on two axes — **compositionality** (transparent → opaque) and **fixedness** (frozen → flexible).

## The classification grid

| compositionality ↓ / fixedness → | frozen | semi-fixed | flexible |
|---|---|---|---|
| **opaque** | **idiom** (kick the bucket) | | |
| **semi-transparent** | binomial (back and forth) | **light-verb** (take a walk) | |
| **transparent** | formula (how do you do) | | **collocation** (strong tea) |

## Idiom → a lexicalized concept (stored, not decomposed)

An **idiom** is **opaque**: its meaning is **not** the sum of its parts. Model it as a **lexicalized**
concept (the lexicalized marker — `sccs-naming/references/linguistic-foundations.md`): **stored whole
with its own meaning, never assembled from its parts**, and exempt from the compositional name checks.
**Render from the stored form, not by composing the parts** — this is the render path the grammar gate
was missing. The literal reading may co-exist as a separate **literal sense**; **sense-link** the two,
do not merge them.

## Collocation → a usage preference (not a hard rule)

A **collocation** is a **conventionalized co-occurrence** (strong tea, make a decision) — transparent
but restricted by **usage**, not grammar. Model it as a **preference** connection between the two
concepts (a `collocation` flag), **never an obligation or cardinality**: "powerful tea" is *odd*, not
*ungrammatical*. A collocation informs render and suggestion, never validity.

## Light-verb / support construction → meaning on the noun

In *take a walk* / *make a decision* the verb is **semantically bleached**; the event meaning lives in
the **noun**. Model as an **event** (sccs-event) whose verb is a **light connector** and whose **content
noun** carries the meaning (`the walk`, `the decision`). Never put the event's meaning on the bleached
verb.

## The idiom principle (retrieval precedence)

After Sinclair: when a **stored** multi-word unit exists, prefer it over open assembly. So resolution
checks the **phraseological inventory first**; only if no fixed unit matches does it fall through to
compositional assembly. This precedence is what keeps an idiom from being mis-parsed into its literal
parts.

## Phraseology lint

- **PH1** — an **opaque** expression assembled compositionally (so it mis-resolves) → mark
  **lexicalized**, store whole, do not decompose.
- **PH2** — a **collocation** stored as an obligation/cardinality → model it as a **preference** flag,
  not a hard rule.
- **PH3** — a **light-verb** unit with its meaning placed on the verb → move the event meaning to the
  content noun (sccs-event).
- **PH4** — a **fixed expression** re-assembled on each read → store it as one lexicalized unit (idiom
  principle).
- **PH5** — a **schematic, productive** pattern modeled here → it has open positions; hand off to
  **sccs-construction** (the continuum boundary).

## Procedure

1. Identify the unit and place it on the **grid** (compositionality × fixedness).
2. Opaque/frozen → a **lexicalized** concept (stored whole). Transparent/restricted → a **collocation**
   preference. Light-verb → an **event** with the meaning on the noun.
3. For an idiom, keep the **literal sense sense-linked**, not merged.
4. Wire the **idiom-principle** precedence (stored unit before assembly); run the phraseology lint.

## Required output

The standard SCCS triple — **Concepts**, **Connections** (with `kind`), and a 2–4 sentence **Summary**
naming each expression, its grid position (compositionality × fixedness), how it is stored
(lexicalized / collocation / light-verb), and its sense-links. Composite `(id, user_id)`.
