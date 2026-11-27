# Prompting Strategy Comparison

## Results

| Strategy | Accuracy (mean of 3) | Parse rate | Judge score | Total cost ($) | Latency p50 (s) |
|---|---:|---:|---:|---:|---:|
| Chain-of-thought (`cot`) | 2.9 | 1.0 | 4.0 | 0.000 | 1.387 |
| Few-shot | **3.0** | 1.0 | 4.0 | 0.001 | 1.407 |
| Structured / role-based | 2.9 | 1.0 | 4.0 | 0.000 | **1.326** |
| Zero-shot | 2.8 | 1.0 | 3.9 | 0.000 | 1.469 |

## Top insights

- **Few-shot led on exact accuracy**, averaging 3.0 correct fields out of 3. It also tied with chain-of-thought and structured prompting for the highest judge score.
- **Every strategy had a 1.0 parse rate.** All responses were parseable; this measures output format, not whether the extracted values were correct.
- **Structured prompting was fastest** at a median of 1.326 seconds. Few-shot was slightly slower at 1.407 seconds, while zero-shot was slowest at 1.469 seconds.
- **Zero-shot had the lowest accuracy and judge score** in this comparison. It still produced parseable results, but consistent JSON alone did not guarantee equally accurate extraction.
- **The judge scores may be too generous to distinguish strategies.** Structured and chain-of-thought both scored 4.0 from the judge despite averaging 2.9 out of 3 on exact field matching. Reviewing individual disagreements would help assess judge reliability.
- **Treat the displayed costs as rounded values.** A reported `0.000` does not mean the calls were free; the table only shows three decimal places. Few-shot shows `0.001`, consistent with its longer example-rich prompts.

These results come from a small set of ten snippets, so the small differences between strategies should be treated as observations rather than conclusive rankings.
