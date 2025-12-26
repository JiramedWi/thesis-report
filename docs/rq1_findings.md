# Research Question 1: Best-Performing Combinations Analysis

<!-- Scrollable table wrapper -->
<style>
.table-wrapper { overflow-x: auto; }
.table-wrapper table { border-collapse: collapse; }
.table-wrapper th, .table-wrapper td { white-space: nowrap; padding: 8px 16px; }
</style>

## Overview

**Analysis Date**: 2025-12-26

This analysis identifies the top-performing feature combinations for test smell detection
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

## Analysis Methodology

### Performance Metrics

Combinations are ranked based on:
1. **rank_total_win_loss** - Primary metric (lower rank = better performance)
2. **total_win_loss** - Secondary metric (higher = better)
3. **total_wins** - Number of wins across all metrics
4. **total_losses** - Number of losses across all metrics

### Ranking Criteria

- Top combinations: Sorted by `rank_total_win_loss` (ascending), then `total_win_loss` (descending)
- Bottom combinations: Sorted by `rank_total_win_loss` (descending), then `total_win_loss` (ascending)

## Findings

### Top 10 Consistently Best-Performing Combinations

The following combinations consistently appeared in top 5 rankings across multiple datasets:

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Appearance Count | Datasets |
|------|-----------------|------------|--------|---------------|------------------|----------|
| 1 | TF | porterstemmer | 1 | ProWSyn | 6/10 | flink_dependencies, flink_issue_in_test_step, flink_test_semantic_smell, hive_issue_in_test_step, hive_test_execution |
| 2 | TF | porterstemmer | 2 | ProWSyn | 4/10 | flink_dependencies, flink_test_semantic_smell, hive_test_execution, hive_test_semantic_smell |
| 3 | TF-IDF | porterstemmer | 2 | ProWSyn | 4/10 | flink_dependencies, flink_test_execution, flink_test_semantic_smell, hive_test_semantic_smell |
| 4 | TF-IDF | textblob | 1 | ProWSyn | 3/10 | flink_dependencies, flink_test_execution, hive_issue_in_test_step |
| 5 | TF | spacy | 2 | Polynomial Fit | 3/10 | flink_test_execution, hive_code_related, hive_issue_in_test_step |
| 6 | TF | porterstemmer | 1 | Polynomial Fit | 3/10 | flink_test_semantic_smell, hive_code_related, hive_issue_in_test_step |
| 7 | TF-IDF | porterstemmer | 2 | Polynomial Fit | 2/10 | flink_code_related, hive_code_related |
| 8 | TF | spacy | 2 | ProWSyn | 2/10 | flink_code_related, hive_dependencies |
| 9 | TF-IDF | lemmatizer | 2 | Polynomial Fit | 2/10 | flink_code_related, flink_issue_in_test_step |
| 10 | TF | textblob | 1 | ProWSyn | 2/10 | flink_issue_in_test_step, flink_test_execution |

</div>

### Top 10 Consistently Worst-Performing Combinations

The following combinations consistently appeared in bottom 5 rankings across multiple datasets:

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Appearance Count |
|------|-----------------|------------|--------|---------------|------------------|
| 1 | TF-IDF | lemmatizer | 1 | ProWSyn | 4/10 |
| 2 | TF-IDF | spacy | 1 | ProWSyn | 3/10 |
| 3 | TF | spacy | 2 | Polynomial Fit | 3/10 |
| 4 | TF | textblob | 1 | Polynomial Fit | 3/10 |
| 5 | TF-IDF | lemmatizer | 2 | ProWSyn | 2/10 |
| 6 | TF-IDF | textblob | 1 | Polynomial Fit | 2/10 |
| 7 | TF-IDF | spacy | 2 | nan | 2/10 |
| 8 | TF | spacy | 1 | ProWSyn | 2/10 |
| 9 | TF | spacy | 2 | ProWSyn | 2/10 |
| 10 | TF-IDF | spacy | 2 | ProWSyn | 2/10 |

</div>

## Top 5 Combinations by Dataset

