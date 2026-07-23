# Engine Genesis Rows — the live row map (stage-13 export)

*Companion reference for `sccs-engine-core`. Read directly from
`concept_connection_system_ontology-Artisana735_stage13.xlsx` (`The_concepts` sheet). Of 79,997
populated ids, **3,855 carry a `full_value`** (the named ontology); the rest are leaf-backed value
rows and reserve to 100,002. Sheet columns 12–59 are SQL/PHP/JS code-generation templates, not
ontology. Notation: `name = category⟨type⟩`.*

## Layer 0 — bootstrap (the strange loop)

| id | name | assembly | note |
|---|---|---|---|
| 1 | the | `^⟨the_category⟩`, ref→"the" | root; carries `(.taxonomy)=((American:English…))` |
| 2 | secure_instance | `secure⟨instance⟩` | |
| 3 | the_secure_instance | `the⟨secure_instance⟩` | |
| 4 | the_secure_instance_of | `the_secure_instance⟨of⟩` | rendered as the marker **`^`** |
| 5 | the_category | `the⟨category⟩` | typed-by, then categorises `the` — the fixed point |

## Layer 1 — the self-describing schema

| id | name | assembly |
|---|---|---|
| 6 | the_concept | `the⟨concept⟩` |
| 7 | the_concept_id | `the_concept⟨id⟩` |
| 8 | the_concept_category | `the_concept⟨category⟩` |
| 9 | the_concept_type | `the_concept⟨type⟩` |
| 10 | the_concept_referent | `the_concept⟨referent⟩` |
| 11 | the_type | `the⟨type⟩` |
| 12 | the_name | `the⟨name⟩` |
| 13 | the_referent | `the⟨referent⟩` |
| 14 | of_the_concept | `of⟨the_concept⟩` |
| 15 | to_the_concept | `to⟨the_concept⟩` |
| 16 | the_connection | `the⟨connection⟩` |
| 17 | the_connection_id | `the_connection⟨id⟩` |
| 18 | the_connection_of_the_concept | `the_connection⟨of_the_concept⟩` |
| 19 | the_connection_type | `the_connection⟨type⟩` |
| 20 | the_connection_order | `the_connection⟨order⟩` |
| 21 | the_connection_to_the_concept | `the_connection⟨to_the_concept⟩` |
| 22 | the_composition | `the⟨composition⟩` |
| 23 | the_order | `the⟨order⟩` |
| 24 | the_combination | `the⟨combination⟩` |
| 25 | the_composition_of_the_concept | `the_composition⟨of_the_concept⟩` |
| 26 | the_prototype_of_the_concept | `the_prototype⟨of_the_concept⟩` |
| 27 | the_id | `the⟨id⟩` |

## Layer 2 — identity & tenancy

`the_user` (28) + `the_user_id` (29) / `the_user_type` (30) / `the_user_concept` (31) /
`the_user_system` (32); `the_system` (33); `the_passcode` (34) + user/type/concept;
`the_function` (39); the composite key — `the_id_combination` (40), `id,user_id` (43),
`id,parent_user_id` (46), `system_id,user_id` (186).

## Layer 3 — value / leaf layer

`the_integer` (48) · `the_character` (49) · `the_datetime` (50) · `the_characters` (51) ·
`the_text` (52) · `the_number` (53) · `the_time` (54) · `the_date` (55) · `the_document` (56) ·
`the_sound` (57) · `the_image` (58) · `the_video` (59) · `the_uuid` (60) · `the_ipv6` (67) ·
`the_password` (69) · `the_users` (91) · `the_url` (141). **`the_document` is the gatekeeper** for
sound/image/video.

## Layer 4 — prototype

`the_prototype_of_the_concept` (26) · `the_prototype` (96).

## Layer 5 — domain & rendering vocabulary (ids 142–9,999, ~1,760 named)

Entities and contact/network domain: `the_entity` (286), `the_person`, `the_company`, `the_email`,
`the_http`, `the_imap`, `the_host`, `the_street`, `the_city`, `the_zip`, `the_job`, `the_address`,
`the_listing`, `the_unit`, `the_url`. **~1,050** concepts typed `the_html_tag` / `the_tag` /
`the_html_attribute` / `the_attribute` — the markup the engine renders into, modelled as concepts.

## Layer 6 — name grounding (ids 10,000–11,999, 2,000 instances)

`^the_name=the`, `^the_name=concept`, `^the_name=category`, `^the_name=type`, `^the_name=referent`,
`^the_name=of`, `^the_name=to`, `^the_name=connection`, `^the_name=order`, … and the leaf names
`^the_name=the_characters`, `^the_name=the_texts`, `^the_name=the_numbers`, … Each binds a token
string into the `the_characters` leaf. The names of the schema concepts are themselves stored as
data.

## The self-reference set (concepts that point to the tables/fields)

Schema: `{1, 5, 6, 7, 8, 9, 10, 11, 13, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 96}`.
Binders: `{4, 14, 15}` + the preposition concepts `of`, `to`.
Grounding: the `^the_name=` block (Layer 6).

## Language connectivity — the five mechanisms

1. Metalanguage declared at id 1 (American English).
2. The assembly operator `category⟨type⟩` + `evaluate()`.
3. Preposition-named endpoints `of_the_concept` / `to_the_concept`.
4. Field vs classification share `⟨⟩`, split by the inner token's nature.
5. `^the_name=` binds tokens to characters (the token-to-surface bridge).

## Caveats

Stage-13 snapshot; ids/counts from this file. The ~76k blank-`full_value` rows are instance/data and
were not enumerated. A few `full_value` cells were stray codegen fragments (e.g. an `IF(...)` cell)
and are noise, not concepts.
