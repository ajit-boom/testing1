---
name: sccs-deixis
description: Model indexical (context-dependent) concepts and reference forms — the current user, now, today, here, this/that, a vs the, pronouns vs nouns — that resolve against the deictic center (who = user_id, when = timestamp, where = tenant). Definiteness, demonstratives, pronoun-antecedent and the givenness scale. Use for "current/now/today/here/this/my", a vs the, pronouns, or resolve-at-read values.
---

# SCCS — Deixis (the indexical concept)

**Precedence:** the original engine is authoritative; this is a non-destructive overlay. It adds no
columns — an indexical is an ordinary derived concept whose prototype reads context.

The determinative `the` already declares the system's **deictic sense of self** — who / what / where /
when / why made a concept (see sccs-naming, sccs-ontology). This skill builds the layer that *resolves*
that declaration: the **indexical concept**, whose referent is **not stored but computed against the
deictic center at read time**.

## The deictic center (the origo)

The zero-point every indexical resolves against — Bühler's *origo*, the **I-here-now** of an
utterance. In SCCS the origo is the **reading context**, and its anchors already exist in the schema,
so this skill wires up hooks that are already there:

| deictic anchor | SCCS carrier (already in the engine) | reads as |
|---|---|---|
| speaker / "who" (person) | `user_id` — the creator/owner, intrinsic to `(id, user_id)` | the current user · the owner · the creator |
| time / "when" | `entry_time_stamp` / the system clock | now · today · the current date |
| place·frame / "where" | the tenant · the composite `(id, user_id)` | here · this context |
| the pointed-to "this" | `^` the secure instance | this record · this entity |
| discourse / "the above" | the current branch / concept graph (order + referent chain) | the above · the previous · the latter |
| social — relative **authority** | the **grant lineage** (`delegated_by`), sccs-entity-system | my manager · my grantor · the responsible individual |
| social — relative **status** | a **title possession** `the_entity_s_title` (title ≠ access) | Dr. · Sir · a courtesy title · formal register |

## Character vs content (the resolution model)

After Kaplan: an indexical carries a stable **character** — a *rule* from context to value — and
yields **content** — the value in a given context. The SCCS mapping is exact:

- the **character** = the indexical's **prototype** (a fixed resolve rule, e.g. "read the current
  `user_id`") — a **type**, `referent NULL`;
- the **content** = what `resolve` returns when run against the origo.

So an indexical is a **derived concept** (`referent 0`, resolve-on-read): it stores the *rule*, never
the *value*. `now` resolved Monday and `now` resolved Tuesday share one character but give different
content. This reuses the existing **derived/view-instance** pattern (sccs-instances) and the
prototype's **`derive`** function (sccs-prototype) — nothing new in the engine.

## Modelling an indexical

```
^the current user        derived · referent 0
   the_current_user_s_anchor  -> user_id (person deixis)        resolve: read origo.user_id
^the today               derived · referent 0
   the_today_s_anchor         -> the system clock (time deixis) resolve: read origo.date
```

The category prototype's **resolve reads a deictic anchor instead of a stored referent**; everything
else (render, format) is ordinary.

## The five deixis categories, in SCCS

- **person** — the current user / the owner / the creator → resolve `user_id`. **"my X"** is a
  possession whose host resolves to the current user (never bake "my" into a token).
- **place (spatial)** — here / this context → resolve the tenant frame; **"this"** → the secure
  instance `^` (the individuated, pointed-to concept).
- **time** — now / today / the current date → resolve the clock; tense/aspect qualifiers anchor here
  (the qualifier operator, sccs-ontology).
- **discourse** — the above / the previous / the latter / the current concept → a reference *into the
  concept graph itself*, via `order` + the referent chain (this is anaphora; referent chains already
  do the resolution).
- **social** has **two distinct anchors — keep them apart:**
  - **relative authority** — my manager / my grantor / the responsible individual → resolve via the
    **grant lineage** (`delegated_by`): your manager is whoever granted your access-control
    role-branch, up to the composite at the root (computed from provenance, not stored).
  - **relative status / honorific** — Dr. / Sir / a courtesy title / formal register → resolve via a
    **title possession** (`the_entity_s_title`), which is **nominal and carries no access**.

  *Title ≠ access* is exactly why these are two anchors, not one: authority is the grant lineage,
  status is a title possession, and a social indexical must say which it means (sccs-entity-system).

## Determination & reference form (a · the · this/that · pronoun · noun)

The five categories above resolve the deictic *center*; this resolves the **reference expressions**
that point at concepts. They vary on two axes — **definiteness/identifiability** and **word-class
(noun vs pro-form)** — and form one **givenness scale**.

**Definiteness — `a/an` vs `the`.**
- **`the` (definite)** — the referent is **identifiable / uniquely retrievable** in context. This *is*
  the SNS determinative `the` (authority · validity · **identifiable**), and at the instance level the
  secure `^`. Definite = "you can tell which one."
