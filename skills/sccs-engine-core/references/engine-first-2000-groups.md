# Engine — the First 2,000 Named Rows: Groups, the Character Set, and Discrepancies

*Companion reference for `sccs-engine-core`. Read from the stage-13 export. The genesis + first 2,000
named rows are the **enforced core** (see SKILL.md). Of ids 1–2000, **1,836 are named** — 1,263
instances (`category = ^`), 205 types (`category = the`), 368 field/value rows. **Treat the export as
imperfect**: the discrepancy register below records where it drifts from the model, and the model
wins.*

## 1. The group map (contiguous id bands)

| id range | group | role | utility |
|---|---|---|---|
| 1–5 | **bootstrap** | types | determinative `the`, instance marker `^` (= `the_secure_instance_of`), the language root (American English), `the_category` |
| 6–27 | **self-describing schema** | types | `the_concept` + 4 columns (7–10), `the_connection` + 5 columns (17–21), composition / combination / order / `the_prototype_of_the_concept` — the self-describing core |
| 28–47 | **identity & tenancy** | types | `the_user` / `the_system` / `the_passcode` / `the_function`; the `(id,user_id)` / parent / system composite-key concepts |
| 48–63 | **value senses (leaves)** | types | integer · character · datetime · the leaves (`the_characters`…`the_uuid`) + media via `the_document` → sound/image/video; first heads (code, title, attachment, email) |
| 64–369 | **domain heads + field stubs** | types + fields | contact/network/entity heads (`the_email`, `the_http`, `the_host`, `the_street`, `the_entity`, `the_person`, `the_company`…) and their generic fields; the `type=id` run (71–80) and `type=data` run (81–90) |
| 370–507 | **HTML attributes — set A** | instances `^the_html_attribute` | the HTML / `data-*` attribute vocabulary (value in the **referent**) |
| **513–767** | **THE CHARACTER SET (ASCII/Latin-1)** | instances `^the_character` | the system **alphabet**, byte-indexed: **referent id = byte offset 1–255**, referent value = the character (9=`\t`, 10=`\n`, 32=space, 48=`0`, 65=`A`, 97=`a`, 255=`ÿ`). Every name ultimately resolves to these. |
| 768–959 | **HTML tags** | instances `^the_html_tag` | the HTML tag vocabulary `a, abbr, address, applet, area, …` (name in the referent) |
| 960–1003 | **HTML attributes — set A (cont.)** | instances `^the_html_attribute` | remainder of the HTML attribute set |
| *1004–1129* | *— gap —* | — | no named concepts |
| 1130–1321 | **tags — set B** ⚠ | value rows `the_tag` | a **second, parallel** tag vocabulary (full value = the tag string) mixed with XML/SVG namespace tokens (`xmlns`, `xmlns:fb`, `xmlns:xlink`) — **duplicates 768–959** |
| 1323–1824 | **attributes — set B** ⚠ | value rows `the_attribute` | a **second, parallel** attribute vocabulary incl. SVG attrs (`clip-path`, `cx`, `cy`, `crossorigin`) — **duplicates 370–507/960–1003** |
| 1825–1880 | **app/domain concepts** | mixed | http / entity / listing / programming / concept-editor app concepts |
| 1881–1989 | **derived / computed** | `evaluate()` | concepts computed on read (109) — never stored data |
| 1990–2019 | **discriminators / transition** | `the_concept_type` | type-discriminator values |

## 2. The character set, placed (ids 513–767)

This is the answer to "ASCII characters loaded into the first concept with a byte offset." The
**character leaf** (`the_characters`, id 51) is populated by 255 `^the_character` instances whose
**referent id is the byte offset** (1–255) and whose referent value is the glyph. It is the **surface
alphabet**: the `^the_name=` tokens (the name-grounding layer) and every rendered string are
sequences over this set. Spot-check: `32→" "`, `48→"0"`, `65→"A"`, `97→"a"` — standard ASCII;
128–255 follow Latin-1.

