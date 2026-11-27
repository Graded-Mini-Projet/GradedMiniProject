# ADR: Few-Shot Prompting for eCommerce Product Extraction

- **Status:** Proposed
- **Date:** 2026-11-26

## Context

The eCommerce capstone needs to turn product-listing text into consistent catalog records that downstream search, filtering, and display features can use. The initial extraction target is:

- `product_name` — string
- `price` — number, or `null` when it is not stated
- `category` — string, or `null` when it is not stated

Product names and categories should be preserved as provided rather than silently corrected or inferred. The source does not establish a currency, so the extractor must not add one.

In the MP1 comparison, few-shot prompting had the highest mean field accuracy (3.0 out of 3) and tied for the highest judge score (4.0). All four strategies had a 100% parse rate. Structured prompting had the lowest median latency (1.326 seconds), while few-shot's median latency was 1.407 seconds. These results came from ten job-posting examples, not eCommerce listings; they provide a starting hypothesis, not proof of performance on product data. Costs were displayed rounded to three decimal places.

## Decision

Start with **few-shot prompting** to extract the three catalog fields. Examples can demonstrate the expected mapping from varied listing text to the catalog JSON format. Require a JSON-only response with exactly the target keys, preserve source values, and return `null` rather than guessing when a value is missing.

Illustrative prompt pattern:

```text
Extract product_name, price, and category from the listing.
Return one JSON object with exactly those keys. Keep the product name and
category as written, represent price as a number, and use null for missing
values. Do not infer a currency or invent details.

Listing: "Cedar Travel Mug — Home & Kitchen — 18"
Output: {"product_name": "Cedar Travel Mug", "price": 18, "category": "Home & Kitchen"}

Listing: "Pocket Notebook, Stationery, price 6"
Output: {"product_name": "Pocket Notebook", "price": 6, "category": "Stationery"}

Listing: <product listing>
Output:
```

These are prompt examples, not evaluation results. The actual few-shot examples should be representative of the capstone's product data.

## Alternatives considered

- **Structured / role-based:** had nearly the same MP1 accuracy and the lowest latency. Prefer this if response speed or a simpler prompt outweighs the possible accuracy benefit of examples.
- **Chain-of-thought:** matched structured prompting's accuracy and judge score in MP1, but did not lead on accuracy or latency. It adds instructions without a demonstrated benefit for this direct field extraction.
- **Zero-shot:** simplest, but had the lowest MP1 accuracy and judge score.

## Consequences

- Few-shot examples add input tokens and may increase cost and latency.
- Incorrect or unrepresentative examples can teach the wrong catalog mappings; keep examples grounded in actual product data and include missing-value and ambiguous-listing cases.
- The eCommerce strategy has **not yet been calibrated**. Before relying on it, compare it with structured prompting on five to ten representative listings and review exact field matches, parse success, and latency.
- Keep exact-match evaluation alongside any LLM judge. The MP1 judge gave perfect average scores to strategies whose field accuracy was below 3.0, so its score alone is not sufficient evidence.
