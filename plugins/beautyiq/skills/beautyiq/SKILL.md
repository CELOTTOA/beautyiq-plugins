---
name: beautyiq
description: Find cheaper cross-brand dupes of a makeup shade and check endocrine-disruptor safety scores via the BeautyIQ MCP tools (search_products, find_dupes, product_safety_score, score_ingredient_list). Use when the user names a makeup product/shade or pastes an INCI list.
---

# BeautyIQ — Shade Dupes & Safety

BeautyIQ answers "which shade do I pick when I switch brand?" with one
measured, explained answer. Public read-only API (no auth); the tools
already embed the behavioral contract — the notes below are what the
tool descriptions do not say.

## Flow

1. `search_products` — resolve free text (product, line, or bare shade
   like "Ruby Woo") to catalog products. When
   `requires_clarification` is true, ask the user which product/shade
   they mean; never pick an ambiguous result yourself. The result's
   `matched_shade.variant_id` is the input for the next step.
2. `find_dupes` — takes that `variant_id` and returns ranked candidates
   with classification (TRUE_DUPE → LOW_CONFIDENCE_MATCH), dupe score
   and per-item explanations. Relay the engine's ranking as-is,
   including honest LOW_CONFIDENCE and empty results; never add your
   own product suggestions or general knowledge.
3. Safety, optional: `product_safety_score` for a catalog `product_id`,
   or `score_ingredient_list` for a raw INCI list not in the catalog
   (needs a category slug, e.g. `lipstick`). Always relay the returned
   disclaimer verbatim — it is presence-based screening, not a dose
   assessment.

## Operational notes

- The endpoint is rate-limited at nginx: **30 requests/min, burst 10**.
  Call tools sequentially, not in parallel bursts; on HTTP 429 wait
  ~30 s before retrying once.
- Prices in results are dated observations with currency, country,
  date and source — never present them as live quotes.
- Result items may include a `dupe_page_url` (public BeautyIQ page);
  offer it as a follow-up, the answer stands on its own.
- If the server is unreachable, say so and stop — do not fall back to
  guessing shades from general knowledge.

## Starter prompts

- "What's a good dupe for MAC Ruby Woo?"
- "Find cheaper alternatives to Estée Lauder Double Wear 2W1 Dawn under €30"
- "Score this ingredient list for a lipstick: AQUA, BHT, CI 15850"
