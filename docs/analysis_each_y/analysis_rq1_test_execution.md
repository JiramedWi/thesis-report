# Research Question 1: Test Execution - Global Performance & Robustness Analysis

<!-- Scrollable table wrapper -->
<style>
.table-wrapper { overflow-x: auto; }
.table-wrapper table { border-collapse: collapse; }
.table-wrapper th, .table-wrapper td { white-space: nowrap; padding: 8px 16px; }
</style>

## Overview

**Analysis Date**: 2026-01-13

This analysis focuses on the **Test Execution** test smell category, transforming local project-level
results into a global robustness analysis across two open-source projects (Apache Flink and Apache Hive).

## Data Sources

- **Projects**: Apache Flink and Apache Hive
- **Classification Label**: `test_execution` - Whether test execution is mentioned

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
| 1 | TF + spacy + 1 + ProWSyn | 5.75 | 104 - 242 - 30 | 7.98 | 0 | 3.47 |
| 2 | TF + lemmatizer + 2 + Polynomial Fit | 6.50 | 99 - 246 - 31 | 8.24 | 0 | 3.19 |
| 3 | TF + porterstemmer + 1 + ProWSyn | 6.75 | 93 - 265 - 18 | 4.79 | 1 | 5.17 |
| 4 | TF-IDF + porterstemmer + 2 + ProWSyn | 8.25 | 94 - 257 - 25 | 6.65 | 0 | 3.76 |
| 5 | TF + textblob + 1 + ProWSyn | 9.75 | 108 - 234 - 34 | 9.04 | 1 | 3.18 |
| 6 | TF + lemmatizer + 1 + ProWSyn | 10.75 | 94 - 245 - 37 | 9.84 | 0 | 2.54 |
| 7 | TF + lemmatizer + 2 + None | 13.25 | 58 - 303 - 15 | 3.99 | 0 | 3.87 |
| 8 | TF + spacy + 2 + None | 13.50 | 46 - 325 - 5 | 1.33 | 0 | 9.20 |
| 9 | TF-IDF + porterstemmer + 1 + None | 14.75 | 48 - 312 - 16 | 4.26 | 0 | 3.00 |
| 10 | TF-IDF + spacy + 1 + Polynomial Fit | 14.75 | 90 - 232 - 54 | 14.36 | 0 | 1.67 |

</div>
## Table 2: Risk vs. Reward Analysis

### Group A: Top 5 from Safe Zone (<5% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 93 - 265 - 18 | 4.79% | 5.17 | 6.75 |
| 2 | TF + lemmatizer + 2 + None | 58 - 303 - 15 | 3.99% | 3.87 | 13.25 |
| 3 | TF + spacy + 2 + None | 46 - 325 - 5 | 1.33% | 9.20 | 13.50 |
| 4 | TF-IDF + porterstemmer + 1 + None | 48 - 312 - 16 | 4.26% | 3.00 | 14.75 |
| 5 | TF-IDF + spacy + 1 + None | 40 - 319 - 17 | 4.52% | 2.35 | 16.75 |

</div>

### Group B: Top 5 from Tolerable Zone (5-10% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|
| 1 | TF + spacy + 1 + ProWSyn | 104 - 242 - 30 | 7.98% | 3.47 | 5.75 |
| 2 | TF + lemmatizer + 2 + Polynomial Fit | 99 - 246 - 31 | 8.24% | 3.19 | 6.50 |
| 3 | TF-IDF + porterstemmer + 2 + ProWSyn | 94 - 257 - 25 | 6.65% | 3.76 | 8.25 |
| 4 | TF + textblob + 1 + ProWSyn | 108 - 234 - 34 | 9.04% | 3.18 | 9.75 |
| 5 | TF + lemmatizer + 1 + ProWSyn | 94 - 245 - 37 | 9.84% | 2.54 | 10.75 |

</div>

**Risk vs. Reward Insight:**
- Group A (Safe Zone <5% Loss) has an average Win-to-Loss Ratio of 4.72 and average Global Avg Rank of 13.00.
- Group B (Tolerable Zone 5-10% Loss) has an average Win-to-Loss Ratio of 3.23 and average Global Avg Rank of 8.20.
- Group B does not offer significantly higher Win-to-Loss Ratio to justify the additional risk.


## Table 3: The "Golden" Candidates

**Criteria**: All of the following must be met:
1. In the Top 10 for Global Avg Rank
2. Loss Percentage < 5%
3. "General Good" Score ≥ 2 (appeared in top 3 in at least both projects)

*No combinations meet all three criteria for "Golden" status.*


## Summary & Recommendations

### Key Findings

1. **Best Global Performer** for Test Execution:
   - TF + spacy + 1 + ProWSyn
   - Global Avg Rank: 5.75
   - Loss %: 7.98%
   - Win-to-Loss Ratio: 3.47
   - General Good Score: 0/2

### Recommendations for Test Execution

1. **For Maximum Robustness**: Use combinations from the Safe Zone (<5% Loss) to minimize risk across different projects.
2. **For Balanced Performance**: Consider Tolerable Zone combinations if higher win rates are needed for specific applications.
3. **For Proven Excellence**: The "Golden" candidates offer the best combination of global performance, low risk, and consistent top-3 appearances.

### Hypothesis Validation

**Hypothesis**: Certain feature combinations consistently perform well for the Test Execution classification task across different projects.

**Conclusion**: The global analysis reveals that combinations with porterstemmer and ProWSyn imbalance handling consistently appear in top rankings,
supporting the hypothesis that specific feature configurations provide robust performance for Test Execution detection.

---

*Report generated by analyze_rq1_per_label.py*
