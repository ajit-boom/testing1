# Linguistic Foundations of the SNS (grounding + the irregular tail)

*Why the Semantic Naming System works, in the terms of established linguistics — and where natural
language exceeds it. Companion to `sccs-naming`. Full validation in the project pack's
`07_LINGUISTIC_VALIDATION.md`.*

## The SNS is built along the grain of English structure

- **Head noun last** (`the big cat`) = the **Right-Hand Head Rule** of compounding (Williams). English
  compounds are head-final.
- **`of` application head-first** (`the F of the X`) = English **PPs are head-initial**. The SNS
  correctly mixes head-final morphology with head-initial syntax — exactly as English does.
- **Connectors with valence; edges as clauses** = **dependency/valency grammar** (Tesnière), **Case
  Grammar** (Fillmore), thematic roles (FrameNet, VerbNet/PropBank). `of` = genitive/source, `to` =
  goal/dative.
- **kind vs type** = **sortal theory** (Strawson, Wiggins, Gupta); subsumption vs characterization =
  substance vs accident. The noun hypernymy is a **WordNet**-style kind tree.
- **referent NULL = type, `^` = instance** = **type/token** (Peirce); **sense vs reference** (Frege).
- **the prototype** = **Generative Lexicon qualia** (Pustejovsky: formal/constitutive/telic/agentive).

## The irregular tail — three markers the SNS must carry

Natural language is not fully compositional. Three escape hatches keep the regular core clean while
admitting the irregular remainder:

1. **Lexicalized concepts (idioms / constructions).** A concept whose meaning is **not** derivable
   from its parts ("kick the bucket", "by and large"). Mark it **lexicalized**: it is named and
   stored but **not decomposed** — its referent is a stored opaque sense, never an assembled
   `category⟨type⟩` derivation. (Construction Grammar, Goldberg.) Do not lint a lexicalized name for
   compositional well-formedness.

2. **Regular polysemy (sense links).** One English **form**, several **related** senses ("email" =
   address vs message; "attachment" = file vs affect; "book" = object vs content). This is **not** a
   token collision to split blindly — it is one form with linked senses. Model it as **a dot-object /
   sense link**: keep the shared head and relate the senses (`the_email` → `…_s_address_sense`,
   `…_s_message_sense`), rather than two unrelated concepts. The grammar gate routes a *regular
   polysemy* clash here; only an **accidental** clash (unrelated meanings sharing a spelling) is split.

3. **The "prototype" terminology guard.** SCCS **prototype** = the qualia/function contract. It is
   **not** the cognitive-science **prototype** (Rosch: graded category membership / best exemplar).
   Never import graded-membership semantics from the word "prototype."

## What the SNS does not, and cannot, capture (boundaries — document, don't "fix")

- **Vagueness/gradience** (sorites; "is a penguin a bird?") — a crisp sub-kind tree has no graded
  membership.
- **Indexicality/deixis** ("I/here/now/this") — needs a context model (Kaplan: character vs content).
- **Quantifier scope, intensionality, negation** ("every", "believes that", "seeks a unicorn") — needs
  a logical-form layer (Montague), above the ontology.
- **Metaphor/blending** ("grasp an idea") — needs cross-domain mapping (Lakoff & Johnson; Fauconnier &
  Turner).
- **Cross-linguistic relativity** — American English at the root carves the world language-particularly;
  lexical gaps (*Schadenfreude*, evidentiality) have no 1:1 image (weak Sapir–Whorf).
- **Symbol grounding** — `^the_name=` grounds tokens in **characters**, not the world; SCCS is
  intralinguistic, like a dictionary (Harnad's grounding problem is unsolved here, as in all such
  systems).

**Bottom line:** the SNS faithfully matches the **regular, compositional, sortal, predicate-argument
core** of English. A *perfect* 1:1 match is impossible; the achievable and largely-realized goal is a
**canonical core + explicit markers (lexicalized, sense-link) for the irregular tail.**
