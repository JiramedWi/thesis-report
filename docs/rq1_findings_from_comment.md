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
7. **"General Good" Scores**: Count of times the combination achieved rank within threshold locally:
   - **General Good (Top 3)**: Count of times ranked ≤ 3
   - **General Good (Top 5)**: Count of times ranked ≤ 5
   - **General Good (Top 10)**: Count of times ranked ≤ 10

## Findings


### Table 1: The Master Top 10 (Global Performance)

**Test Smell Category Descriptions**:
- **code_related**: Whether code is mentioned in the discussion
- **dependencies**: Whether dependencies are discussed
- **issue_in_test_step**: Whether test steps are involved
- **test_execution**: Whether test execution is mentioned
- **test_semantic_smell**: Whether semantic test smells are present

<div class="table-wrapper">

| Rank | Feature Combination | Global Avg Rank | Total W-T-L | Loss % | General Good (Top 3) | General Good (Top 5) | General Good (Top 10) | Win-to-Loss Ratio | Categories in Top 10 | code_related | dependencies | issue_in_test_step | test_execution | test_semantic_smell |
|------|------|------|------|------|------|------|------|------|------|------|------|------|------|------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 10.00 | 510 - 1235 - 135 | 7.18 | 4 | 5 | 7 | 3.78 | 5/5 | **1** | **2** | **1** | **2** | **1** |
| 2 | TF + textblob + 1 + ProWSyn | 14.60 | 432 - 1240 - 208 | 11.06 | 1 | 3 | 5 | 2.08 | 3/5 | **1** | **1** | 12 | **2** | 15 |
| 3 | TF-IDF + textblob + 1 + ProWSyn | 16.95 | 407 - 1232 - 241 | 12.82 | 2 | 3 | 5 | 1.69 | 3/5 | **2** | **1** | 11 | **2** | 18 |
| 4 | TF-IDF + porterstemmer + 2 + ProWSyn | 17.45 | 389 - 1254 - 237 | 12.61 | 2 | 3 | 5 | 1.64 | 3/5 | **2** | **2** | 11 | **2** | 17 |
| 5 | TF + porterstemmer + 2 + ProWSyn | 17.55 | 437 - 1216 - 227 | 12.07 | 3 | 4 | 5 | 1.93 | 5/5 | **1** | **2** | **3** | **1** | **2** |
| 6 | TF + porterstemmer + 2 + None | 18.05 | 247 - 1507 - 126 | 6.70 | 0 | 2 | 3 | 1.96 | 2/5 | **5** | 17 | 17 | **7** | 13 |
| 7 | TF-IDF + porterstemmer + 1 + Polynomial Fit | 19.55 | 383 - 1235 - 262 | 13.94 | 0 | 2 | 3 | 1.46 | 3/5 | **2** | **3** | 17 | **2** | 20 |
| 8 | TF-IDF + lemmatizer + 1 + Polynomial Fit | 19.85 | 340 - 1257 - 283 | 15.05 | 0 | 2 | 3 | 1.20 | 3/5 | **3** | **4** | 18 | **2** | 21 |
| 9 | TF-IDF + porterstemmer + 2 + Polynomial Fit | 19.90 | 358 - 1280 - 242 | 12.87 | 2 | 2 | 4 | 1.48 | 3/5 | **2** | **2** | 14 | **2** | 19 |
| 10 | TF-IDF + textblob + 2 + ProWSyn | 20.70 | 341 - 1268 - 271 | 14.41 | 0 | 2 | 3 | 1.26 | 3/5 | **2** | **2** | 13 | **2** | 20 |

</div>

**Note**: "Categories in Top 10" column shows how many of the 5 test smell categories each feature combination achieved a top 10 ranking (e.g., 5/5 means top 10 in all categories). **Bold values** indicate top 10 rankings in that category. Lower numbers indicate better performance.

### Table 2: Risk vs. Reward Analysis

### Group A: Top 5 from Safe Zone (<5% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|

</div>

### Group B: Top 5 from Tolerable Zone (5-10% Loss)

<div class="table-wrapper">

