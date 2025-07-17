# INFO411 Data Mining and Knowledge Discovery - Project 4

## Document Classification (Web Spam Detection)

### Assignment Overview

This repository contains the implementation of Project 4 for INFO411 Data Mining and Knowledge Discovery course. The project focuses on web spam detection using the UK2007 benchmark dataset, implementing and comparing different classification methods across multiple feature sets.

### Problem Definition

**Task**: Document Classification (Web Spam Detection)

Web spam refers to activities intended to mislead search engines into believing that a particular web page has high authority value for specific queries, while the page may contain little or no relevant information. Search engines rank URLs based on:

1. **Content relevance** - how well page content matches the query
2. **Page popularity** - typically measured by link-based metrics

### Background

Spam techniques are classified into two main categories:

#### Link-based Spam

- Web pages linked by many other sites to artificially increase popularity
- Often involves "link farms" where links are automatically generated
- Exploits popularity-based ranking factors

#### Content-based Spam

- Web pages contain terms that are visually hidden from users
- Terms are irrelevant to actual content but indexable by search engines
- Increases probability of appearing in search results

### Dataset: UK2007 Benchmark

The UK2007 dataset is a large collection of annotated spam/nonspam hosts:

- **Size**: 105,896,555 pages across 114,529 hosts in the .UK domain
- **Host IDs**: Numbered from 0 to 114,528 (same ordering as in uk-2007-05.hostnames.txt.gz)
- **Labeling**: Tagged at host level by volunteers
- **Labels**: Available from [WEBSPAM-UK2007](https://chato.cl/webspam/datasets/uk2007/)
- **Features**: Available from [UK2007 Features](https://chato.cl/webspam/datasets/uk2007/features/)

### Data Downloads & Feature Sets

This project uses re-computed feature sets provided for the Web Spam Challenge 2008. All feature sets are available in CSV, Matlab, and ARFF (Weka) formats.

#### Feature Set 1: Direct Features

- **File**: `1.uk-2007-05.obvious_features.csv` (renamed from downloaded file)
- **Download**: [uk-2007-05.obvious_features.csv.gz](https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.obvious_features.csv.gz) **(1.3 MB)**
- **Description**: Computed from graph files, includes two direct, obvious features
- **Content**:
  - Number of pages in the host
  - Number of characters in the host name

#### Feature Set 2a: Link-based Features (Raw)

- **File**: Available but not used in this project
- **Download**: [uk-2007-05.link_based_features.csv.gz](https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.link_based_features.csv.gz) **(19 MB)**
- **Description**: Raw link-based features computed from graph files
- **Content**: In-degree, out-degree, PageRank, edge reciprocity, assortativity coefficient, TrustRank, Truncated PageRank, supporter estimates, etc.
- **Additional File**: [uk-2007-05.homepageuid_maxpruid.csv.gz](https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.homepageuid_maxpruid.csv.gz) (home page and max PageRank page URL-IDs)

#### Feature Set 2b: Transformed Link-based Features

- **File**: `2.uk-2007-05.link_based_features_transformed.csv` (renamed from downloaded file)
- **Download**: [uk-2007-05.link_based_features_transformed.csv.gz](https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.link_based_features_transformed.csv.gz) **(68 MB)**
- **Description**: Numeric transformations of link-based features proven more effective for classification
- **Content**:
  - Feature ratios (e.g., Indegree/PageRank, TrustRank/PageRank)
  - Logarithmic transformations of several features
  - Optimized transformations for better classification performance

#### Feature Set 3a: Content-based Features

- **File**: `3.uk-2007-05.content_based_features.csv` (renamed from downloaded file)
- **Download**: [uk-2007-05.content_based_features.csv.gz](https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.content_based_features.csv.gz) **(47 MB)**
- **Description**: Features computed from page content summaries
- **Content**:
  - Number of words in home page
  - Average word length
  - Average title length
  - Additional content-based statistics for sample pages on each host

### Label Files

The spam/nonspam labels are provided in two sets:

- **SET1**: `WEBSPAM-UK2007-SET1-labels.txt` (training set, ~2/3 of data)
- **SET2**: `WEBSPAM-UK2007-SET2-labels.txt` (test set, ~1/3 of data)

**Label Statistics:**

- **SET1**: 3,776 nonspam, 222 spam, 277 undecided
- **SET2**: 1,933 nonspam, 122 spam, 149 undecided

### Data Preparation Instructions

For users who want to download and prepare the data themselves:

1. **Download Feature Sets:**

   ```bash
   # Download and extract feature set 1 (Direct Features)
   wget https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.obvious_features.csv.gz
   gunzip uk-2007-05.obvious_features.csv.gz
   mv uk-2007-05.obvious_features.csv 1.uk-2007-05.obvious_features.csv

   # Download and extract feature set 2b (Transformed Link-based Features)
   wget https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.link_based_features_transformed.csv.gz
   gunzip uk-2007-05.link_based_features_transformed.csv.gz
   mv uk-2007-05.link_based_features_transformed.csv 2.uk-2007-05.link_based_features_transformed.csv

   # Download and extract feature set 3a (Content-based Features)
   wget https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.content_based_features.csv.gz
   gunzip uk-2007-05.content_based_features.csv.gz
   mv uk-2007-05.content_based_features.csv 3.uk-2007-05.content_based_features.csv
   ```

2. **Download Label Files:**

   ```bash
   # Download label files from the main dataset page
   wget https://chato.cl/webspam/datasets/uk2007/WEBSPAM-UK2007-SET1-labels.txt
   wget https://chato.cl/webspam/datasets/uk2007/WEBSPAM-UK2007-SET2-labels.txt
   ```

3. **Optional - Download Additional Files:**

   ```bash
   # Host names file (if needed for reference)
   wget https://chato.cl/webspam/datasets/uk2007/uk-2007-05.hostnames.txt.gz

   # Raw link-based features (if you want to compare with transformed features)
   wget https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.link_based_features.csv.gz

   # Home page and max PageRank page mapping
   wget https://chato.cl/webspam/datasets/uk2007/features/uk-2007-05.homepageuid_maxpruid.csv.gz
   ```

### Project Objectives

1. **Feature Set Evaluation**: Identify which feature set provides the best predictive power
2. **Method Selection**: Deploy the most suitable classification methods for each feature set
3. **Performance Ranking**: Rank feature sets by quality using AUC (Area Under ROC Curve)
4. **Comprehensive Analysis**: Compare results and explain findings
5. **Combination Analysis**: Evaluate performance of combined feature sets

### Requirements & Analysis Framework

#### 1. Dataset Description

- Present general properties and characteristics of the UK2007 dataset
- Provide comprehensive exploratory data analysis

#### 2. Classification Method Deployment

- Deploy appropriate classification methods to each feature set
- Justify method selection for each feature set
- Present and analyze results
- Discuss strengths and weaknesses in context of web spam detection

#### 3. Feature Set Ranking

- Rank feature sets by predictive performance
- Use AUC as primary comparison metric
- Expected ranking based on domain knowledge:
  1. Content-based Features (best)
  2. Link-based Features
  3. Direct Features (poorest)

#### 4. Results Analysis & Comparison

- Comprehensive analysis using AUC comparisons
- Detailed explanation of findings
- Discussion of classification method performance

#### 5. Feature Set Combinations

- Deploy classification methods on combined feature sets
- Present results with qualitative comparisons
- Analyze performance improvements/degradations

#### 6. Discovery & Insights

- Summarize new and interesting discoveries
- Discuss implications for web spam detection
- Provide recommendations for future work

### Implementation

#### Main Analysis File

- **Source Code**: `output.rmd` (RMarkdown file)
- **Generated Report**: `output.html` (Final analysis report)

#### Required R Packages

```r
# Install required packages if not already installed
packages <- c("tidyverse", "skimr", "corrplot", "scales",
              "kableExtra", "caret", "pROC", "randomForest", "e1071")
install.packages(packages)
```

#### Rendering the Report

**In R/RStudio Console:**

```r
rmarkdown::render("output.rmd")
```

**In Terminal:**

```sh
Rscript -e "rmarkdown::render('output.rmd')"
```

### Project Structure

```
├── README.md                                           # This file
├── output.rmd                                         # Main analysis code
├── output.html                                        # Generated report
├── 1.uk-2007-05.obvious_features.csv                 # Direct features
├── 2.uk-2007-05.link_based_features_transformed.csv  # Link-based features
├── 3.uk-2007-05.content_based_features.csv           # Content-based features
├── WEBSPAM-UK2007-SET1-labels.txt                    # Training labels
└── WEBSPAM-UK2007-SET2-labels.txt                    # Test labels
```

### Methodology

The analysis follows a systematic approach:

1. **Data Exploration**: Comprehensive EDA of each feature set
2. **Preprocessing**: Data cleaning, handling missing values, feature scaling
3. **Model Selection**: Justify and implement appropriate classification algorithms
4. **Evaluation**: Use cross-validation and AUC metrics for robust comparison
5. **Feature Combination**: Systematic evaluation of combined feature sets
6. **Results Interpretation**: Domain-specific analysis of findings

### Evaluation Metrics

- **Primary Metric**: AUC (Area Under ROC Curve)
- **Additional Metrics**: Accuracy, Precision, Recall, F1-Score
- **Validation**: Cross-validation and train/test split evaluation

### Expected Outcomes

Based on domain knowledge and previous research:

1. **Content-based features** should perform best (direct relevance to spam content)
2. **Link-based features** should show moderate performance (link farm detection)
3. **Direct features** should have limited predictive power (too simple)
4. **Combined features** should outperform individual feature sets

### References

- Manning, C., Raghavan, P., & Schütze, H. (2008). "An introduction to information retrieval", Cambridge University Press
- Brin, S., & Page, L. (1998). The anatomy of a large–scale hypertextual Web search engine. Computer Networks and ISDN Systems, 30, 107–117.
- Gyöngyi, Z., & Garcia-Molina, H. (2005). "Web spam taxonomy". Adversarial Information Retrieval on the Web

### Course Information

- **Course**: INFO411 Data Mining and Knowledge Discovery
- **Project**: Project 4 - Document Classification (Web Spam Detection)
- **Institution**: University of Wollongong
- **Dataset**: UK2007 Web Spam Benchmark

---

For questions or issues with the analysis, please refer to the detailed implementation in `output.rmd` or the generated report `output.html`.
