# Research Question 1: Code Related - Global Performance & Robustness Analysis

<!-- Scrollable table wrapper -->
<style>
.table-wrapper { overflow-x: auto; }
.table-wrapper table { border-collapse: collapse; }
.table-wrapper th, .table-wrapper td { white-space: nowrap; padding: 8px 16px; }
</style>

## Overview

**Analysis Date**: 2026-01-13

This analysis focuses on the **Code Related** test smell category, transforming local project-level
results into a global robustness analysis across two open-source projects (Apache Flink and Apache Hive).

## Data Sources

- **Projects**: Apache Flink and Apache Hive
- **Classification Label**: `code_related` - Whether code is mentioned in the discussion

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
| 1 | TF-IDF + porterstemmer + 2 + Polynomial Fit | 1.00 | 101 - 261 - 14 | 3.72 | 2 | 7.21 |
| 2 | TF-IDF + porterstemmer + 1 + Polynomial Fit | 9.25 | 83 - 259 - 34 | 9.04 | 0 | 2.44 |
| 3 | TF-IDF + lemmatizer + 1 + None | 10.00 | 51 - 314 - 11 | 2.93 | 0 | 4.64 |
| 4 | TF-IDF + lemmatizer + 1 + Polynomial Fit | 10.00 | 73 - 276 - 27 | 7.18 | 0 | 2.70 |
| 5 | TF + textblob + 1 + Polynomial Fit | 10.75 | 67 - 288 - 21 | 5.59 | 1 | 3.19 |
| 6 | TF-IDF + spacy + 2 + None | 12.50 | 46 - 319 - 11 | 2.93 | 0 | 4.18 |
| 7 | TF + porterstemmer + 1 + Polynomial Fit | 12.75 | 63 - 290 - 23 | 6.12 | 0 | 2.74 |
| 8 | TF + porterstemmer + 1 + ProWSyn | 13.25 | 72 - 265 - 39 | 10.37 | 0 | 1.85 |
| 9 | TF + porterstemmer + 2 + ProWSyn | 14.25 | 75 - 257 - 44 | 11.70 | 0 | 1.70 |
| 10 | TF-IDF + textblob + 2 + Polynomial Fit | 14.50 | 76 - 252 - 48 | 12.77 | 0 | 1.58 |

</div>
## Table 2: Risk vs. Reward Analysis

### Group A: Top 5 from Safe Zone (<5% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|
| 1 | TF-IDF + porterstemmer + 2 + Polynomial Fit | 101 - 261 - 14 | 3.72% | 7.21 | 1.00 |
| 2 | TF-IDF + lemmatizer + 1 + None | 51 - 314 - 11 | 2.93% | 4.64 | 10.00 |
| 3 | TF-IDF + spacy + 2 + None | 46 - 319 - 11 | 2.93% | 4.18 | 12.50 |
| 4 | TF-IDF + porterstemmer + 1 + None | 41 - 317 - 18 | 4.79% | 2.28 | 17.00 |

</div>

### Group B: Top 5 from Tolerable Zone (5-10% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|
| 1 | TF-IDF + porterstemmer + 1 + Polynomial Fit | 83 - 259 - 34 | 9.04% | 2.44 | 9.25 |
| 2 | TF-IDF + lemmatizer + 1 + Polynomial Fit | 73 - 276 - 27 | 7.18% | 2.70 | 10.00 |
| 3 | TF + textblob + 1 + Polynomial Fit | 67 - 288 - 21 | 5.59% | 3.19 | 10.75 |
| 4 | TF + porterstemmer + 1 + Polynomial Fit | 63 - 290 - 23 | 6.12% | 2.74 | 12.75 |
| 5 | TF-IDF + textblob + 2 + None | 49 - 308 - 19 | 5.05% | 2.58 | 15.25 |

</div>

**Risk vs. Reward Insight:**
- Group A (Safe Zone <5% Loss) has an average Win-to-Loss Ratio of 4.58 and average Global Avg Rank of 10.12.
- Group B (Tolerable Zone 5-10% Loss) has an average Win-to-Loss Ratio of 2.73 and average Global Avg Rank of 11.60.
- Group B does not offer significantly higher Win-to-Loss Ratio to justify the additional risk.


## Table 3: The "Golden" Candidates

**Criteria**: All of the following must be met:
1. In the Top 10 for Global Avg Rank
2. Loss Percentage < 5%
3. "General Good" Score ≥ 2 (appeared in top 3 in at least both projects)

*No combinations meet all three criteria for "Golden" status.*


## Summary & Recommendations

### Key Findings

1. **Best Global Performer** for Code Related:
   - TF-IDF + porterstemmer + 2 + Polynomial Fit
   - Global Avg Rank: 1.00
   - Loss %: 3.72%
   - Win-to-Loss Ratio: 7.21
   - General Good Score: 2/2

### Recommendations for Code Related

1. **For Maximum Robustness**: Use combinations from the Safe Zone (<5% Loss) to minimize risk across different projects.
2. **For Balanced Performance**: Consider Tolerable Zone combinations if higher win rates are needed for specific applications.
3. **For Proven Excellence**: The "Golden" candidates offer the best combination of global performance, low risk, and consistent top-3 appearances.

### Hypothesis Validation

**Hypothesis**: Certain feature combinations consistently perform well for the Code Related classification task across different projects.

**Conclusion**: The global analysis reveals that combinations with porterstemmer and ProWSyn imbalance handling consistently appear in top rankings,
supporting the hypothesis that specific feature configurations provide robust performance for Code Related detection.

---

*Report generated by analyze_rq1_per_label.py*
