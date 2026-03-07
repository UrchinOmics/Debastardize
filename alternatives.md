# Alternatives Reference

Statistical methods with problematic namesakes and their alternatives. This is not exhaustive — it covers the most commonly encountered methods from Fisher, Pearson, and Galton.

---

## Fisher's exact test

| Alternative | Best use case | Assumptions | Where it fails | Reference |
|---|---|---|---|---|
| Barnard's exact test | 2×2 contingency tables; uniformly more powerful than Fisher's | Independent observations; fixed total sample size (but not fixed margins) | Only defined for 2×2 tables; computationally intensive for very large samples | [Barnard's test (R-statistics blog)](https://www.r-statistics.com/2010/02/barnards-exact-test-a-powerful-alternative-for-fishers-exact-test-implemented-in-r/) |
| Boschloo's exact test | 2×2 contingency tables; maximizes power among exact tests | Independent observations; fixed total sample size | Only for 2×2 tables; less widely implemented in software | [Boschloo's test (Wikipedia)](https://en.wikipedia.org/wiki/Boschloo%27s_test) |
| Chi-squared test with simulation | Contingency tables larger than 2×2 with small expected counts | Independent observations | Requires choosing number of Monte Carlo replicates; results vary slightly between runs | [Monte Carlo chi-squared (R documentation)](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/chisq.test.html) |

---

## F-test (equality of variances)

| Alternative | Best use case | Assumptions | Where it fails | Reference |
|---|---|---|---|---|
| Levene's test | Testing equality of variances across groups; robust to non-normality | Independent observations; continuous data | Low power with very small samples; sensitive to outliers when using group means (use median variant instead) | [Levene's test (NIST)](https://www.itl.nist.gov/div898/software/dataplot/refman2/auxillar/levltest.htm) |
| Brown-Forsythe test | Same as Levene's but uses group medians; more robust to skewed data | Independent observations; continuous data | Very low power when group sizes are extremely unequal | [Brown-Forsythe (STHDA)](http://www.sthda.com/english/wiki/compare-multiple-sample-variances-in-r) |
| Fligner-Killeen test | Nonparametric test of variance homogeneity; most robust option | Independent observations; at least ordinal data | Less powerful than Levene's when normality holds | [Fligner-Killeen (R documentation)](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/fligner.test.html) |

---

## ANOVA (one-way)

| Alternative | Best use case | Assumptions | Where it fails | Reference |
|---|---|---|---|---|
| Welch's ANOVA | Comparing means across 3+ groups; does not assume equal variances | Independent observations; approximately normal data within groups (robust with n > 30) | Very small and very unequal group sizes with heavy skew | [Welch's ANOVA (Statology)](https://www.statology.org/welchs-anova-in-r/) |
| Kruskal-Wallis test | Comparing distributions across 3+ groups; nonparametric | Independent observations; at least ordinal data; similar distribution shapes across groups (for location interpretation) | Tests stochastic dominance, not means specifically; loses power relative to ANOVA when normality holds | [Kruskal-Wallis (STHDA)](http://www.sthda.com/english/wiki/kruskal-wallis-test-in-r) |

---

## Fisher's LSD (post-hoc pairwise comparisons)

| Alternative | Best use case | Assumptions | Where it fails | Reference |
|---|---|---|---|---|
| Tukey's HSD | All pairwise comparisons after ANOVA; controls family-wise error rate | Equal group sizes (approximately); normality; equal variances | Conservative with very unequal group sizes; requires ANOVA context | [Tukey's HSD (Real Statistics)](https://real-statistics.com/one-way-analysis-of-variance-anova/unplanned-comparisons/tukey-hsd/) |
| Dunn's test | Pairwise comparisons after Kruskal-Wallis; nonparametric | Independent observations; at least ordinal data | Less powerful than Tukey's when normality holds; multiple correction methods affect interpretation | [Dunn's test (R dunn.test package)](https://cran.r-project.org/web/packages/dunn.test/dunn.test.pdf) |
| Games-Howell test | Pairwise comparisons when variances are unequal; no equal variance assumption | Independent observations; approximately normal | Can be liberal with very small samples (n < 6 per group) | [Games-Howell (Real Statistics)](https://real-statistics.com/one-way-analysis-of-variance-anova/unplanned-comparisons/games-howell-test/) |

---

## Pearson correlation coefficient (r)

| Alternative | Best use case | Assumptions | Where it fails | Reference |
|---|---|---|---|---|
| Kendall's tau | Rank correlation; robust to outliers and non-linearity; works well with small samples and tied ranks | At least ordinal data; paired observations | Smaller in magnitude than Pearson's r or Spearman's rho for the same data (expected, not a flaw); computationally slower for very large datasets | [Kendall's tau (Wikipedia)](https://en.wikipedia.org/wiki/Kendall_rank_correlation_coefficient) |
| Spearman's rho | Rank correlation; measures monotonic association; more intuitive magnitude than tau | At least ordinal data; paired observations | Spearman himself has adjacent issues (g factor / intelligence testing); sensitive to tied ranks; assumes monotonic relationship | [Spearman's rho (Laerd Statistics)](https://statistics.laerd.com/statistical-guides/spearmans-rank-order-correlation-statistical-guide.php) |
| Distance correlation | Measures association including nonlinear relationships; equals zero only if variables are truly independent | Continuous data; paired observations | Computationally expensive; harder to interpret magnitude; less widely known in biological sciences | [Distance correlation (Penn State)](https://online.stat.psu.edu/stat505/lesson/14) |

---

## Pearson's chi-squared test (independence)

| Alternative | Best use case | Assumptions | Where it fails | Reference |
|---|---|---|---|---|
| G-test (likelihood ratio test) | Contingency tables of any size; asymptotically equivalent to chi-squared | Independent observations; expected cell counts > 5 | Same minimum expected count issues as chi-squared; less familiar to some reviewers | [G-test of independence (R Companion)](https://rcompanion.org/rcompanion/b_06.html) |
| Barnard's exact test | 2×2 tables with small samples; more powerful exact alternative | Independent observations; 2×2 table only | Only for 2×2; computationally intensive | [Barnard's test (R-statistics blog)](https://www.r-statistics.com/2010/02/barnards-exact-test-a-powerful-alternative-for-fishers-exact-test-implemented-in-r/) |
| Permutation test of independence | Any contingency table; no distributional assumptions | Independent observations; exchangeability under null | Computationally intensive; results vary slightly between runs | [Permutation tests (R-bloggers)](https://www.r-bloggers.com/2019/07/what-is-a-permutation-test/) |

---

## Pearson's chi-squared goodness-of-fit

| Alternative | Best use case | Assumptions | Where it fails | Reference |
|---|---|---|---|---|
| G-test (goodness-of-fit) | Testing observed vs. expected frequencies; additive across categories | Independent observations; expected counts > 5 | Same minimum count issues; less familiar name | [G-test goodness of fit (LibreTexts)](https://stats.libretexts.org/Bookshelves/Applied_Statistics/Biological_Statistics_(McDonald)/02:_Tests_for_Nominal_Variables/2.04:_GTest_of_Goodness-of-Fit) |
| Kolmogorov-Smirnov test | Comparing a sample to a continuous reference distribution | Continuous data; fully specified null distribution | Not valid for discrete data; less powerful than Anderson-Darling for tail differences; cannot estimate parameters from the same data | [KS test (NIST)](https://www.itl.nist.gov/div898/software/dataplot/refman2/auxillar/ks1samp.htm) |
| Anderson-Darling test | Testing fit to a specific distribution; more sensitive to tails than KS | Continuous data; specific distribution families (normal, exponential, etc.) | Only implemented for certain distributions; cannot use with arbitrary reference distributions | [Anderson-Darling (NIST)](https://www.itl.nist.gov/div898/software/dataplot/refman2/auxillar/anddarl.htm) |

---

## Galton's regression to the mean

| Alternative | Best use case | Assumptions | Where it fails | Reference |
|---|---|---|---|---|
| Regression to the mean (no eponym) | The concept is universal and unavoidable. Just drop the name. Call it "regression to the mean." | — | — | [Regression to the mean (conjointly.com)](https://conjointly.com/kb/regression-to-the-mean/) |

---

## Notes

1. **Spearman's rho** appears as an alternative to Pearson's r above. Spearman developed the concept of general intelligence (the g factor), a framework central to eugenic arguments about racial hierarchy. His rank correlation method is mathematically independent of that work, but researchers should be aware of the history. Kendall's tau is an alternative to both.

2. **The F-distribution** underlies Welch's ANOVA, many regression diagnostics, and other tests that don't carry Fisher's name explicitly. It is mathematically unavoidable in many contexts. Acknowledging this is part of the process.

3. **p-values and significance testing** as a framework were substantially built by Fisher and the Neyman-Pearson school (Egon Pearson was Karl's son). Bayesian inference is a fundamentally different paradigm that sidesteps this lineage entirely, but is not a drop-in replacement.
