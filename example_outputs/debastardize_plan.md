# Debastardize Plan

> **Before applying any changes from this plan, ensure your project is backed up or under version control so all changes are tracked and reversible.** This project uses git. Consider creating a dedicated branch: `git checkout -b debastardize`

---

## Summary Table

| Location | Method/reference | Named after | Issue | Confidence | Decision | Replacement | Notes |
|----------|-----------------|-------------|-------|------------|----------|-------------|-------|
| `10v1_abundance_analysis.Rmd` L98-116 | `fisher.test()` | R.A. Fisher | Eugenics, scientific racism | CONFIRMED | **SWAP** | Barnard's exact test (`Exact::exact.test()`) | More powerful for 2x2 tables |
| `10v1_abundance_analysis.Rmd` L99,107,116 | `fisher_pval`, `fisher_padj` | R.A. Fisher | (variable names) | CONFIRMED | **RENAME** | `exact_pval`, `exact_padj` | Column names in output CSV will change |
| `08v1_concordance.Rmd` L143-145 | `cor.test(method="pearson")` | Karl Pearson | Eugenics, white supremacy | CONFIRMED | **SWAP** | `cor.test(method="kendall")` | Rank correlation is more appropriate for pseudotime |
| `08v1_concordance.Rmd` L143-161 | `pearson_test`, `pearson_r`, `pearson_p` | Karl Pearson | (variable names) | CONFIRMED | **RENAME** | `kendall_test`, `kendall_tau`, `kendall_p` | |
| `08v1_concordance.Rmd` L140-1091 | `spearman_rho`, `spearman_test`, `spearman_p` | Charles Spearman | g factor / intelligence testing | FLAGGED | **RENAME** | `rank_rho`, `rank_cor_test`, `rank_cor_p` | Method kept; eponym stripped from variables |
| `08v1_concordance.Rmd` L202-206 | "Spearman rho" in plot labels | Charles Spearman | (labels) | FLAGGED | **RENAME** | "Rank correlation rho" | |
| `09v1_cytotrace.Rmd` | `method = "spearman"` | Charles Spearman | (function parameter) | FLAGGED | **KEEP** | — | Parameter is R's API; cannot rename |
| `06v3_trajectory_palantir.ipynb` | `spearmanr()` | Charles Spearman | (function call) | FLAGGED | **KEEP** | — | scipy API; cannot rename |
| Multiple files (6) | `p.adjust(method="BH")` | Benjamini & Hochberg | — | No concerns | **KEEP** | — | Clean lineage |
| Multiple files | `wilcox.test()` | Frank Wilcoxon | — | No concerns | **KEEP** | — | Clean lineage |
| `12v1_trajectory_projection.Rmd` | `ks.test()` | Kolmogorov & Smirnov | — | No concerns | **KEEP** | — | Clean lineage |

---

## Suggested Changes

### File 1: `code/10v1_abundance_analysis.Rmd`

**Change type:** SWAP Fisher's exact test to Barnard's exact test + RENAME variables

**Statistical impact:** P-values will change. Barnard's test is uniformly more powerful for 2x2 contingency tables (does not condition on both margins). Significant results will remain significant; some borderline results may become significant. This is a *better* test for this use case.

**New dependency:** `library(Exact)` — install with `install.packages("Exact")`

#### Diff 1a: Add library call (near top of file, with other library calls)

```
OLD (add after existing library calls):
(no Exact library call)

NEW:
library(Exact)
```

#### Diff 1b: Lines 98-105

```
OLD:
# Fisher's exact test per cluster
count_table$fisher_pval <- sapply(seq_len(nrow(count_table)), function(i) {
  a <- count_table$pmar1_LNP[i]
  b <- count_table$total_pmar1[1] - a
  c_val <- count_table$empty_LNP[i]
  d <- count_table$total_empty[1] - c_val
  fisher.test(matrix(c(a, b, c_val, d), nrow = 2))$p.value
})

NEW:
# Barnard's exact unconditional test per cluster
count_table$exact_pval <- sapply(seq_len(nrow(count_table)), function(i) {
  a <- count_table$pmar1_LNP[i]
  b <- count_table$total_pmar1[1] - a
  c_val <- count_table$empty_LNP[i]
  d <- count_table$total_empty[1] - c_val
  exact.test(matrix(c(a, b, c_val, d), nrow = 2), method = "Z-pooled",
             to.plot = FALSE)$p.value
})
```

#### Diff 1c: Line 107

```
OLD:
count_table$fisher_padj <- p.adjust(count_table$fisher_pval, method = "BH")

NEW:
count_table$exact_padj <- p.adjust(count_table$exact_pval, method = "BH")
```

#### Diff 1d: Lines 114-117

```
OLD:
knitr::kable(count_table %>% select(predicted_label, empty_LNP, pmar1_LNP,
                                      prop_empty, prop_pmar1, log2fc,
                                      fisher_pval, fisher_padj),
             digits = 4, caption = "Cluster abundance: pmar1 vs GFP")

NEW:
knitr::kable(count_table %>% select(predicted_label, empty_LNP, pmar1_LNP,
                                      prop_empty, prop_pmar1, log2fc,
                                      exact_pval, exact_padj),
             digits = 4, caption = "Cluster abundance: pmar1 vs GFP")
```

