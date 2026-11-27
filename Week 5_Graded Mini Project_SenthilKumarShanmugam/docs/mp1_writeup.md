# MP1 Prompt Lab — Reflection

1. **Which strategy won, and on what dimension?**  
   Few-shot had the highest accuracy at 3.0 out of 3 and tied for the top judge score at 4.0. All four strategies had a 100% parse rate. Structured prompting was fastest, with a median latency of 1.326 seconds. The cost values are rounded, so the displayed `0.000` values should not be read as free.

2. **What surprised you?**  
   I expected chain-of-thought to be the strongest, but few-shot had the best accuracy. I was also surprised that the judge score was 4.0 for structured and chain-of-thought even though their field accuracy was 2.9; that suggests the judge may be lenient, so I would inspect the individual disagreements rather than rely on its average alone.

3. **For my eCommerce Capstone, which strategy would you reach for first?**  
   I would start with structured prompting because a clear schema is useful when downstream code needs consistent fields, and it had the lowest median latency while still scoring highly. If the first evaluation showed recurring interpretation errors, I would add a few relevant examples, since few-shot performed best on accuracy here.

4. **If I had another day, what would I try next?**  
   I would evaluate more representative eCommerce examples, including ambiguous product attributes and missing values, and repeat the runs to see whether the small accuracy differences hold. I would also compare structured prompting with and without a few-shot examples and review cases where the judge score disagrees with exact field matching.
