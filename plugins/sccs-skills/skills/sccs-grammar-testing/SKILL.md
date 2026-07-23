---
name: sccs-grammar-testing
description: Validate SCCS names and output: the L1-L15 naming lint (owned by sccs-naming) and the N1-N7 noun lint (owned by sccs-noun-creation), English normalization, and completeness against a prototype. Flags (never rewrites) meaning changes. Use to check names or output.
---

# SCCS — Grammar Testing

**Precedence:** the original engine is authoritative; this corrector is a non-destructive overlay.
It validates and normalizes; it never changes what a name *means*.

SCCS is built on precise American-English morphology, so name and output grammar is load-bearing.
This skill is the actor that **checks and corrects** it. It exists because the two existing pieces —
the sccs-naming **lint** (grammar of *names*) and the prototype **evaluation** (grammar of *output*)
— are otherwise disconnected and rely on discipline.

## The actor: a program entity

The corrector is `^the grammar program entity` with `the_entity_type = program` (see
sccs-entity-system). As a program actor it can be the **author** of records, so every correction is
attributable through its CRUD provenance. It is given the dataset(s) it acts on; credentials are
referenced, never stored raw.

## Where it attaches — once, at the branch

The gate is incorporated **only at the branch concept** (the `of` side of a connection), never copied
onto every node. From the branch it **uses the trunk to resolve upward to the root concept**, so a
single attachment validates the entire inheritance chain: the branch's own names and overrides
first, then each component it inherits — resolved up the trunk — all the way to the root. This is the
single-point principle applied to validation itself — one incorporation point, inherited — matching
how connection-prototype information is hoisted to its single most-general node
(see sccs-trunk-prototype).

## The pipeline

```
read the concept graph
  → validate the names      (the naming lint L1–L15  +  the noun lint N1–N7)
  → normalize the reading   (determiner · number · subject-verb agreement · one term per concept)
  → check completeness      (each component's obligation + cardinality)
  → render via the prototype(order · visibility · punctuation · format)
  → emit corrected output
       ⌙ flag for review     (anything that would change meaning — do NOT rewrite)
```

### 1. Validate the names — the naming lint (hard gate)
Re-derive every name from sccs-naming and reject/fix on L1–L15 (head-noun-last; `_` part vs `_s_`
possession; proper-names never in a type token; leaf-sense matches the hypernymy; no doubled-noun
componency; no redundant qualifier; legacy tokens; reads-as test; non-sortal-qualifier-as-sub-kind; **token drift/doubling**;
**engine-core collision**; **reflexive edge only at a self-terminating apex**; **`of`-application not split**; **`of`/`to` sense committed**). Legacy-token fixes include `container → composite`,
`slot → component connection`, `the_place_place → containment`, **`property → component connection`**,
and **`facet →` name the semantic concept (`label`/`format`)**.

**The noun lint N1–N7 runs here too (its owner is `sccs-noun-creation`).** Where a name introduces a
**head noun**, the gate also enforces the N-lint — single-word head, has a hypernym, category/properties
declared, no proper-name head, leaf-sense matches the hypernym, no duplicate, and **no role/title/phase
lexeme grafted as a permanent kind** (the sortal test, N7 ↔ L12). The two families are parallel: the
L-lint (owned by sccs-naming) governs **how a name assembles**; the N-lint (owned by sccs-noun-creation)
governs **whether its head noun is well-formed and rightly placed**. Both are enforced here.

**Engine-core enforcement (L13) — single-tier HARD BLOCK.** For the genesis core (ids 1–141) and the
first 2,000 named rows, **reuse before create** is a hard block (not a warning, no exempt region; see
**sccs-engine-core**): **reject** a name that collides with an existing engine row under a different
meaning (a re-minted character, leaf, connector `of`/`to`, schema field, or controlled-vocabulary
member). Match on the `category⟨type⟩` assembly and the **referent** against the **canonical index**
(which already collapses D2 duplicates and reads D3 flat instances as their sub-kind), never the
contaminated leaf `data` column. Reconcile a drifted row **in place** — never add a parallel. A
`the_character` instance's referent must be its **byte offset (1–255)**.

**Polysemy vs accidental collision.** Before splitting an L13/L14 name clash, ask whether the two
meanings are **related** (regular polysemy — "email" address vs message) or **unrelated** (accidental
homonymy). Related → **sense-link** them under one head (a dot-object: `the_email` +
`…_s_address_sense` / `…_s_message_sense`), do **not** split into two unrelated concepts. Unrelated →
split. A **lexicalized** concept (idiom/construction, meaning not derivable from parts) is exempt from
the compositional name checks entirely — it is stored, not assembled (see
`sccs-naming/references/linguistic-foundations.md`). Its render path now has an owner: a **fixed**
expression is stored and rendered whole by **sccs-phraseology** (the idiom principle — stored unit
before assembly); a **schematic, productive** pattern carries its meaning on a construction prototype
via **sccs-construction**. The grammar gate checks the *decision* (lexicalized vs compositional); those
two skills supply the *render*.

### 2. Normalize the reading — English grammar
Enforce determiner (`the`), grammatical **number** (singular vs plural — which also selects inline
`the_integer`/`the_character` vs the plural base table), subject-verb agreement, and **one
consistent term per concept**: once a concept is named (e.g. a *component*), every later reference
resolves through that name, so a renderer can never silently drift to "label" or "property".

### 3. Check completeness — obligation + cardinality
Read each component's obligation and cardinality from its prototype (sccs-prototype):
`the required` absent → **fail**; `the requested` absent → **soft flag**; `the conditional` →
evaluate its condition; `the prohibited` present → **fail**; cardinality outside
minimum/maximum-occurrence → **fail**.

### 4. Render — the prototype evaluation
Render each referent through its prototype's 5-stage evaluation; for connections, the `kind` selects
the reads-as grammar (sccs-trunk-prototype).

## Non-destruction rule (the line that must not be crossed)

The corrector may **silently** fix anything that changes only grammar: an article, a plural, an
agreement error, terminology drift to a non-canonical synonym. It must **flag, never rewrite**,
anything that would change *which relation a name asserts* — part ↔ possession, a different head
noun, a different obligation — because that alters semantics, not grammar. When unsure whether a fix
is grammatical or semantic, flag.

## Testing procedure (use this to test a skill, a name set, or live output)

1. Assemble the **input**: the names/connections/output under test plus the prototypes that govern
   them.
2. Run the four passes above; record each finding as `pass`, `auto-corrected`, or `flagged`.
3. For `auto-corrected`, show before → after and the rule (L#, normalization, render).
4. For `flagged`, state the meaning-change risk and the human decision required; do not apply.
5. Re-run after fixes until only `pass` and intentional `flag` remain.

## Also validates external producers (pre-sync)

The gate is not limited to names already in the graph. Run it **pre-sync against an external
producer's emitted `the_*` vocabulary** — a scraper, importer, or agent — so naming drift and
malformed connections are caught at write time rather than discovered later as missing query
results. The four passes apply unchanged; surface flags before any `Sync`.

## Required output format

Return a **Findings** table (`element`, `pass` ∈ validate/normalize/completeness/render,
`status` ∈ pass/auto-corrected/flagged, `rule`, `before → after` or `meaning-change risk`,
`notes`) and a 2–4 sentence **Summary** of what was checked, what was corrected silently, and what
was flagged for human review. Never assert a fix that crosses the non-destruction rule.