#### Diff 1e: Line 153

```
OLD:
                      fill = ifelse(fisher_padj < 0.05, "significant", "ns"))) +

NEW:
                      fill = ifelse(exact_padj < 0.05, "significant", "ns"))) +
```

#### Diff 1f: Line 212

```
OLD:
                         fill = ifelse(fisher_padj < 0.05, "significant", "ns"))) +

NEW:
                         fill = ifelse(exact_padj < 0.05, "significant", "ns"))) +
```

#### Diff 1g: Line 227

```
OLD:
write.csv(count_table_ordered %>% select(predicted_label, log2fc, fisher_padj, mean_pt),

NEW:
write.csv(count_table_ordered %>% select(predicted_label, log2fc, exact_padj, mean_pt),
```

---

### File 2: `code/08v1_concordance.Rmd`

**Change type:** SWAP Pearson to Kendall + RENAME Spearman variable names

**Statistical impact:** Kendall's tau will replace Pearson's r for one comparison (lines 143-145). Tau values are typically smaller in magnitude than r for the same data — this is expected and does not indicate less agreement. The Spearman method calls remain unchanged (only variable names change).

#### Diff 2a: Lines 143-145

```
OLD:
pearson_test <- cor.test(cell_df$palantir_pseudotime[valid],
                         cell_df$monocle3_pseudotime[valid],
                         method = "pearson")

NEW:
kendall_test <- cor.test(cell_df$palantir_pseudotime[valid],
                         cell_df$monocle3_pseudotime[valid],
                         method = "kendall")
```

#### Diff 2b: Lines 140-142

```
OLD:
spearman_test <- cor.test(cell_df$palantir_pseudotime[valid],
                          cell_df$monocle3_pseudotime[valid],
                          method = "spearman")

NEW:
rank_cor_test <- cor.test(cell_df$palantir_pseudotime[valid],
                          cell_df$monocle3_pseudotime[valid],
                          method = "spearman")
```

#### Diff 2c: Lines 147-155

```
OLD:
pt_overall <- data.frame(
  method_pair = "Palantir_vs_Monocle3",
  spearman_rho = spearman_test$estimate,
  spearman_p = spearman_test$p.value,
  pearson_r = pearson_test$estimate,
  pearson_p = pearson_test$p.value,
  n_cells = n_valid,
  stringsAsFactors = FALSE
)

NEW:
pt_overall <- data.frame(
  method_pair = "Palantir_vs_Monocle3",
  rank_rho = rank_cor_test$estimate,
  rank_cor_p = rank_cor_test$p.value,
  kendall_tau = kendall_test$estimate,
  kendall_p = kendall_test$p.value,
  n_cells = n_valid,
  stringsAsFactors = FALSE
)
```

#### Diff 2d: Lines 160-161

```
OLD:
cat("Overall Spearman rho:", round(spearman_test$estimate, 4), "\n")
cat("Overall Pearson r:", round(pearson_test$estimate, 4), "\n")

NEW:
cat("Overall rank correlation rho:", round(rank_cor_test$estimate, 4), "\n")
cat("Overall Kendall tau:", round(kendall_test$estimate, 4), "\n")
```

#### Diff 2e: Line 173

```
OLD:
    spearman_rho = ifelse(

NEW:
    rank_rho = ifelse(
```

#### Diff 2f: Lines 202-206 (plot labels)

```
OLD:
    title = format_title("Pseudotime Concordance: Palantir vs Monocle3",
                         caption = paste0("Spearman rho = ",
                                          round(spearman_test$estimate, 3),
                                          ", n = ", n_valid)),
    caption = format_caption(paste0("Spearman rho = ",
                                    round(spearman_test$estimate, 3),

NEW:
    title = format_title("Pseudotime Concordance: Palantir vs Monocle3",
                         caption = paste0("Rank correlation rho = ",
                                          round(rank_cor_test$estimate, 3),
                                          ", n = ", n_valid)),
    caption = format_caption(paste0("Rank correlation rho = ",
                                    round(rank_cor_test$estimate, 3),
```

#### Remaining `spearman_rho` references in 08v1_concordance.Rmd

The following lines also reference `spearman_rho` and should be renamed to `rank_rho`:

- Line 428: `spearman_rho = NA_real_,` → `rank_rho = NA_real_,`
- Line 438: `fate_cor$spearman_rho[i]` → `fate_cor$rank_rho[i]`
- Line 446: `arrange(desc(spearman_rho))` → `arrange(desc(rank_rho))`
- Line 452: `levels = fate[order(spearman_rho)]` → `levels = fate[order(rank_rho)]`
- Line 454: `aes(x = fate, y = spearman_rho,` → `aes(x = fate, y = rank_rho,`
- Line 486: `round(spearman_rho, 2)` → `round(rank_rho, 2)`
- Line 990: `spearman_rho)` → `rank_rho)`
- Line 991: `rename(pt_spearman_rho = spearman_rho)` → `rename(pt_rank_rho = rank_rho)`
- Line 1022: `spearman_rho)` → `rank_rho)`
- Line 1023: `rename(wot_vs_palantir_rho = spearman_rho)` → `rename(wot_vs_palantir_rho = rank_rho)`
- Line 1051: `pseudotime_spearman_rho = pt_overall$spearman_rho` → `pseudotime_rank_rho = pt_overall$rank_rho`
- Line 1052: `fate_median_spearman_rho = median(fate_cor$spearman_rho)` → `fate_median_rank_rho = median(fate_cor$rank_rho)`
- Line 1053: `fate_mean_spearman_rho = mean(fate_cor$spearman_rho)` → `fate_mean_rank_rho = mean(fate_cor$rank_rho)`
- Line 1065: `round(scores$pseudotime_spearman_rho, 3)` → `round(scores$pseudotime_rank_rho, 3)`
- Line 1067: `round(scores$fate_median_spearman_rho, 3)` → `round(scores$fate_median_rank_rho, 3)`
- Line 1085: `!is.na(spearman_rho), spearman_rho < 0.3` → `!is.na(rank_rho), rank_rho < 0.3`
- Line 1091: `"Spearman rho = ", round(low_pt$spearman_rho, 3)` → `"Rank rho = ", round(low_pt$rank_rho, 3)`

---

### Output files affected

Applying these changes and re-rendering the Rmds will produce output CSVs with renamed columns:

| Output file | Old columns | New columns |
|-------------|-------------|-------------|
| `results/10v1_abundance/abundance_table.csv` | `fisher_pval`, `fisher_padj` | `exact_pval`, `exact_padj` |
| `results/08v1_concordance/pseudotime_overall_correlation.csv` | `spearman_rho`, `spearman_p`, `pearson_r`, `pearson_p` | `rank_rho`, `rank_cor_p`, `kendall_tau`, `kendall_p` |
| `results/08v1_concordance/pseudotime_per_cluster_correlation.csv` | `spearman_rho` | `rank_rho` |

**Note:** Any downstream code or manuscript that reads these CSVs by column name will need updating. Check `results/08v1_concordance/` consumers.

---

## No documented concerns (no action needed)

| Method | Person | Who they were |
|--------|--------|---------------|
| Wilcoxon rank-sum/signed-rank test | Frank Wilcoxon (1892-1965) | Industrial chemist who developed nonparametric tests for biological assay |
| Kolmogorov-Smirnov test | Andrey Kolmogorov (1903-1987), Nikolai Smirnov (1900-1966) | Foundational probabilist; Soviet mathematician who tabulated KS critical values |
| Benjamini-Hochberg FDR correction | Yoav Benjamini (b. 1949), Yosef Hochberg (1945-2013) | Israeli statisticians at Tel Aviv University |
| Seurat package | Georges Seurat (1859-1891) | Post-Impressionist painter; pointillism metaphor for single-cell analysis |
| WOT | C.H. Waddington (1905-1975) | Developmental biologist; coined "epigenetics"; anti-eugenic manifesto signatory |

---

## Footnotes for future manuscript use

If you reference these methods in a manuscript, the following footnotes are available:

### For Barnard's exact test (if replacing Fisher's):

> The exact test of independence used here is Barnard's exact unconditional test^1, which provides greater statistical power than the conditional exact test for 2x2 contingency tables. The conditional exact test was developed by R.A. Fisher, who served as Head of the Department of Eugenics at University College London and campaigned for eugenic sterilization programs^2.

### For Kendall's tau (if replacing Pearson's r):

> Rank correlation was assessed using Kendall's tau^3. The Pearson product-moment correlation, which assumes linearity, is named for Karl Pearson, the first Galton Professor of Eugenics at UCL, who wrote that national progress comes "by way of war with inferior races"^4. Kendall's tau is both more robust for ordinal data and free of this lineage.

### For rank correlation (Spearman's rho, if keeping but acknowledging):

> Rank correlation was computed using Spearman's rho. Spearman developed the concept of general intelligence (the *g* factor), a framework later central to eugenic arguments about racial hierarchy^5,6. His rank correlation method, however, is mathematically independent of his intelligence testing work.

### References

1. Barnard, G.A. A new test for 2x2 tables. *Nature* **156**, 177 (1945).
2. Evans, R.J. RA Fisher and the science of hatred. *New Statesman* (2020).
3. Kendall, M.G. A new measure of rank correlation. *Biometrika* **30**, 81-93 (1938).
4. Pearson, K. *National Life from the Standpoint of Science* (Adam and Charles Black, 1901).
5. Fancher, R.E. *The Intelligence Men: Makers of the IQ Controversy* (Norton, 1985).
6. Gould, S.J. *The Mismeasure of Man* (revised edition, Norton, 1996).
