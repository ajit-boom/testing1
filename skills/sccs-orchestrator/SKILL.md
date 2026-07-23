---
name: sccs-orchestrator
description: Routes, sequences, and validates SCCS ontology work. Use first for any multi-step SCCS task spanning types, instances, entities, naming, prototypes, or the branch/trunk/target model.
---

# SCCS — Orchestrator

Routes an SCCS task to the right specialist and sequences multi-part work so the pieces are
built in the correct order with one consistent set of conventions. Shared spec:
`references/sccs-spec.md`. Full current-state synthesis (engine + this session's upgrade
layer): `SCCS_CURRENT_STATE.md` if present in the workspace.

## Precedence (read first)

The **original 2018 specification is the authoritative core engine.** The current model adds an
**upgrade layer that rides on top as a non-destructive overlay** — it never changes the schema.
Where engine and upgrade conflict, the engine wins; where the upgrade introduced new language
(`composite`, `componency`, `atom`, `branch/trunk/target`, the prototype), use it. The upgrade
is built *with* the engine, not beside it.

## The specialists

| Skill | Use it for |
|---|---|
| **sccs-engine-core** | **Ground truth — consult first.** The live two-table engine as it describes itself: the self-describing schema (`the_concept` / `the_connection` and their columns as rows), the **`category⟨type⟩`** assembly operator + `evaluate()`, the **`^` = the_secure_instance_of** marker, the **`^the_name=`** grounding, the confirmed leaf set, the `(id, user_id)` key, and the American-English root declaration. Reuse-before-create against real rows; never propose a second schema. |
| **sccs-composition** | **The heart — start here.** Assemble a whole from **ordered component connections** defined by its prototype; the **required parts are the boundary** of what must **travel together** as one unit (the share/transfer unit). Concept-side, and the primary **connection-side (trunk) composition**. The grammar gate sits **under** this. |
| **sccs-prototype** | **Defines the composition.** The **concept and its prototype** — the function contract (resolve, read, the 5 render stages, connect, compose, inherit, derive, convert) that turns an inert referent into **all upward functionality**; the 5-stage evaluation, location-statements, security-via-visibility, composition, derived-on-read. |
| **sccs-hypernym-placement** | The organic-growth function: **place a new noun into the live hypernymy** — find its nearest subsuming node, test the is-a + SNS reading + one-noun-one-leaf fit, reuse-not-duplicate, and graft it (front-qualifier / tail) so the tree grows by extension. |
| **sccs-noun-creation** | Creating/vetting the **head nouns** concepts are built on: placing a noun in the noun-hypernymy taxonomy, classifying it (common/proper/…), assigning its semantic properties, and the **noun lint** — the precursor that later interfaces to the type prototype. |
| **sccs-naming** | The **Semantic Naming System (SNS)**: name concepts/connections/tables/fields by the relation (`_` part / `_s_` possession / kind), head-noun-last off the hypernymy, components & composition into human data structures, and label/format **reporting facets**. |
| **sccs-application** | The SNS **application connector `of`**: `the <F> of the <X>` applies a function-concept's functions to an argument (e.g. `the prototype of the concept`); head-first grammar (inverse of possession), the **no-dangling-`of`** rule, and `of` vs possession vs part. |
| **sccs-ontology** | Graph mechanics — the prototype mechanism, the branch/trunk/target connection + the `the trunk` overlay, combination vs composition, the determinative `the`, the referent rule. (Naming itself → **sccs-naming**.) |
| **sccs-types** | Defining **types** (typological concepts, `referent_id` NULL): "The X" roots, left-to-right assembly, type-level schema/components, and the **category prototype** that binds them. |
| **sccs-instances** | Creating **instances** (`referent_id` = value / 0 / target): referring, compositional, hybrid; referent chains; integer-inline values; leaf resolution. |
| **sccs-entity-system** | Modeling **entities** (actors): the three types **individual (atom) / composite / program**, the composite's company·place·group branch-lenses, **role/kind branches of the entity** (`the student/company/employee entity`), the person hypernym, and the **access-control** model. |
| **sccs-units-of-measure** | Generating **units of measure** in both **metric** and **English/imperial** systems for a **dimension** (length, mass, volume, …), with conversion via a shared canonical base; measured-value instances. |
| **sccs-postal-address** | Country-parametric **postal/physical address**: the place hierarchy, address line 1/2/3 composition, the thoroughfare/sub-premise **classifications**, and per-country junction prototypes (order · obligation · postal-code format). |
| **sccs-event** | Modeling **events/actions** — a verb reified into a concept that *happens* (`the payment`, `the booking`): participant roles (agent/object/recipient via valence), time (`the_times`/`the_dates`), tense/aspect & modality (qualifier), the actor as an entity. A state describes; an event happens. |
| **sccs-commerce** | Modeling **commerce** — `the offer` (item + price + intent), `the price` as a **role** over `the monetary amount`, quantity-price bands, `the order`, payment agreements/installments, and inventory stock. Composes money (units), entities, and events. |
| **sccs-deixis** | Modeling **indexical (context-dependent) concepts** — `the current user`, `now`, `today`, `here`, `this`, `the above`, `my X` — that resolve against the **deictic center** (who = `user_id`, when = `entry_time_stamp`, where = tenant). The *character* (rule) is the prototype; the *content* is resolved on read, so an indexical is a **derived concept**, never a stored value. |
| **sccs-construction** | Modeling **constructions** — learned **form↔meaning pairings** on the schematic↔substantive continuum: open positions (typed variables) that bind fillers, **meaning carried by the pattern** not its fillers, placed in a **construction inheritance network** (subsumption). The **productive/schematic** end; argument-structure constructions and partially-filled templates. |
| **sccs-phraseology** | Modeling **fixed multi-word expressions** — idioms, collocations, light-verb units, formulae — by **compositionality × fixedness**. Idioms are **lexicalized** (stored whole, not decomposed); collocations are **usage preferences**; light-verb meaning sits on the noun. The **substantive/fixed** end; owns the **idiom principle** (stored unit before assembly). |
| **sccs-lexical-relations** | The **lexical-semantic relation family** that grounds the SNS connectors, **hypernymy as the head**: hypernymy/hyponymy (= subsumption / the kind tree), **meronymy** (= componency/containment/division, with subtypes), **synonymy** (= one concept + aliases), **antonymy** (= polarity/converse), polysemy/homonymy (= sense-link/split). The relation theory over `sccs-hypernym-placement`'s grafting. |
| **sccs-source-encoding** | The **workflow that builds the data solid from authoritative sources** — a dictionary, business reference, industry glossary, or a client's product catalog → clean, **linkable, expandable** concepts. Two modes: **backbone** (general sources → the reusable ~99% spine) and **client/domain** (a client's products → thin grafts). Reuse before create; never fork the spine. Orchestrates noun-creation, hypernym-placement, lexical-relations, event, and grammar-testing. |
| **sccs-trunk-prototype** | The **connection-side** prototype — `the prototype of the connection` and its trunk node — the dual of the concept-side master: how a *relation* resolves and renders (branch/trunk/target/kind, the kind→reads-as grammar), and **branch-directed trunk roots**. The connection-side counterpart to **sccs-prototype**. |
| **sccs-grammar-testing** | **A connection under `sccs-composition`:** checking that a composition is well-formed (the naming lint L1–L15, English normalization) and **complete** against its prototype (obligation/cardinality). Realized as a **program entity** (`^the grammar program entity`); the gate the orchestrator runs **after composing**, before emitting. **Flags, never rewrites,** any change that would alter meaning. |