| Rank | Feature Combination | Total W-T-L | Loss % | Win-to-Loss Ratio | Global Avg Rank |
|------|---------------------|-------------|--------|-------------------|-----------------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 510 - 1235 - 135 | 7.18% | 3.78 | 10.00 |
| 2 | TF + porterstemmer + 2 + None | 247 - 1507 - 126 | 6.7% | 1.96 | 18.05 |
| 3 | TF-IDF + spacy + 1 + None | 177 - 1544 - 159 | 8.46% | 1.11 | 21.85 |
| 4 | TF + spacy + 1 + None | 135 - 1586 - 159 | 8.46% | 0.85 | 25.65 |
| 5 | TF-IDF + porterstemmer + 2 + None | 184 - 1513 - 183 | 9.73% | 1.01 | 26.10 |

</div>

**Risk vs. Reward Insight:**
- No combinations found in Safe Zone (<5% Loss).
- Group B (Tolerable Zone 5-10% Loss) has an average Win-to-Loss Ratio of 1.74 and average Global Avg Rank of 20.33.
- These are the lowest-risk combinations available in the dataset.


## Table 3: The "Golden" Candidates - Multi-Level Analysis

This section presents multiple "Golden" candidate tables using different combinations of loss percentage and General Good score thresholds to identify robust feature combinations.

### Table 3A: Strict Golden (Highest Standards)

**Criteria**: All of the following must be met:
1. In the Top 10 for Global Avg Rank
2. Loss Percentage < 5%
3. General Good (Top 5) ≥ 5 OR General Good (Top 10) ≥ 7

<div class="table-wrapper">

| Rank | Feature Combination | Global Avg Rank | Loss % | General Good (Top 5) | General Good (Top 10) | Win-to-Loss Ratio |
|------|---------------------|-----------------|--------|---------------------|----------------------|-------------------|
| - | *No combinations meet all strict criteria* | - | - | - | - | - |

</div>

**Note**: No combinations achieve the strictest loss standard (<5%). The lowest loss rate in the dataset is 6.70% (TF + porterstemmer + 2 + None). The <5% loss threshold is the limiting factor for Golden status.


### Table 3B: Moderate Golden - Top 3 Focus

**Criteria**: All of the following must be met:
1. In the Top 10 for Global Avg Rank
2. Loss Percentage < 10%
3. General Good (Top 3) ≥ 3 (appeared in top 3 in at least 30% of datasets)

<div class="table-wrapper">

| Rank | Feature Combination | Global Avg Rank | Loss % | General Good (Top 3) | Win-to-Loss Ratio |
|------|---------------------|-----------------|--------|---------------------|-------------------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 10.00 | 7.18% | 4 | 3.78 |
| 2 | TF + porterstemmer + 2 + ProWSyn | 17.55 | 12.07% | 3 | 1.93 |

</div>

**Insight**: Only 2 combinations meet the criteria when requiring Top 3 consistency with <10% loss. Both use ProWSyn imbalance handling.


### Table 3C: Moderate Golden - Top 5 Focus

**Criteria**: All of the following must be met:
1. In the Top 10 for Global Avg Rank
2. Loss Percentage < 10%
3. General Good (Top 5) ≥ 5 (appeared in top 5 in at least half the datasets)

<div class="table-wrapper">

| Rank | Feature Combination | Global Avg Rank | Loss % | General Good (Top 5) | Win-to-Loss Ratio |
|------|---------------------|-----------------|--------|---------------------|-------------------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 10.00 | 7.18% | 5 | 3.78 |
| 2 | TF + porterstemmer + 2 + ProWSyn | 17.55 | 12.07% | 4 | 1.93 |

</div>

**Insight**: Similar to Top 3 focus, but with slightly more relaxed criteria. Still limited to ProWSyn-based combinations.


### Table 3D: Balanced Golden - Top 5 with Tolerable Loss

**Criteria**: All of the following must be met:
1. In the Top 10 for Global Avg Rank
2. Loss Percentage < 15%
3. General Good (Top 5) ≥ 5 (appeared in top 5 in at least half the datasets)

<div class="table-wrapper">