- **`a / an` (indefinite)** — introduces a **new, not-yet-identifiable** referent. It is **not** the
  determinative (only `the` carries that authority — sccs-naming). Model it as an **existential over
  the type** ("any X") or a **freshly-introduced instance not yet secured**. First mention is `a`
  (new); every later mention is `the` / a pronoun (given) — the **new → given** chain, resolved through
  the referent chain.

**Demonstratives — `this` / `that`.** Deictic **definite** determiners that point: **`this`** (proximal)
→ the secure instance `^` (the pointed-to concept); **`that`** (distal) → a referent further off in
space or discourse. Identifiable *by pointing*.

**Pronouns — pro-forms.** A **pronoun** (it / they / he / she) is a **pro-form with no head noun of its
own**; it **resolves to its antecedent** — anaphoric (refers back, via the referent chain / discourse
deixis) or deictic (I / you, via person deixis). A pronoun is a **reference**, never a stored head.

**The givenness scale (which form to use).** Full noun phrase (*the red car*) → reduced (*the car*) →
demonstrative (*that car*) → pronoun (*it*) → zero. The more **accessible / given** a referent (Ariel's
Accessibility, Gundel's Givenness Hierarchy), the more **reduced** the form. New → indefinite full NP;
given → pronoun. The referent chain + discourse deixis supply the accessibility; the renderer picks the
form.

**Word-class — noun vs pronoun.** A **noun** names a concept directly (the head, placed in the hypernymy
— sccs-noun-creation). A **pronoun** has **no head noun**; it is a derived/indexical reference whose
anchor is its **antecedent** (discourse deixis) or a **participant role** (person deixis). Don't graft a
pronoun into the noun hypernymy — it isn't a kind, it's a pointer.

**What other skills inform this:**
- **sccs-naming** — the determinative `the` (definite) vs its absence (indefinite); the operator.
- **sccs-lexical-relations** — a pronoun's antecedent must match its **kind** (hypernymy) to resolve;
  coreference rides the kind tree.
- **sccs-instances** + referent chains — anaphora: the given referent a `the`/pronoun points back to.
- **sccs-construction** — the determiner is part of the **noun-phrase construction** (Det + N).
- **sccs-entity-system** — `I` / `you` are participant roles (person deixis), and the social anchors.

## Coding time vs reading time (the rule that matters)

An indexical resolves against the **reading** context, not the creation context. A `now` frozen at
creation time is a bug. Therefore an indexical **must be derived (resolve-at-read), never a stored
value**. A *record of when something was made* is a different, **stored** concept (`entry_time_stamp`)
— not an indexical; don't confuse the two.

## Deixis lint (enforced through the grammar gate's completeness / render passes)

- **DX1 — frozen indexical:** a concept that reads as indexical ("current", "now", "today", "here",
  "this", "my", "the above") but stores a fixed value → mark **derived**, bind it to its anchor.
- **DX2 — missing anchor:** an indexical with no declared deictic anchor (person / place / time /
  discourse / social) → declare it.
- **DX3 — wrong origo:** resolving against the creation context instead of the reading context →
  resolve at read.
- **DX4 — "my" baked in:** a token like `the_my_order` → rewrite as `the_order` whose host resolves to
  `the current user` (person deixis); "my" is indexical, never a stored qualifier (cf. the L7
  proper-name rule, sccs-naming).
- **DX5 — conflated social anchor:** a social indexical that resolves **status** through the authority
  lineage (or **authority** through a title) → split it: authority → the grant lineage, status → a
  title possession. Title ≠ access.
- **DX6 — indefinite as definite:** a **not-yet-identifiable** referent named with `the` / `^`
  (definite) → it is indefinite; introduce it with a new/existential reference first, then refer back
  with `the`.
- **DX7 — unresolvable pronoun:** a **pro-form** with no resolvable **antecedent** in the graph → bind
  it to its antecedent (discourse deixis) or a participant role (person deixis); a pronoun is never a
  head noun and is never grafted into the hypernymy.
- **DX8 — over-reduced reference:** a reduced form (a pronoun) used for a **low-accessibility** (not yet
  given) referent → use a fuller form (a definite noun phrase) until the referent is given.

## Procedure

1. Identify the indexical and its **deixis category** (person / place / time / discourse / social).
2. Bind it to the matching **deictic anchor** (the origo table above).
3. Model it as a **derived concept** (`referent 0`) whose prototype's `resolve` reads the anchor —
   store the rule (character), never the value (content).
4. Confirm it resolves at **read** time; run the deixis lint.

## Required output

The standard SCCS triple — **Concepts**, **Connections** (with `kind`), and a 2–4 sentence
**Summary** naming each indexical, its deixis category, its deictic anchor, and confirming it is
**derived (resolve-at-read)**. Use composite `(id, user_id)`; keep names self-documenting.
