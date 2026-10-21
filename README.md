# BeautyIQ — Shade Dupes & Safety (ZCode / Claude Code plugin)

Connects your coding agent to the public BeautyIQ MCP server so it can
answer the recurring makeup question — *"which shade do I pick when I
switch brand?"* — with measured, explained answers:

- **Cross-brand dupes** ranked by CIEDE2000 color distance and formula
  similarity, each with an explicit classification (TRUE_DUPE →
  LOW_CONFIDENCE_MATCH) and dated price observations, never live quotes.
- **Endocrine Disruption Score** for catalog products and raw INCI lists,
  against a curated ECHA/SCCS dataset, always with data confidence and
  limits stated.

Remote MCP server: `https://api.beautyiq.cloud/mcp` — read-only, public
data, no authentication, no secrets. Rate limited server-side
(30 req/min); the bundled skill teaches the agent to pace calls
sequentially.

## Tools

| Tool | What it does |
|---|---|
| `search_products` | Resolves free text (product, line, or bare shade like "Ruby Woo") to catalog products with `variant_id` |
| `find_dupes` | Ranked cross-brand dupe candidates for a confirmed shade, with scores and explanations |
| `product_safety_score` | Endocrine Disruption Score for a catalog product |
| `score_ingredient_list` | Stateless ED score for a raw INCI list + category |

## Install — ZCode

1. Open **Plugin Marketplace → Add → Add Plugin Marketplace**.
2. Paste this repository: `CELOTTOA/beautyiq-plugins`
   (or `https://github.com/CELOTTOA/beautyiq-plugins`).
3. Open **Personal → beautyiq-plugins → BeautyIQ — Shade Dupes & Safety**
   and click **Install**.

## Install — Claude Code

```bash
claude plugin marketplace add CELOTTOA/beautyiq-plugins
claude plugin install beautyiq@beautyiq-plugins
```

## Try it

> What's a good dupe for MAC Ruby Woo?

Expected: the agent resolves the shade via `search_products`, then calls
`find_dupes` and relays the engine's ranked candidates with
classifications and scores — no general-knowledge guesses.

## Links

- Site: https://www.beautyiq.cloud
- Dupe hub: https://www.beautyiq.cloud/dupe/
- Privacy: https://www.beautyiq.cloud/privacy.html
- Terms: https://www.beautyiq.cloud/terms.html

## Versioning

The plugin is a thin client: the catalog, scoring and safety data live
server-side and update continuously. Plugin version bumps track manifest
and skill changes only.
