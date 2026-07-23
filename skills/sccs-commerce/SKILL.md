---
name: sccs-commerce
description: Model commerce: the offer (item + price + intent), price as a role over the monetary amount, orders, payment agreements/installments, inventory. Use for offers, prices, orders, payments, or stock.
---

# SCCS — Commerce

**Precedence:** the original engine is authoritative; this is a non-destructive overlay. Commerce is
not new machinery — it is a **domain composed from skills you already have** — and every concept it names obeys the **SNS** (sccs-naming): the offer, the order and the price-role are head-noun-last types, `_s_` carries the price role, `of`/`to` name the endpoints. Money is a measured
quantity (`sccs-units-of-measure`), buyers and sellers are entities (`sccs-entity-system`), the *acts*
of pricing/ordering/paying are events (`sccs-event`), and every whole is a composition
(`sccs-composition`). This skill names the recurring commerce concepts and how they wire together.

## Money first — price is a role, not a structure

A price is **never a bare number**. The canonical money structure is `the monetary amount` — a
**measured value** = `magnitude (the_numbers)` + a **currency unit** — and **`price` / `cost` / `fee`
/ `salary` are roles** over it (same structure, different relation). `the_variant_price` and
`the_variant_cost` point at the same `the monetary amount`; only the role differs. Rendering
(`$1,250.00`) is the currency unit's prototype; conversion is a **live rate**. (Full model:
`sccs-units-of-measure`.) Reach for an explicit `the_price_s_quantity` + `the_price_s_currency`
composition only when the parts must be independently addressable.

## The offer — the catalog unit

`the offer` is a **composition**: what is for sale, at what price, on what terms.

- `the_offer_s_item` (possession) → `the item` — the good/service.
- `the_offer_s_price` (possession, ordered) → `the price` (the price **role** over a monetary amount);
  multiple tiers are **ordered**.
- `the_offer_s_intent` (possession) → `the intent` — sell / rent / auction / quote.

**Quantity-price bands** (bulk pricing) are a small composition: `the quantity price` with
`the_quantity_price_minimum_quantity` and `…_maximum_quantity` (parts → integers) plus its price.

## The item & inventory

`the item` is the good/service type; a sellable variant is an instance. **Inventory** is stock as a
**measured quantity** (a count, or a measured amount with a unit): `the_item_s_stock → the monetary
amount`-style measured value over a *count/weight/volume* unit, optionally per **place**
(`sccs-postal-address` — a warehouse/location) and per **time slot** (availability windows). Stock
changes are **events** (`sccs-event`): receipts, reservations, sales decrement.

## The order & payment

- `the order` is a composition of order lines, each a `the_order_line_s_offer` + a quantity + a
  resolved `the price`; its total is a **derived** monetary amount (computed on read, never stored).
- `the payment agreement` carries terms (`the_payment_agreement_s_term`) and **ordered**
  `the payment installment`s (part, order); each installment has
  `the_payment_installment_s_due_date → the_dates` and `…_s_amount → the monetary amount`.
- A **payment** itself is an **event** (`sccs-event`): agent = buyer entity, recipient = seller
  entity, amount = monetary amount, time = when. The *act* of pricing/ordering/checkout are events
  too — distinct from the standing offer/order they act on.

## Actors & access

Buyer and seller are **entities** (`sccs-entity-system`) — individuals or composites (a company/shop).
Ownership is the `user_id`; sharing/transfer of an offer or order follows the composition's
share/transfer rules (the catalog branch travels by value; referenced entities are shared; the
security branch never travels).

## Authoring procedure

1. Model the **money** first — a `the monetary amount` (magnitude + currency unit); pick the **role**
   (price/cost/fee). Never a bare number.
2. Compose **`the offer`** (item + price(s) + intent); add quantity-price bands if tiered.
3. Model **inventory** as a measured stock quantity, optionally per place / time slot; stock moves are
   events.
4. Compose **`the order`** (lines → offers + quantities); derive totals on read.
5. Add a **`the payment agreement`** with ordered installments; model each **payment as an event**.
6. Make buyers/sellers **entities**; validate with `sccs-grammar-testing` (names read as clauses;
   completeness against each composition's prototype).

## Required output format

Return the standard SCCS triple — a **Concepts** table (`local_ref`, `role`, `category`, `type`,
`referent`, `notes`), a **Connections** table (`local_ref`, `of`, `type`, `order`, `to`, `kind`,
`notes`), and a 2–4 sentence **Summary** naming the offer/order, the price role and its monetary
amount, the payment terms, and which acts are modeled as events.
