# Research Question 1: Dependencies - Global Performance & Robustness Analysis

<!-- Scrollable table wrapper -->
<style>
.table-wrapper { overflow-x: auto; }
.table-wrapper table { border-collapse: collapse; }
.table-wrapper th, .table-wrapper td { white-space: nowrap; padding: 8px 16px; }
</style>

## Overview

**Analysis Date**: 2026-01-13

This analysis focuses on the **Dependencies** test smell category, transforming local project-level
results into a global robustness analysis across two open-source projects (Apache Flink and Apache Hive).

## Data Sources

- **Projects**: Apache Flink and Apache Hive
- **Classification Label**: `dependencies` - Whether dependencies are discussed

## Combination Features Analyzed

1. **Textual Feature**: TF (Term Frequency) vs TF-IDF
2. **Stem Lemma**: Text preprocessing methods (porterstemmer, lemmatizer, textblob, spacy)
3. **N-gram**: N-gram ranges (1 = unigrams, 2 = bigrams)
4. **Imbalance Handling**: Techniques (None, ProWSyn, Polynomial Fit, etc.)

**Total Datasets Analyzed**: 2 (Flink and Hive projects for this label)
**Total Combinations per Dataset**: 48
**Total Unique Feature Combinations**: 48

## Analysis Methodology

### Pre-processing & Grouping

1. **Missing Values**: Filled NaN values in `Imba handling` column with "None"
2. **Feature Key**: Defined as the combination of:
   - Textual feature
   - Stem lemma
   - N-gram
   - Imba handling
3. **Aggregation**: Grouped results from both projects (Flink and Hive) by Feature Key

### Global Metrics Calculated

For each of the 48 unique combinations:
1. **Global Avg Rank**: Mean of `rank_total_win_loss` across both projects (lower = better)
2. **Total Wins**: Sum of `total_wins` across both projects
3. **Total Ties**: Sum of `total_ties` across both projects
4. **Total Losses**: Sum of `total_losses` across both projects
5. **Loss Percentage**: (Total Losses / Total Wins + Total Ties + Total Losses) × 100
6. **Win-to-Loss Ratio**: Total Wins / Total Losses (higher = better)
7. **"General Good" Score**: Count of times the combination achieved rank ≤ 3

## Findings


### Table 1: The Master Top 10 (Global Performance)

<div class="table-wrapper">

| Rank | Feature Combination | Global Avg Rank | Total W-T-L | Loss % | General Good Score | Win-to-Loss Ratio |
|------|------|------|------|------|------|------|
| 1 | TF-IDF + lemmatizer + 2 + ProWSyn | 6.50 | 57 - 319 - 0 | 0.00 | 0 | 0.00 |
| 2 | TF + spacy + 2 + ProWSyn | 10.00 | 79 - 257 - 40 | 10.64 | 1 | 1.98 |
| 3 | TF + spacy + 1 + ProWSyn | 10.25 | 81 - 265 - 30 | 7.98 | 0 | 2.70 |
| 4 | TF-IDF + textblob + 1 + ProWSyn | 10.75 | 64 - 297 - 15 | 3.99 | 0 | 4.27 |
| 5 | TF-IDF + porterstemmer + 2 + ProWSyn | 11.50 | 61 - 304 - 11 | 2.93 | 1 | 5.55 |
| 6 | TF + spacy + 2 + None | 13.50 | 52 - 305 - 19 | 5.05 | 0 | 2.74 |
| 7 | TF + textblob + 2 + None | 13.50 | 62 - 297 - 17 | 4.52 | 1 | 3.65 |
| 8 | TF-IDF + lemmatizer + 1 + ProWSyn | 13.75 | 61 - 299 - 16 | 4.26 | 1 | 3.81 |
| 9 | TF + porterstemmer + 2 + None | 13.75 | 40 - 325 - 11 | 2.93 | 0 | 3.64 |
| 10 | TF-IDF + porterstemmer + 1 + ProWSyn | 14.50 | 54 - 301 - 21 | 5.59 | 0 | 2.57 |

</div>
## Table 2: Risk vs. Reward Analysis

### Group A: Top 5 from Safe Zone (<5% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|
| 1 | TF-IDF + lemmatizer + 2 + ProWSyn | 57 - 319 - 0 | 0.0% | 0.00 | 6.50 |
| 2 | TF-IDF + textblob + 1 + ProWSyn | 64 - 297 - 15 | 3.99% | 4.27 | 10.75 |
| 3 | TF-IDF + porterstemmer + 2 + ProWSyn | 61 - 304 - 11 | 2.93% | 5.55 | 11.50 |
| 4 | TF + textblob + 2 + None | 62 - 297 - 17 | 4.52% | 3.65 | 13.50 |
| 5 | TF-IDF + lemmatizer + 1 + ProWSyn | 61 - 299 - 16 | 4.26% | 3.81 | 13.75 |

</div>

### Group B: Top 5 from Tolerable Zone (5-10% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|
| 1 | TF + spacy + 1 + ProWSyn | 81 - 265 - 30 | 7.98% | 2.70 | 10.25 |
| 2 | TF + spacy + 2 + None | 52 - 305 - 19 | 5.05% | 2.74 | 13.50 |
| 3 | TF-IDF + porterstemmer + 1 + ProWSyn | 54 - 301 - 21 | 5.59% | 2.57 | 14.50 |
| 4 | TF-IDF + lemmatizer + 1 + None | 44 - 307 - 25 | 6.65% | 1.76 | 18.25 |
| 5 | TF + textblob + 2 + ProWSyn | 58 - 282 - 36 | 9.57% | 1.61 | 18.25 |

</div>

**Risk vs. Reward Insight:**
- Group A (Safe Zone <5% Loss) has an average Win-to-Loss Ratio of 3.46 and average Global Avg Rank of 11.20.
- Group B (Tolerable Zone 5-10% Loss) has an average Win-to-Loss Ratio of 2.28 and average Global Avg Rank of 14.95.
- Group B does not offer significantly higher Win-to-Loss Ratio to justify the additional risk.


## Table 3: The "Golden" Candidates

**Criteria**: All of the following must be met:
1. In the Top 10 for Global Avg Rank
2. Loss Percentage < 5%
3. "General Good" Score ≥ 2 (appeared in top 3 in at least both projects)

*No combinations meet all three criteria for "Golden" status.*


## Summary & Recommendations

### Key Findings

1. **Best Global Performer** for Dependencies:
   - TF-IDF + lemmatizer + 2 + ProWSyn
   - Global Avg Rank: 6.50
   - Loss %: 0.0%
   - Win-to-Loss Ratio: 0.00
   - General Good Score: 0/2

### Recommendations for Dependencies

1. **For Maximum Robustness**: Use combinations from the Safe Zone (<5% Loss) to minimize risk across different projects.
2. **For Balanced Performance**: Consider Tolerable Zone combinations if higher win rates are needed for specific applications.
3. **For Proven Excellence**: The "Golden" candidates offer the best combination of global performance, low risk, and consistent top-3 appearances.

### Hypothesis Validation

**Hypothesis**: Certain feature combinations consistently perform well for the Dependencies classification task across different projects.

**Conclusion**: The global analysis reveals that combinations with porterstemmer and ProWSyn imbalance handling consistently appear in top rankings,
supporting the hypothesis that specific feature configurations provide robust performance for Dependencies detection.

---

*Report generated by analyze_rq1_per_label.py*