### Flink Code Related

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Win-Loss Rank | Total Win-Loss | Total Wins | Total Ties | Total Losses |
|------|-----------------|------------|--------|---------------|---------------|---------------|------------|-------------|--------------|
| 1 | TF-IDF | porterstemmer | 2 | Polynomial Fit | 1 | 39 | 44 | 139 | 5 |
| 2 | TF-IDF | spacy | 2 | Polynomial Fit | 2 | 30 | 41 | 136 | 11 |
| 3 | TF | spacy | 2 | ProWSyn | 2 | 30 | 45 | 128 | 15 |
| 4 | TF-IDF | lemmatizer | 2 | Polynomial Fit | 4 | 26 | 37 | 140 | 11 |
| 5 | TF-IDF | lemmatizer | 1 | nan | 5 | 25 | 25 | 163 | 0 |

</div>

### Flink Dependencies

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Win-Loss Rank | Total Win-Loss | Total Wins | Total Ties | Total Losses |
|------|-----------------|------------|--------|---------------|---------------|---------------|------------|-------------|--------------|
| 1 | TF-IDF | lemmatizer | 1 | ProWSyn | 1 | 52 | 57 | 126 | 5 |
| 2 | TF | porterstemmer | 2 | ProWSyn | 2 | 47 | 61 | 113 | 14 |
| 3 | TF-IDF | porterstemmer | 2 | ProWSyn | 3 | 46 | 54 | 126 | 8 |
| 4 | TF | porterstemmer | 1 | ProWSyn | 4 | 41 | 55 | 119 | 14 |
| 5 | TF-IDF | textblob | 1 | ProWSyn | 5 | 39 | 45 | 137 | 6 |

</div>

### Flink Issue In Test Step

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Win-Loss Rank | Total Win-Loss | Total Wins | Total Ties | Total Losses |
|------|-----------------|------------|--------|---------------|---------------|---------------|------------|-------------|--------------|
| 1 | TF-IDF | lemmatizer | 2 | ProWSyn | 1 | 115 | 122 | 59 | 7 |
| 2 | TF | porterstemmer | 1 | ProWSyn | 2 | 113 | 117 | 67 | 4 |
| 3 | TF-IDF | textblob | 2 | Polynomial Fit | 3 | 93 | 98 | 85 | 5 |
| 4 | TF | textblob | 1 | ProWSyn | 4 | 84 | 94 | 84 | 10 |
| 5 | TF-IDF | lemmatizer | 2 | Polynomial Fit | 5 | 78 | 78 | 110 | 0 |

</div>

### Flink Test Execution

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Win-Loss Rank | Total Win-Loss | Total Wins | Total Ties | Total Losses |
|------|-----------------|------------|--------|---------------|---------------|---------------|------------|-------------|--------------|
| 1 | TF | textblob | 1 | ProWSyn | 1 | 59 | 66 | 115 | 7 |
| 2 | TF-IDF | textblob | 1 | ProWSyn | 2 | 47 | 64 | 107 | 17 |
| 3 | TF | spacy | 2 | Polynomial Fit | 4 | 43 | 54 | 123 | 11 |
| 4 | TF-IDF | porterstemmer | 2 | ProWSyn | 4 | 43 | 46 | 139 | 3 |
| 5 | TF | lemmatizer | 2 | Polynomial Fit | 5 | 36 | 52 | 120 | 16 |

</div>

### Flink Test Semantic Smell

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Win-Loss Rank | Total Win-Loss | Total Wins | Total Ties | Total Losses |
|------|-----------------|------------|--------|---------------|---------------|---------------|------------|-------------|--------------|
| 1 | TF | porterstemmer | 1 | ProWSyn | 1 | 47 | 48 | 139 | 1 |
| 2 | TF | porterstemmer | 2 | ProWSyn | 2 | 36 | 42 | 140 | 6 |
| 3 | TF | porterstemmer | 1 | Polynomial Fit | 4 | 34 | 50 | 122 | 16 |
| 4 | TF | textblob | 2 | ProWSyn | 4 | 34 | 40 | 142 | 6 |
| 5 | TF-IDF | porterstemmer | 2 | ProWSyn | 5 | 32 | 39 | 142 | 7 |

</div>

### Hive Code Related

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Win-Loss Rank | Total Win-Loss | Total Wins | Total Ties | Total Losses |
|------|-----------------|------------|--------|---------------|---------------|---------------|------------|-------------|--------------|
| 1 | TF-IDF | porterstemmer | 2 | Polynomial Fit | 1 | 48 | 57 | 122 | 9 |
| 2 | TF | spacy | 2 | Polynomial Fit | 2 | 46 | 55 | 124 | 9 |
| 3 | TF | textblob | 1 | Polynomial Fit | 3 | 39 | 51 | 125 | 12 |
| 4 | TF-IDF | lemmatizer | 1 | Polynomial Fit | 4 | 36 | 45 | 134 | 9 |
| 5 | TF | porterstemmer | 1 | Polynomial Fit | 6 | 34 | 45 | 132 | 11 |