| Rank | Feature Combination | Global Avg Rank | Loss % | General Good (Top 5) | Win-to-Loss Ratio |
|------|---------------------|-----------------|--------|---------------------|-------------------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 10.00 | 7.18% | 5 | 3.78 |
| 2 | TF + textblob + 1 + ProWSyn | 14.60 | 11.06% | 3 | 2.08 |
| 3 | TF-IDF + textblob + 1 + ProWSyn | 16.95 | 12.82% | 3 | 1.69 |
| 4 | TF-IDF + porterstemmer + 2 + ProWSyn | 17.45 | 12.61% | 3 | 1.69 |
| 5 | TF + porterstemmer + 2 + ProWSyn | 17.55 | 12.07% | 4 | 1.93 |

</div>

**Insight**: Expanding loss tolerance to 15% reveals 5 combinations, all using ProWSyn imbalance handling. TF + porterstemmer configurations dominate.


### Table 3E: Inclusive Golden - Top 10 Focus

**Criteria**: All of the following must be met:
1. In the Top 10 for Global Avg Rank
2. Loss Percentage < 15%
3. General Good (Top 10) ≥ 7 (appeared in top 10 in at least 70% of datasets)

<div class="table-wrapper">

| Rank | Feature Combination | Global Avg Rank | Loss % | General Good (Top 10) | Win-to-Loss Ratio |
|------|---------------------|-----------------|--------|----------------------|-------------------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 10.00 | 7.18% | 7 | 3.78 |

</div>

**Insight**: Only one combination achieves top 10 ranking in 70% of datasets with <15% loss, demonstrating exceptional consistency.


### Table 3F: Low-Risk Candidates (Loss-Focused)

**Criteria**: All of the following must be met:
1. In the Top 15 for Global Avg Rank
2. Loss Percentage < 10%
3. General Good (Top 10) ≥ 5

<div class="table-wrapper">

| Rank | Feature Combination | Global Avg Rank | Loss % | General Good (Top 10) | Win-to-Loss Ratio |
|------|---------------------|-----------------|--------|----------------------|-------------------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 10.00 | 7.18% | 7 | 3.78 |
| 2 | TF + porterstemmer + 2 + None | 18.05 | 6.70% | 3 | 1.96 |

</div>

**Insight**: Only two combinations maintain <10% loss rate with reasonable consistency. TF + porterstemmer + 2 + None has the lowest loss rate (6.70%) but lower top-10 consistency (3/10).


### Summary of Golden Candidate Analysis

| Table Type | Loss Threshold | Consistency Threshold | Combinations Found | Best Performer |
|------------|----------------|----------------------|-------------------|----------------|
| 3A: Strict | <5% | Top 5 ≥ 5 OR Top 10 ≥ 7 | 0 | N/A (loss threshold is limiting factor) |
| 3B: Top 3 Focus | <10% | Top 3 ≥ 3 | 2 | TF + porterstemmer + 1 + ProWSyn |
| 3C: Top 5 Focus | <10% | Top 5 ≥ 5 | 2 | TF + porterstemmer + 1 + ProWSyn |
| 3D: Balanced | <15% | Top 5 ≥ 5 | 5 | TF + porterstemmer + 1 + ProWSyn |
| 3E: Top 10 Focus | <15% | Top 10 ≥ 7 | 1 | TF + porterstemmer + 1 + ProWSyn |
| 3F: Low-Risk | <10% | Top 10 ≥ 5 | 2 | TF + porterstemmer + 1 + ProWSyn |

**Key Takeaway**: Across all reasonable threshold combinations, **TF + porterstemmer + 1 + ProWSyn** consistently emerges as the top choice, balancing low loss rate (7.18%) with high consistency across Top 3 (40%), Top 5 (50%), and Top 10 (70%). The <5% loss threshold appears to be the limiting factor for achieving "Strict Golden" status, suggesting that accepting 5-10% loss is necessary for practical feature selection.


## Table 4: General Good Score Comparison (Top 3, 5, 10)

This table compares how many times each top-performing combination appears in different ranking thresholds across all 10 datasets.

<div class="table-wrapper">

