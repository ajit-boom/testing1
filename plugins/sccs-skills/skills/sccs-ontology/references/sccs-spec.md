# SCCS spec — reconciled data dictionary

Shared reference for the skills that defer field-level detail to `references/sccs-spec.md`
(`sccs-types`, `sccs-instances`, `sccs-ontology`, `sccs-entity-system`, `sccs-orchestrator`).

> **Precedence.** The original 2018 SCCS engine is **authoritative**. This file is the overlay's
> reconciled, current-understanding **summary** of the engine's data model — enough to author and
> validate against. Where exact legacy column types differ from what is described here, the original
> engine governs. Always say **concept**, never "node."

## The two tables

SCCS stores meaning as a graph in two relational tables, both keyed on a composite **`(id, user_id)`**:

- **`the_concepts`** — nodes.
- **`the_connections`** — reified edges (each edge is a row, so it can itself be qualified, dated, and
  reasoned over).

## The concept model

A concept is **`category` + `type` + `referent`** (each carrying its `user_id`), with a
`character_value` where a value sits inline.

**The referent rule** (type-vs-instance axis; *not* a compositionality axis):

| `referent` | meaning |
|---|---|
| `NULL` | a **type** — a compositional schema/recipe, no value of its own. **All prototypes are types ⇒ `referent NULL`.** |
| `0` | a compositional **instance** — its value *is* its filled-in component connections |
| small integer | the **value itself**, inline |
| row-id / target | a **referring instance** — a pointer into a leaf table or another concept |

Both a type (NULL) and a `referent 0` concept are *compositional*; the difference is **type vs
instance**.

## The connection model

A connection carries:

- **`of`** — the subject concept (the branch / `from` side);
- **`to`** — the object concept (the target);
- **`type`** — the generated, self-documenting name (the relation);
- **`order`** — position within an ordered composition;
- **`kind`** — `component` (`the_x_y`, "the X Y") · `possession` (`the_x_s_y`, "the X's Y") ·
  `classification` (`the_x_type`, "the X is a Y").

The **branch / trunk / target** generalization is carried by a `the trunk` overlay row on this same
unchanged schema (the trunk sits at the most-general node; `of_the_concepts_id` is retained with the
trunk on the `to` side). See `references/inheritance-and-rooting.md`.

## The determinative and hypernymy

Every concept name is determinative — **`the X`** — which lets the name function as a stable referent
(`the` is the root, concept 0). Nouns live on an **is-a hypernymy**, head noun last; new nouns are
grafted into the live hierarchy, never duplicated.

## Leaf tables (7)

The SNS's original 10 data types reconcile to **seven leaf tables**:
`the_characters`, `the_texts`, `the_numbers` (floats/decimals; integers inline), `the_times`,
`the_dates`, `the_documents`, `the_UUIDs`. **`the_documents` is the gatekeeper** — `sounds`,
`images`, `videos` are document types resolved through it via specialty media servers (the row holds
the locator; the bytes live on the server). Leaf tables are **extensible**: a value earns its own leaf
when it has a distinct canonical format.

## The access layer (structural)

Entities are the actors that change data and sit near the access layer (`the_users`, `the_passcodes`
in the engine). Access grants are `(entity, operation)`; the **entity is the access unit**;
**default-deny**; **composite-as-root-of-trust** with a responsible individual; ownership is the
`user_id`. Exact engine column types for the access tables are governed by the authoritative spec.

## Worked example (a referring instance)

```
the dollar                  category=the, type=dollar,  referent=NULL          (a type)
the dollar's country        connection: of=the dollar, to=^the USA entity,
                            type=the_dollar_s_country, kind=possession
^the USA entity             category=the, type=USA entity, referent=0          (a compositional instance)
```

Read top-to-bottom, every connection reads back as an English clause — the name *is* the meaning.
