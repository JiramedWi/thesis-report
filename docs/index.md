# Thesis Report

Welcome to my thesis research report on **test smell detection using machine learning techniques**.

## Research Overview

This research analyzes test smell detection in open-source projects using various machine learning combinations across different feature extraction methods and text preprocessing techniques.

### Projects Analyzed
- **Apache Flink**
- **Apache Hive**

### Classification Labels
- `code_related` - Whether code is mentioned in the discussion
- `dependencies` - Whether dependencies are discussed
- `issue_in_test_step` - Whether test steps are involved
- `test_execution` - Whether test execution is mentioned
- `test_semantic_smell` - Whether semantic test smells are present

## Research Questions

### [RQ1: Best-Performing Combinations Analysis](rq1_findings)

Analysis of top-performing feature combinations for test smell detection across projects and classification tasks.

**Key Findings:**
- Best Performing: **TF + porterstemmer + unigrams + ProWSyn**
- Worst Performing: **TF-IDF + lemmatizer + unigrams + ProWSyn**
- 48 unique combinations analyzed across 10 datasets

---

*Last updated: December 2025*