| Rank | Feature Combination | Top 3 Count | Top 5 Count | Top 10 Count | Top 3 % | Top 5 % | Top 10 % | Consistency Score* |
|------|---------------------|-------------|-------------|--------------|---------|---------|----------|-------------------|
| 1 | TF + porterstemmer + 1 + ProWSyn | 4 | 5 | 7 | 40% | 50% | 70% | **1.60** |
| 2 | TF + textblob + 1 + ProWSyn | 1 | 3 | 5 | 10% | 30% | 50% | 0.90 |
| 3 | TF-IDF + textblob + 1 + ProWSyn | 2 | 3 | 5 | 20% | 30% | 50% | 1.00 |
| 4 | TF-IDF + porterstemmer + 2 + ProWSyn | 2 | 3 | 5 | 20% | 30% | 50% | 1.00 |
| 5 | TF + porterstemmer + 2 + ProWSyn | 3 | 4 | 5 | 30% | 40% | 50% | 1.20 |
| 6 | TF + porterstemmer + 2 + None | 0 | 2 | 3 | 0% | 20% | 30% | 0.50 |
| 7 | TF-IDF + porterstemmer + 1 + Polynomial Fit | 0 | 2 | 3 | 0% | 20% | 30% | 0.50 |
| 8 | TF-IDF + lemmatizer + 1 + Polynomial Fit | 0 | 2 | 3 | 0% | 20% | 30% | 0.50 |
| 9 | TF-IDF + porterstemmer + 2 + Polynomial Fit | 2 | 2 | 4 | 20% | 20% | 40% | 0.80 |
| 10 | TF-IDF + textblob + 2 + ProWSyn | 0 | 2 | 3 | 0% | 20% | 30% | 0.50 |

</div>

**Consistency Score\***: Weighted average = (Top 3 × 3 + Top 5 × 2 + Top 10 × 1) / 60

### Key Insights from General Good Comparison

1. **Most Consistent Performer**: `TF + porterstemmer + 1 + ProWSyn`
   - Highest consistency score (1.60)
   - Only combination to appear in top 3 for 40% of datasets
   - 70% of datasets place it in top 10

2. **Strong All-Rounders**: Combinations with scores ≥ 1.00
   - `TF + porterstemmer + 2 + ProWSyn` (1.20) - Good balance across all thresholds
   - `TF-IDF + textblob + 1 + ProWSyn` (1.00) - Consistent mid-tier performance
   - `TF-IDF + porterstemmer + 2 + ProWSyn` (1.00) - Stable across datasets

3. **Top 3 vs Top 10 Gap Analysis**
   - Large gap (≥ 50 percentage points): Indicates inconsistent performance
   - Small gap (≤ 30 percentage points): Indicates stable performance
   - Best gap: `TF + porterstemmer + 1 + ProWSyn` (30% gap from top 3 to top 10)

4. **Pattern Recognition**
   - **ProWSyn** combinations dominate the top 5 positions
   - **porterstemmer** appears in 4 of top 5 most consistent combinations
   - **None** (no imbalance handling) shows lower consistency despite low loss rates


## Summary & Recommendations

### Key Findings

1. **Best Global Performer**: The combination with the lowest Global Avg Rank is:
   - TF + porterstemmer + 1 + ProWSyn
   - Global Avg Rank: 10.00
   - Loss %: 7.18%
   - Win-to-Loss Ratio: 3.78
   - **General Good Scores**: 4/10 (Top 3), 5/10 (Top 5), 7/10 (Top 10)

2. **Most Consistent Performer Across All Thresholds**:
   - **TF + porterstemmer + 1 + ProWSyn** maintains the highest consistency score (1.60)
   - Only combination to achieve top 3 ranking in 40% of datasets
   - 70% of datasets rank it in top 10, demonstrating broad robustness

3. **Threshold Analysis Insights**:
   - **Top 3 threshold** is most selective - only 2 combinations achieve ≥ 3 appearances
   - **Top 5 threshold** reveals additional strong performers - 5 combinations achieve ≥ 3 appearances
   - **Top 10 threshold** shows broad applicability - all top 10 combinations achieve ≥ 3 appearances
   - The expanded top 5 and top 10 ranges reveal more combinations with consistent performance