Group placement: it belongs in the **value/leaf layer** (it *is* the content of `the_characters`),
sitting logically beside id 51 even though the instances were loaded later at 513–767.

## 3. Type vs instance — the `h1` question

`h1` (like every tag) is stored in the export as a **flat instance** `^the_html_tag` with the name
"h1" in the referent — a bare enumeration member. Whether that is right depends on one test:

> **The type/token test: will distinct occurrences of `h1` ever need to differ from each other?**

- **`h1` as an instance (enumeration member).** Correct when you only need the *controlled
  vocabulary* — the finite, closed HTML tag set — and never instantiate a particular `<h1>`. Simple,
  finite, matches a spec. *Cost:* you cannot carry per-occurrence data (this `<h1>`'s text, class,
  order) without inventing a further instance layer; and `h1` cannot be sub-typed.
- **`h1` as a type (a sub-kind).** Correct when you render documents where each `<h1>` element is a
  distinct object. `the h1 tag` **is a kind of** `the html tag` (sortal subsumption, referent NULL);
  each rendered element is then `^the h1 tag` (referent 0) carrying its own content/attributes/order.

**Ruling (the layered answer).** Model the tag *name* as a **sub-kind type** —
`the h1 tag` is-a-kind-of `the html tag` — and the rendered element as an **instance** `^the h1 tag`.
The export's flat-instance choice is acceptable for a static lookup but is **under-modeled for a
rendering engine**, which needs per-occurrence instances. This is the kind-vs-type discipline applied:
`h1` is a **sub-kind** (subsumption into the kind tree), not merely a value.

**General rule to encode:** *a controlled-vocabulary member you only ever reference is an instance; a
member you instantiate per occurrence or sub-type further is a type (a sub-kind). When unsure, prefer
the sub-kind type — you can always instantiate a type, but you cannot sub-type or render-per-occurrence
a bare instance without rework.*

## 4. Discrepancy register (the export is not perfect; the model wins)

| # | discrepancy | evidence | resolution |
|---|---|---|---|
| **D1** | **Leaf `data` column is contaminated** in ids ~370–1824 with an unrelated word-series — hypernymy nouns (animal, plant, primate, embryo→deceased), titles (mr, mrs), DB index names (`the_sounds_..._index`), SVG attrs — aligned by spreadsheet row, not by concept | the_character rows show `leaf=animal/plant/…`; the_tag rows show `leaf=the_dates_..._references` | **The authoritative value is the `referent`, never column 11** in this region. Treat col 11 there as a paste artifact. |
| **D2** | **Duplicate tag & attribute vocabularies** — `^the_html_tag` (768–959) vs `the_tag` (1130–1321); `^the_html_attribute` (370–507/960–1003) vs `the_attribute` (1323–1824) | two parallel id bands of the same domain, one as `^` instances of a type, one as flat value rows | **Reconcile to one** model (the `^the_html_tag` instance form, promoted to sub-kinds per §3). Set B is redundant drift. |
| **D3** | **`h1` (every tag) modeled as a flat instance, not a sub-kind** — the vocabulary member and the per-occurrence element are conflated | §3 | Promote tag names to **sub-kind types**; render occurrences as instances. |
| **D4** | **Stray `full_value` / codegen noise** — e.g. id 35 `data-target-atc` for `the_passcode_id`; ids 372/374 `full=None` | individual rows | Ignore as paste/codegen noise; the `category⟨type⟩` assembly is authoritative, not the stray cell. |
| **D5** | **`the` numbered row 1, not 0** | export id 1 = `the` | Numbering convention; confirm against live DDL. Not a semantic difference. |

## 5. Why this matters

The first 2,000 rows are the **bedrock** — the schema, the alphabet, the leaves, the connectors, the
controlled HTML vocabulary. Enforcing reuse against them (SKILL.md) stops new work from re-minting a
character, a leaf, a connector, or a tag that already exists. But D1–D3 show the bedrock has paste-era
drift in the vocabulary region; the enforcement therefore keys on the **`category⟨type⟩` assembly and
the `referent`**, never on the contaminated leaf column.
