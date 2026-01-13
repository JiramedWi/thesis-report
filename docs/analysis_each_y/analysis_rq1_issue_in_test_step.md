# Research Question 1: Issue In Test Step - Global Performance & Robustness Analysis

<!-- Scrollable table wrapper -->
<style>
.table-wrapper { overflow-x: auto; }
.table-wrapper table { border-collapse: collapse; }
.table-wrapper th, .table-wrapper td { white-space: nowrap; padding: 8px 16px; }
</style>

## Overview

**Analysis Date**: 2026-01-13

This analysis focuses on the **Issue In Test Step** test smell category, transforming local project-level
results into a global robustness analysis across two open-source projects (Apache Flink and Apache Hive).

## Data Sources

- **Projects**: Apache Flink and Apache Hive
- **Classification Label**: `issue_in_test_step` - Whether test steps are involved

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
| 1 | TF + porterstemmer + 1 + ProWSyn | 2.00 | 171 - 192 - 13 | 3.46 | 2 | 13.15 |
| 2 | TF-IDF + textblob + 2 + Polynomial Fit | 4.50 | 148 - 206 - 22 | 5.85 | 1 | 6.73 |
| 3 | TF-IDF + textblob + 1 + ProWSyn | 8.25 | 118 - 221 - 37 | 9.84 | 1 | 3.19 |
| 4 | TF-IDF + lemmatizer + 2 + ProWSyn | 8.50 | 158 - 194 - 24 | 6.38 | 1 | 6.58 |
| 5 | TF-IDF + porterstemmer + 1 + Polynomial Fit | 8.50 | 129 - 204 - 43 | 11.44 | 0 | 3.00 |
| 6 | TF + textblob + 1 + ProWSyn | 8.50 | 133 - 217 - 26 | 6.91 | 0 | 5.12 |
| 7 | TF-IDF + textblob + 2 + ProWSyn | 11.50 | 121 - 215 - 40 | 10.64 | 0 | 3.02 |
| 8 | TF + porterstemmer + 1 + Polynomial Fit | 13.25 | 65 - 277 - 34 | 9.04 | 1 | 1.91 |
| 9 | TF + porterstemmer + 2 + None | 13.50 | 87 - 261 - 28 | 7.45 | 0 | 3.11 |
| 10 | TF + porterstemmer + 2 + Polynomial Fit | 15.00 | 56 - 281 - 39 | 10.37 | 0 | 1.44 |

</div>
## Table 2: Risk vs. Reward Analysis

### Group A: Top 5 from Safe Zone (<5% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 171 - 192 - 13 | 3.46% | 13.15 | 2.00 |

</div>

### Group B: Top 5 from Tolerable Zone (5-10% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|
| 1 | TF-IDF + textblob + 2 + Polynomial Fit | 148 - 206 - 22 | 5.85% | 6.73 | 4.50 |
| 2 | TF-IDF + textblob + 1 + ProWSyn | 118 - 221 - 37 | 9.84% | 3.19 | 8.25 |
| 3 | TF-IDF + lemmatizer + 2 + ProWSyn | 158 - 194 - 24 | 6.38% | 6.58 | 8.50 |
| 4 | TF + textblob + 1 + ProWSyn | 133 - 217 - 26 | 6.91% | 5.12 | 8.50 |
| 5 | TF + porterstemmer + 1 + Polynomial Fit | 65 - 277 - 34 | 9.04% | 1.91 | 13.25 |

</div>

**Risk vs. Reward Insight:**
- Group A (Safe Zone <5% Loss) has an average Win-to-Loss Ratio of 13.15 and average Global Avg Rank of 2.00.
- Group B (Tolerable Zone 5-10% Loss) has an average Win-to-Loss Ratio of 4.71 and average Global Avg Rank of 8.60.
- Group B does not offer significantly higher Win-to-Loss Ratio to justify the additional risk.


## Table 3: The "Golden" Candidates

**Criteria**: All of the following must be met:
1. In the Top 10 for Global Avg Rank
2. Loss Percentage < 5%
3. "General Good" Score ≥ 2 (appeared in top 3 in at least both projects)

*No combinations meet all three criteria for "Golden" status.*


## Summary & Recommendations

### Key Findings

1. **Best Global Performer** for Issue In Test Step:
   - TF + porterstemmer + 1 + ProWSyn
   - Global Avg Rank: 2.00
   - Loss %: 3.46%
   - Win-to-Loss Ratio: 13.15
   - General Good Score: 2/2

### Recommendations for Issue In Test Step

1. **For Maximum Robustness**: Use combinations from the Safe Zone (<5% Loss) to minimize risk across different projects.
2. **For Balanced Performance**: Consider Tolerable Zone combinations if higher win rates are needed for specific applications.
3. **For Proven Excellence**: The "Golden" candidates offer the best combination of global performance, low risk, and consistent top-3 appearances.

### Hypothesis Validation

**Hypothesis**: Certain feature combinations consistently perform well for the Issue In Test Step classification task across different projects.

**Conclusion**: The global analysis reveals that combinations with porterstemmer and ProWSyn imbalance handling consistently appear in top rankings,
supporting the hypothesis that specific feature configurations provide robust performance for Issue In Test Step detection.

---

*Report generated by analyze_rq1_per_label.py*