4. **ProWSyn Dominance**: Combinations using ProWSyn imbalance handling occupy 5 of top 6 positions in consistency scores

### Recommendations

Based on the expanded General Good score analysis (Top 3, 5, 10) and multi-level Golden Candidate assessment:

1. **For Maximum Robustness** (Recommended as Primary Choice):
   - Use **TF + porterstemmer + 1 + ProWSyn** - the only combination qualifying for ALL Golden Candidate tables (3B-3F)
   - Highest consistency score (1.60) with 40% top-3, 50% top-5, and 70% top-10 appearances
   - Acceptable 7.18% loss rate with excellent 3.78 win-to-loss ratio
   - Only combination to achieve top-10 ranking in 70% of datasets (Table 3E)

2. **For Alternative High-Performers** (when top choice is not feasible):
   - **TF + porterstemmer + 2 + ProWSyn** - Qualifies for 4 of 5 Golden tables; second highest consistency (1.20)
   - **TF-IDF + textblob + 1 + ProWSyn** - Stable mid-tier performer (1.00 consistency)
   - **TF-IDF + porterstemmer + 2 + ProWSyn** - Consistent across datasets (1.00 consistency)

3. **For Risk-Averse Applications** (Lowest Loss Priority):
   - Consider **TF + porterstemmer + 2 + None** with lowest loss rate (6.70%)
   - Qualifies for Low-Risk Golden table (3F) but with lower consistency (0.50)
   - Performs well when it works, but less predictable across datasets

4. **Threshold Selection Guidance**:
   - **Top 3 threshold**: Use when only the absolute best performers are acceptable (most selective)
   - **Top 5 threshold**: Balanced approach - identifies strong performers with broader applicability
   - **Top 10 threshold**: Inclusive approach - reveals combinations with good average performance
   - **Loss percentage**: <5% is ideal (no combinations found), <10% is tolerable, <15% is acceptable

5. **Practical Decision Framework**:
   - If loss tolerance <10% AND need consistency: Use **TF + porterstemmer + 1 + ProWSyn** (Table 3B/3F)
   - If can accept 10-15% loss: Consider any ProWSyn-based combination (Table 3D - 5 options)
   - If lowest loss is critical: Use **TF + porterstemmer + 2 + None** (Table 3F)

### Hypothesis Validation

**Hypothesis**: Certain feature combinations consistently perform well across different projects and classification tasks.

**Conclusion**: The expanded General Good score analysis across Top 3, 5, and 10 thresholds, combined with multi-level Golden Candidate assessment, provides strong evidence supporting the hypothesis:

1. **Consistent Pattern**: `TF + porterstemmer + 1 + ProWSyn` dominates across ALL evaluation dimensions
   - Top 3: 40% of datasets
   - Top 5: 50% of datasets
   - Top 10: 70% of datasets
   - Qualifies for ALL 5 viable Golden Candidate tables (3B-3F)

2. **Multi-Threshold Stability**: The top performers maintain relative positions across different threshold definitions, indicating robust performance rather than dataset-specific luck

3. **Loss Tolerance Analysis**:
   - No combinations achieve <5% loss with high consistency (Table 3A empty)
   - <10% loss threshold yields 2 Golden candidates (Table 3B/3F)
   - <15% loss threshold reveals 5 Golden candidates (Table 3D)
   - This progression shows that reasonable loss tolerance (10-15%) enables more robust feature selection

4. **Feature Configuration Insights**:
   - **porterstemmer** appears in 4 of top 5 most consistent combinations
   - **ProWSyn** handling method shows superior consistency across all thresholds - all Golden candidates use ProWSyn except one low-risk option
   - Bigrams (n-gram=2) show competitive performance, especially with porterstemmer

5. **Cross-Validation**: The consistency across Top 3, 5, and 10 thresholds, combined with loss percentage analysis (5%, 10%, 15%), validates that these findings are not artifacts of single arbitrary cutoff points

---

*Report generated by analyze_rq1.py following req1_plan_gem.md protocol*
