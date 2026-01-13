# Research Question 1: Global Performance & Robustness Analysis

<!-- Scrollable table wrapper -->
<style>
.table-wrapper { overflow-x: auto; }
.table-wrapper table { border-collapse: collapse; }
.table-wrapper th, .table-wrapper td { white-space: nowrap; padding: 8px 16px; }
</style>

## Overview

**Analysis Date**: 2026-01-13

This analysis transforms local project-level results into a global robustness analysis
across two open-source projects (Apache Flink and Apache Hive) and five classification tasks.

### Detailed Analysis by Classification Label

- **[Code Related Analysis](analysis_each_y/analysis_rq1_code_related.md)** - Analysis of code-related feature combinations
- **[Dependencies Analysis](analysis_each_y/analysis_rq1_dependencies.md)** - Analysis of dependency-related feature combinations
- **[Issue in Test Step Analysis](analysis_each_y/analysis_rq1_issue_in_test_step.md)** - Analysis of test step involvement feature combinations
- **[Test Execution Analysis](analysis_each_y/analysis_rq1_test_execution.md)** - Analysis of test execution feature combinations
- **[Test Semantic Smell Analysis](analysis_each_y/analysis_rq1_test_semantic_smell.md)** - Analysis of semantic test smell feature combinations

## Data Sources

- **Projects**: Apache Flink and Apache Hive
- **Classification Labels**:
  - `code_related` - Whether code is mentioned in the discussion
  - `dependencies` - Whether dependencies are discussed
  - `issue_in_test_step` - Whether test steps are involved
  - `test_execution` - Whether test execution is mentioned
  - `test_semantic_smell` - Whether semantic test smells are present

## Combination Features Analyzed

1. **Textual Feature**: TF (Term Frequency) vs TF-IDF
2. **Stem Lemma**: Text preprocessing methods (porterstemmer, lemmatizer, textblob, spacy)
3. **N-gram**: N-gram ranges (1 = unigrams, 2 = bigrams)
4. **Imbalance Handling**: Techniques (None, ProWSyn, Polynomial Fit, etc.)

**Total Datasets Analyzed**: 10 (2 projects × 5 labels)
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
3. **Aggregation**: Grouped 480 rows (10 datasets × 48 combinations) by Feature Key

### Global Metrics Calculated

For each of the 48 unique combinations:
1. **Global Avg Rank**: Mean of `rank_total_win_loss` across all datasets (lower = better)
2. **Total Wins**: Sum of `total_wins` across all datasets
3. **Total Ties**: Sum of `total_ties` across all datasets
4. **Total Losses**: Sum of `total_losses` across all datasets
5. **Loss Percentage**: (Total Losses / Total Wins + Total Ties + Total Losses) × 100
6. **Win-to-Loss Ratio**: Total Wins / Total Losses (higher = better)
7. **"General Good" Score**: Count of times the combination achieved rank ≤ 3 locally

## Findings


### Table 1: The Master Top 10 (Global Performance)

<div class="table-wrapper">

| Rank | Feature Combination | Global Avg Rank | Total W-T-L | Loss % | General Good Score | Win-to-Loss Ratio |
|------|------|------|------|------|------|------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 10.00 | 510 - 1235 - 135 | 7.18 | 4 | 3.78 |
| 2 | TF + textblob + 1 + ProWSyn | 14.60 | 432 - 1240 - 208 | 11.06 | 1 | 2.08 |
| 3 | TF-IDF + textblob + 1 + ProWSyn | 16.95 | 407 - 1232 - 241 | 12.82 | 2 | 1.69 |
| 4 | TF-IDF + porterstemmer + 2 + ProWSyn | 17.45 | 389 - 1254 - 237 | 12.61 | 2 | 1.64 |
| 5 | TF + porterstemmer + 2 + ProWSyn | 17.55 | 437 - 1216 - 227 | 12.07 | 3 | 1.93 |
| 6 | TF + porterstemmer + 2 + None | 18.05 | 247 - 1507 - 126 | 6.70 | 0 | 1.96 |
| 7 | TF-IDF + porterstemmer + 1 + Polynomial Fit | 19.55 | 383 - 1235 - 262 | 13.94 | 0 | 1.46 |
| 8 | TF-IDF + lemmatizer + 1 + Polynomial Fit | 19.85 | 340 - 1257 - 283 | 15.05 | 0 | 1.20 |
| 9 | TF-IDF + porterstemmer + 2 + Polynomial Fit | 19.90 | 358 - 1280 - 242 | 12.87 | 2 | 1.48 |
| 10 | TF-IDF + textblob + 2 + ProWSyn | 20.70 | 341 - 1268 - 271 | 14.41 | 0 | 1.26 |

</div>
## Table 2: Risk vs. Reward Analysis

### Group A: Top 5 from Safe Zone (<5% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|--------|-------------------|-----------------|

</div>

### Group B: Top 5 from Tolerable Zone (5-10% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|--------|-------------------|-----------------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 7.18% | 3.78 | 10.00 |
| 2 | TF + porterstemmer + 2 + None | 6.7% | 1.96 | 18.05 |
| 3 | TF-IDF + spacy + 1 + None | 8.46% | 1.11 | 21.85 |
| 4 | TF + spacy + 1 + None | 8.46% | 0.85 | 25.65 |
| 5 | TF-IDF + porterstemmer + 2 + None | 9.73% | 1.01 | 26.10 |

</div>

**Risk vs. Reward Insight:**
- No combinations found in Safe Zone (<5% Loss).
- Group B (Tolerable Zone 5-10% Loss) has an average Win-to-Loss Ratio of 1.74 and average Global Avg Rank of 20.33.
- These are the lowest-risk combinations available in the dataset.


## Table 3: The "Golden" Candidates

**Criteria**: All of the following must be met:
1. In the Top 10 for Global Avg Rank
2. Loss Percentage < 5%
3. "General Good" Score ≥ 5 (appeared in top 3 in at least half the datasets)

*No combinations meet all three criteria for "Golden" status.*


## Summary & Recommendations

### Key Findings

1. **Best Global Performer**: The combination with the lowest Global Avg Rank is:
   - TF + porterstemmer + 1 + ProWSyn
   - Global Avg Rank: 10.00
   - Loss %: 7.18%
   - Win-to-Loss Ratio: 3.78
   - General Good Score: 4/10

### Recommendations

1. **For Maximum Robustness**: Use combinations from the Safe Zone (<5% Loss) to minimize risk across different datasets.
2. **For Balanced Performance**: Consider Tolerable Zone combinations if higher win rates are needed for specific applications.
3. **For Proven Excellence**: The "Golden" candidates offer the best combination of global performance, low risk, and consistent top-3 appearances.

### Hypothesis Validation

**Hypothesis**: Certain feature combinations consistently perform well across different projects and classification tasks.

**Conclusion**: The global analysis reveals that combinations with porterstemmer and ProWSyn imbalance handling consistently appear in top rankings,
supporting the hypothesis that specific feature configurations provide robust performance across diverse datasets.

---

*Report generated by analyze_rq1.py following req1_plan_gem.md protocol*