</div>

### Hive Dependencies

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Win-Loss Rank | Total Win-Loss | Total Wins | Total Ties | Total Losses |
|------|-----------------|------------|--------|---------------|---------------|---------------|------------|-------------|--------------|
| 1 | TF | textblob | 2 | nan | 1 | 48 | 48 | 140 | 0 |
| 2 | TF | spacy | 2 | ProWSyn | 2 | 31 | 39 | 141 | 8 |
| 3 | TF | porterstemmer | 2 | nan | 4 | 30 | 35 | 148 | 5 |
| 4 | TF | textblob | 1 | nan | 4 | 30 | 34 | 150 | 4 |
| 5 | TF | lemmatizer | 2 | ProWSyn | 5 | 28 | 35 | 146 | 7 |

</div>

### Hive Issue In Test Step

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Win-Loss Rank | Total Win-Loss | Total Wins | Total Ties | Total Losses |
|------|-----------------|------------|--------|---------------|---------------|---------------|------------|-------------|--------------|
| 1 | TF-IDF | textblob | 1 | ProWSyn | 1 | 46 | 54 | 126 | 8 |
| 2 | TF | porterstemmer | 1 | ProWSyn | 2 | 45 | 54 | 125 | 9 |
| 3 | TF | porterstemmer | 1 | Polynomial Fit | 3 | 44 | 51 | 130 | 7 |
| 4 | TF | spacy | 2 | Polynomial Fit | 4 | 42 | 50 | 130 | 8 |
| 5 | TF | textblob | 2 | Polynomial Fit | 5 | 38 | 49 | 128 | 11 |

</div>

### Hive Test Execution

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Win-Loss Rank | Total Win-Loss | Total Wins | Total Ties | Total Losses |
|------|-----------------|------------|--------|---------------|---------------|---------------|------------|-------------|--------------|
| 1 | TF | porterstemmer | 2 | ProWSyn | 1 | 66 | 74 | 106 | 8 |
| 2 | TF | porterstemmer | 1 | ProWSyn | 2 | 55 | 63 | 117 | 8 |
| 3 | TF | lemmatizer | 2 | ProWSyn | 3 | 47 | 51 | 133 | 4 |
| 4 | TF | lemmatizer | 1 | ProWSyn | 4 | 45 | 56 | 121 | 11 |
| 5 | TF | spacy | 1 | ProWSyn | 4 | 45 | 58 | 117 | 13 |

</div>

### Hive Test Semantic Smell

<div class="table-wrapper">

| Rank | Textual Feature | Stem Lemma | N-gram | Imba Handling | Win-Loss Rank | Total Win-Loss | Total Wins | Total Ties | Total Losses |
|------|-----------------|------------|--------|---------------|---------------|---------------|------------|-------------|--------------|
| 1 | TF-IDF | porterstemmer | 2 | ProWSyn | 1 | 71 | 79 | 101 | 8 |
| 2 | TF-IDF | porterstemmer | 1 | ProWSyn | 2 | 57 | 65 | 115 | 8 |
| 3 | TF | lemmatizer | 1 | ProWSyn | 3 | 54 | 66 | 110 | 12 |
| 4 | TF | porterstemmer | 1 | ProWSyn | 4 | 51 | 61 | 117 | 10 |
| 5 | TF | porterstemmer | 2 | ProWSyn | 5 | 49 | 58 | 121 | 9 |

</div>

## Hypothesis Validation

### Hypothesis

*Certain combinations consistently appear in the top K and bottom K rankings across different projects and classification tasks.*

### Conclusion

**SUPPORTED**. The analysis shows that certain combinations consistently appear in top/bottom rankings across multiple datasets.

- Average appearance of top 10 combinations in top 5: **3.10/10 datasets**
- Average appearance of bottom 10 combinations in bottom 5: **2.50/10 datasets**

### Key Insights

1. **Best Performing Combination**: TF + porterstemmer + 1 + ProWSyn
2. **Worst Performing Combination**: TF-IDF + lemmatizer + 1 + ProWSyn
3. **Total Unique Combinations Analyzed**: 48

---

*Report generated by analyze_rq1.py*