## Routing

- The **concept and its prototype**, the function contract, the 5-stage render, location-statements,
  what a concept "does," security-via-visibility, derived-on-read → **sccs-prototype** (start here).
- Creating or vetting a **head noun**, placing a word in the noun-hypernymy, or assigning noun
  properties/categories → **sccs-noun-creation** (do this *before* assembling the type).
- Placing/grafting a new noun into the existing hypernymy, where a concept belongs, growing the
  tree organically, or avoiding duplicate/orphan nouns → **sccs-hypernym-placement**.
- Naming or renaming anything, the SNS, the connector layer, self-documenting names, table/field
  names, or reporting facets → **sccs-naming**.
- The connector **`of`** / applying one concept's functions to another / `the prototype of …` / a
  derived concept, or checking an `of` name is well-formed → **sccs-application**.
- The branch/trunk/target overlay, combination vs composition, the extensible leaf set, or referent
  mechanics → **sccs-ontology** (the prototype's function contract → **sccs-prototype**).
- A "kind of thing"/recipe/schema with nothing bound → **sccs-types**.
- A concrete thing, a bound value, a referent chain → **sccs-instances**.
- Actors that change data, permissions, roles, who-can-see/manipulate, companies/places/groups/
  programs, granting/revoking/delegating, **roles like patient/student/client/employee** →
  **sccs-entity-system**.
- Mixed ("model this domain in SCCS") → use the sequencing below.
- Units of measure, dimensions, metric/English/imperial, or converting measurements →
  **sccs-units-of-measure**.
- An **address / postal / mailing address**, ZIP/postcode, or a country-region-locality hierarchy →
  **sccs-postal-address** (sequence after sccs-entity-system; coordinates via sccs-units-of-measure).
- An **event / action / occurrence / transaction** — something that *happened* with an actor and a
  time, a verb reified, or tense/aspect/modality on a happening → **sccs-event**.
- **Commerce** — offers, prices/cost/fee, orders/carts/checkout, invoices/payments/installments,
  catalogs, or inventory/stock → **sccs-commerce** (composes sccs-units-of-measure money,
  sccs-entity-system actors, and sccs-event acts).
- An **indexical / context-dependent reference** — `the current user`, `now`, `today`, `here`,
  `this`, `the above`, `my X`, or any value that must **resolve at read time** against the deictic
  center (who/when/where) → **sccs-deixis**.
- A **productive pattern / form-meaning construction** — argument-structure constructions, a
  partially-filled template ("the X-er the Y-er"), open positions binding typed fillers, or
  **meaning carried by the pattern** → **sccs-construction** (the schematic end).
- A **fixed multi-word expression** — an idiom, set phrase, collocation, light-verb unit, or
  formula; anything **stored whole rather than assembled** → **sccs-phraseology** (the fixed end).
- A **sense relation between concepts** — is-a-kind-of (hypernymy/hyponymy), part-whole (meronymy),
  same-sense (synonymy), opposite (antonymy), or one-form-many-senses (polysemy/homonymy); choosing
  or checking which relation a connector encodes → **sccs-lexical-relations** (hypernymy is the head;
  grafting itself → **sccs-hypernym-placement**).
- **Encoding an authoritative source into the database** — a dictionary, business/management book,
  industry glossary, or a client's product catalog → clean, linkable, expandable concepts; building
  the **backbone** from general sources or **grafting a client/domain** onto it → **sccs-source-encoding**
  (it runs the others as its pipeline).

## Sequencing

0. **Ground in the engine first.** Check **sccs-engine-core** — does the concept (or its hypernym,
   leaf, or connector) already exist as a real row? Reuse before create; anchor names to
   `category⟨type⟩`, endpoints to `of`/`to`, instances to `^`. Never propose a second schema.
1. **The concept and its prototype first.** Settle what each concept is (category + type + referent)
   and give its category a **prototype** — the function contract (resolve · the 5 render stages ·
   connect · compose · inherit · derive · convert). A bare grouping is inert without it; everything
   below is built on it (**sccs-prototype**).
2. **Head nouns — gate hypernymy vs characterization first.** Before placing anything in the kind
   tree, apply the **sortal test** (sccs-lexical-relations / the kind-vs-type rule): is the candidate
   *a kind of* its parent (**sortal → place it** via sccs-hypernym-placement) or a *role / quality*
   (**characterization → keep it OUT of the tree**, route to a describing qualifier or an entity
   role-branch)? Only sortal kinds are placed/classified (**sccs-noun-creation**). This gate runs
   **before** placement so a role/quality is never grafted and then ripped back out — the rest of the
   linguistic layer (step 6) stays a flexible overlay, but this one check gates the order, because it
   decides *where the noun goes*.
3. **Types.** Establish "The X" roots and sub-types; each `category_id` chain climbs to `the`.
4. **Instances.** `category = The instance of`, the right referent kind, head-anchored chains
   (specific → general → raw value).
5. **Entities/roles/access last.** Build the entity as a compositional instance; set the subtype
   only at the **individual-vs-composite** line; grow **role/kind branches** off the entity trunk;
   put grants on the **role-type**; apply the access rules.
6. **Linguistic-discipline layer (overlay on any of the above, as the material calls for it).** These
   don't gate the build order; they engage when a name or relation has the relevant property:
   - a **sense relation** between concepts (kind / part / same / opposite / one-form-many-senses) →
     **sccs-lexical-relations** (hypernymy is the head; grafting → sccs-hypernym-placement);
   - a **productive form-meaning pattern** with open positions → **sccs-construction**; a **fixed
     multi-word unit** stored whole → **sccs-phraseology**;
   - an **indexical / reference form** (`the current user`, `now`, `here`, `a` vs `the`, a pronoun)
     that resolves at read time → **sccs-deixis**.

## Shared validation pass

- **Referent rule:** every `referent_id` is intentionally NULL (type) / 0 (compositional) /
  small integer = the value itself (inline) / row-id = pointer to a leaf table.
- **Root & assembly:** all types are "The X"; names assemble left → right (head noun last);
  instances anchor on the head noun and chain specific → general.
- **Linguistic-discipline checks (when applicable):** an **indexical / reference form** is **derived
  (resolve-at-read)**, never a frozen value (deixis DX1–DX8: definiteness, pronoun-antecedent); a
  **fixed/idiomatic** expression is **lexicalized** (stored whole, not assembled — phraseology); a
  **productive pattern** carries its meaning on a construction prototype (construction); a **sense
  relation** is the right family member and **linked, not duplicated** (lexical-relations LR1–LR5,
  hypernymy = subsumption).
- **Naming lint (hard gate):** every concept/connection/table/field name must pass the **L1–L15
  naming lint in sccs-naming** before emission — re-derive each name from the rules, never carry a
  prior artifact's token forward unchecked. Key reject cases: underscore inside a type token; head
  noun that mis-types the leaf; possession host ≠ the owning concept; redundant qualifier; ordered
  relation missing `order`; proper name baked into a type token; legacy tokens. Connection types are
  named by the relation — possession `from_s_to`, or for structural belonging the **kind** concept
  (`containment`, `division`) — never by doubling endpoints (`the_place_place` → `containment`).
- **Overlay:** the branch/trunk/target model is carried by a `the trunk` connection-type on the
  **unchanged** schema; `of_the_concepts_id` is retained (the trunk sits on the `to` side); the
  overlay is additive and the original stays authoritative.
- **Tenant keys / lifespan:** composite `(id, user_id)` everywhere; never hard-delete (terminate
  via `termination_datetime`). **Ownership is the `user_id`**, not a modeled connection.
- **Prototype completeness:** every report-bearing concept has a **prototype** whose function tiers
  apply (resolve · the 5 render stages · connect · compose · inherit · derive · convert); each upward
  output the concept must produce reduces to those functions, or the prototype is incomplete (see
  sccs-prototype).
- **Measured quantities (R1):** a bare `the_numbers` is only a dimensionless count; any quantity
  meaningless without a unit (price→currency unit, duration→time, size→data, coordinate→angle) is a
  measured value `magnitude + unit` (see sccs-units-of-measure).
- **Stored vs derived:** any view/recap/smart-collection/search is a **derived** query concept,
  flagged as such and evaluated on read — never stored data (see sccs-instances).
- **Entity/access (when present):** three types only (individual atom / composite / program);
  company/place/group are composite branch-lenses; role/kind specializations (student, patient,
  company, employee) are **branches of the entity, not components**; entity is
  the access unit; grants on role-types; default-deny; title ≠ access; delegation scoped,
  non-escalating, with no-upward-revocation and root inviolability. Creation stamps the creator as
  `user_id` (ownership); access is an explicit, capped, recursive grant (authority via the
  `delegated_by` lineage). The **branch is the unit of sharing/transfer**. The three-type model
  governs — older five-subtype/employee-junction encodings are legacy and should be re-validated.

## Required output format

Return the standard SCCS triple once for the combined result: a **Concepts** table, a
**Connections** table, and a 2–4 sentence **Summary** naming the root types, the domain, any
referent chains, and (if applicable) the entities, roles, and what each role grants. Group tables
by stage when large; keep one column layout. State assumptions inline.
