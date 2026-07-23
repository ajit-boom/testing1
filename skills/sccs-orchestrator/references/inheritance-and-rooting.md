# Connection-prototype inheritance & branch-directed rooting

Reference detail for `sccs-trunk-prototype`. Covers the single-point-of-definition inheritance chain,
the populate-upward rule, and how a branch directs a trunk at its own appropriate root so that
cross-cutting queries fall out for free.

## The inheritance hierarchy (single point of definition)

Connection prototypes form a strict chain so that **every component is defined exactly once, at the
most-general node where it is shared**, and everything below inherits it rather than copying it:

```
the prototype of the prototype            root — universal: evaluation · obligation · cardinality
 └─ the prototype of the connection            all connections — of · to · kind · order
     └─ the prototype of the trunk   the trunk-overlay mechanics;
                                                 THE single inheritance point for connections
         └─ ^the prototype of <a connection branch>   only branch-specific overrides
```

- `the prototype of the connection` holds what is true of **any** connection (it has an `of`, a `to`,
  a `kind`, an `order`, and renders through them).
- `the prototype of the trunk` holds the **trunk-overlay mechanics** — how a branch
  elaborates upward to the root — and is the **single point** every connection branch inherits from.
- A branch carries only its own overrides (its specific `target`, `kind`, obligation).

Each of these prototypes is a **type** (`referent NULL`) — compositional, defined by its render-rule
components — not a `referent 0` instance.

## Populate by working upward

Start at a branch, then hoist each component to the highest node where it is shared:

- branch-specific → stays on the branch;
- common to all connections of that trunk → lands on `the prototype of the trunk`;
- common to *every* connection → lands on `the prototype of the connection`;
- universal render/obligation/cardinality → lives on the root.

The result is **one definition per component, inherited down** — nothing duplicated to drift out of
sync.

## Branch-directed trunk roots

A trunk's root is **not** a single fixed node. **A branch may create a trunk at its own appropriate
root**, chosen by peeling qualifiers off its own name down to the generalization it wants to join. A
branch may plant several trunks at several roots for several cross-cutting generalizations.

`^the prototype of <a connection branch>` refers up to whichever trunk root its branch directs,
defaulting to `the prototype of the trunk` but free to target a more specific appropriate
root when one is meaningful.

**Example.** `the red toyota model A car` peels `red` and `toyota` to direct a trunk at
`the model A car`, so the system ranges over *all* model-A cars of any manufacturer/color.

## Why a cross-cutting query falls out for free

"All model A cars of any manufacturer" is just a view rooted at the trunk `the model A car` that the
red-toyota branch — and every other make's model-A branch — directed a trunk at. No special query
construct is needed: **the rooting *is* the index.** The available roots are exactly the
generalizations the name already encodes (head-noun-last peel per `sccs-naming`; referent-chain peel
per `sccs-instances`).

## The grammar gate — incorporated once, at the branch

`sccs-grammar-testing` is incorporated **only at the branch concept** (the `of` side), never at every
node. From there it **uses the trunk to resolve upward to the root**, so a single attachment validates
the whole inheritance chain — the branch's own names first, then each inherited component it reaches
through the trunk, up to the root. The gate obeys the same single-point principle as the prototypes it
checks: one incorporation point, inherited, rather than a copy of the validator on every node.
