# The Connector Senses of "of" and "to" — disambiguating English prepositions into SCCS connectors

*A deep dive into the two most structural English prepositions, grounded in recognized sources, and
the rule that resolves their ambiguity in SCCS: **one English preposition fans out into several
distinct connector concepts — pick the sense, never the bare word.** Companion to `sccs-naming` and
`sccs-application`. See also `linguistic-foundations.md`.*

## How many senses? (sources)

- **"of"** — *The Preposition Project* (Litkowski & Hargraves, from the **Oxford Dictionary of
  English**) lists **18 contemporary senses**; it is the **second-most-frequent word in English** and
  among its most polysemous. The **OED**'s full historical entry (with obsolete/rare senses) runs to
  **dozens** — a count in the 40–66 range is reasonable for the exhaustive historical list.
- **"to"** — TPP lists **17 senses**; the OED entry is likewise very large.

The exact number depends on the source and on how finely senses are split (sense delimitation is a
known open problem in lexical semantics). The point for SCCS is **not** the count — it is that a
single high-frequency preposition is **massively polysemous**, so using the bare word as a connector
is ambiguous. Each sense must map to its **own** connector concept.

## The sense families of "of" (the genitive relations), and where each routes in SCCS

English "of" is the chief marker of the **genitive/oblique** relation. Its sense families (standard
grammar — possessive, partitive, subjective/objective, origin, material, descriptive, appositive,
reference, separative) route to **different** SCCS connectors:

| "of" sense family | English example | SCCS connector | why |
|---|---|---|---|
| **objective genitive / argument** | the fear **of** the enemy; the prototype **of** the concept | **`of` = branch-root** (the canonical SCCS `of`) | the complement is the **argument** the head roots in — SCCS's structural `of` |
| **possession (alienable/relational)** | the leg **of** the table; the music **of** Beethoven | **`_s_` possession** (`the_table_s_leg`) | possession is `_s_`, never SCCS `of` |
| **partitive / part-of-whole** | a part **of** the state; some **of** the food | **component / containment** (`the_x_y`, kind = part/containment) | a part is structural componency |
| **material / composition** | a wheel **of** cheese; made **of** wood | **composition** | the whole is composed of the parts |
| **subjective genitive / agent** | the love **of** God (God loves) | **the agent role** (event participant) | the complement is the **agent**, an event role, not a branch |
| **origin / source** | men **of** Rome; the sea's fish | **`from` / source** (or branch-root if it is the argument) | source/origin connector |
| **descriptive / quality** | a man **of** honour | **characterization** (a quality) → describing qualifier | non-sortal quality, not a kind |
| **appositive / identity** | the Isle **of** Man; the city **of** Rome | **identity / naming** (the X named Y) | apposition, not a relation between two things |
| **reference / about** | a study **of** birds | **about / topic** connector | the topic the head concerns |
| **measure / quantity** | a pound **of** flour; a dollar's worth | **quantity + unit** | a measured quantity |
| **separation / deprivation** | rid **of**, free **of**, cure **of** | **without / minus** (ablative) | the separative/ablative sense |

**SCCS `of` is one of these — the objective-genitive / argument / branch-root sense.** All the others
are deliberately **routed away** (most importantly: possession → `_s_`, composition → the composition
operator, agent → an event role). This is preposition-sense disambiguation done once, at design time.

## The sense families of "to", and where each routes

English "to" is the chief marker of the **dative/allative (goal)** relation.

| "to" sense family | English example | SCCS connector | why |
|---|---|---|---|
| **goal / destination** | go **to** London; the connection **to** the concept | **`to` = target** (the canonical SCCS `to`) | the complement is the **destination** — SCCS's structural `to` |
| **recipient / dative** | give **to** her | **the recipient role** (event participant) | a thematic role on an event, not a structural target |
| **direction** | point **to** | **direction** (often the same as target) | directed-toward |
| **purpose** | (in order) **to** win | **purpose** connector | telic / goal-state |
| **result / extent** | soaked **to** the skin; **to** my surprise | **extent / result** | endpoint of a scale |
| **comparison / proportion** | superior **to**; 3 **to** 1 | **comparison / ratio** | relates two values |
| **conformity / standard** | **to** my taste | **standard** connector | conformity to a norm |
| **attachment** | tied **to** | **attachment** (a relation) | bound-to |
| **time** | quarter **to** five | **time offset** | temporal "to" |

**SCCS `to` is the goal/destination/target sense.** Recipient, purpose, comparison, etc. become their
own roles/connectors.

## The clean rule (this is the connector law)

1. **`of` (SCCS) = branch-root** — the objective-genitive / argument / source the head sits on. It is
   reused as `of_the_X` across every head (connection · composition · prototype), exactly as the
   engine stores it (`the_prototype⟨of_the_concept⟩`, `the_connection⟨of_the_concept⟩` share one
   `of_the_concept`).
2. **`to` (SCCS) = target** — the goal/destination endpoint; the second endpoint of a **binary**
   connection only. Applications/compositions are **unary** (of-only).
3. **Never use the bare word "of" or "to" ambiguously.** If the intended relation is possession,
   composition, agent, partitive, recipient, purpose, comparison, etc., use **that** connector — not
   the structural `of`/`to`. The grammar gate flags a connector whose `of`/`to` sense is unspecified
   (lint **L14**).

A connection therefore reads as a directed clause: **of [the branch it roots in] … to [the target it
points at]** — the `of` end and the `to` end, each one specific sense, not the whole ambiguous
preposition.
