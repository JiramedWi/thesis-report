# Research Question 1: Test Semantic Smell - Global Performance & Robustness Analysis

<!-- Scrollable table wrapper -->
<style>
.table-wrapper { overflow-x: auto; }
.table-wrapper table { border-collapse: collapse; }
.table-wrapper th, .table-wrapper td { white-space: nowrap; padding: 8px 16px; }
</style>

## Overview

**Analysis Date**: 2026-01-13

This analysis focuses on the **Test Semantic Smell** test smell category, transforming local project-level
results into a global robustness analysis across two open-source projects (Apache Flink and Apache Hive).

## Data Sources

- **Projects**: Apache Flink and Apache Hive
- **Classification Label**: `test_semantic_smell` - Whether semantic test smells are present

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
| 1 | TF + porterstemmer + 1 + ProWSyn | 2.50 | 109 - 256 - 11 | 2.93 | 1 | 9.91 |
| 2 | TF-IDF + porterstemmer + 2 + ProWSyn | 3.00 | 118 - 243 - 15 | 3.99 | 1 | 7.87 |
| 3 | TF + porterstemmer + 2 + ProWSyn | 3.50 | 100 - 261 - 15 | 3.99 | 1 | 6.67 |
| 4 | TF + lemmatizer + 1 + ProWSyn | 5.00 | 102 - 251 - 23 | 6.12 | 1 | 4.43 |
| 5 | TF + textblob + 2 + ProWSyn | 6.25 | 98 - 251 - 27 | 7.18 | 0 | 3.63 |
| 6 | TF-IDF + porterstemmer + 1 + ProWSyn | 8.00 | 92 - 260 - 24 | 6.38 | 1 | 3.83 |
| 7 | TF + lemmatizer + 1 + Polynomial Fit | 8.00 | 95 - 250 - 31 | 8.24 | 0 | 3.06 |
| 8 | TF-IDF + textblob + 1 + ProWSyn | 9.50 | 94 - 248 - 34 | 9.04 | 0 | 2.76 |
| 9 | TF + textblob + 1 + Polynomial Fit | 9.50 | 101 - 232 - 43 | 11.44 | 0 | 2.35 |
| 10 | TF + porterstemmer + 1 + Polynomial Fit | 9.75 | 91 - 245 - 40 | 10.64 | 0 | 2.28 |

</div>
## Table 2: Risk vs. Reward Analysis

### Group A: Top 5 from Safe Zone (<5% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 109 - 256 - 11 | 2.93% | 9.91 | 2.50 |
| 2 | TF-IDF + porterstemmer + 2 + ProWSyn | 118 - 243 - 15 | 3.99% | 7.87 | 3.00 |
| 3 | TF + porterstemmer + 2 + ProWSyn | 100 - 261 - 15 | 3.99% | 6.67 | 3.50 |
| 4 | TF-IDF + spacy + 1 + None | 40 - 322 - 14 | 3.72% | 2.86 | 18.50 |
| 5 | TF-IDF + spacy + 2 + None | 25 - 337 - 14 | 3.72% | 1.79 | 21.75 |

</div>

### Group B: Top 5 from Tolerable Zone (5-10% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|
| 1 | TF + lemmatizer + 1 + ProWSyn | 102 - 251 - 23 | 6.12% | 4.43 | 5.00 |
| 2 | TF + textblob + 2 + ProWSyn | 98 - 251 - 27 | 7.18% | 3.63 | 6.25 |
| 3 | TF-IDF + porterstemmer + 1 + ProWSyn | 92 - 260 - 24 | 6.38% | 3.83 | 8.00 |
| 4 | TF + lemmatizer + 1 + Polynomial Fit | 95 - 250 - 31 | 8.24% | 3.06 | 8.00 |
| 5 | TF-IDF + textblob + 1 + ProWSyn | 94 - 248 - 34 | 9.04% | 2.76 | 9.50 |

</div>

**Risk vs. Reward Insight:**
- Group A (Safe Zone <5% Loss) has an average Win-to-Loss Ratio of 5.82 and average Global Avg Rank of 9.85.
- Group B (Tolerable Zone 5-10% Loss) has an average Win-to-Loss Ratio of 3.54 and average Global Avg Rank of 7.35.
- Group B does not offer significantly higher Win-to-Loss Ratio to justify the additional risk.


## Table 3: The "Golden" Candidates

**Criteria**: All of the following must be met:
1. In the Top 10 for Global Avg Rank
2. Loss Percentage < 5%
3. "General Good" Score ≥ 2 (appeared in top 3 in at least both projects)

*No combinations meet all three criteria for "Golden" status.*


## Summary & Recommendations

### Key Findings

1. **Best Global Performer** for Test Semantic Smell:
   - TF + porterstemmer + 1 + ProWSyn
   - Global Avg Rank: 2.50
   - Loss %: 2.93%
   - Win-to-Loss Ratio: 9.91
   - General Good Score: 1/2

### Recommendations for Test Semantic Smell

1. **For Maximum Robustness**: Use combinations from the Safe Zone (<5% Loss) to minimize risk across different projects.
2. **For Balanced Performance**: Consider Tolerable Zone combinations if higher win rates are needed for specific applications.
3. **For Proven Excellence**: The "Golden" candidates offer the best combination of global performance, low risk, and consistent top-3 appearances.

### Hypothesis Validation

**Hypothesis**: Certain feature combinations consistently perform well for the Test Semantic Smell classification task across different projects.

**Conclusion**: The global analysis reveals that combinations with porterstemmer and ProWSyn imbalance handling consistently appear in top rankings,
supporting the hypothesis that specific feature configurations provide robust performance for Test Semantic Smell detection.

---

*Report generated by analyze_rq1_per_label.py*
