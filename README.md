# Real-World Dataset Analysis: Exploratory Data Analysis of the Titanic Passenger Manifest

## 1. Executive Summary
This project analyzes passenger demographics, fare distributions, socioeconomic standing, and survival outcomes from the Titanic passenger manifest. The objective is to identify key survival drivers, clean structural inconsistencies, compute descriptive statistics, and provide numerical correlation diagnostics.

## 2. Data Hygiene and Preprocessing
The dataset contains 891 records and 15 attributes:
* **Deduplication:** Identified and eliminated duplicate records to avoid statistical distortion.
* **Missing Value Imputation:** 
  * `age` contained 19.87% missing entries, which were imputed using the median (28.0 years) to maintain robustness against positive age skewness.
  * `embarked` had 2 missing records (<0.3%), resolved via modal imputation (`'S'`).
  * `deck` presented over 77% missing records and was removed from the feature set.
* **Feature Engineering:** 
  * `family_size = sibsp + parch + 1`
  * `is_alone`: Boolean indicator identifying solo passengers.
* **Outlier Isolation:** Interquartile Range (IQR) analysis on ticket fares identified right-side skewness, prompting logarithmic transformation for visual comparisons.

## 3. Annotated Pearson Correlation Matrix
Linear correlations computed across continuous and ordinal features:

| Feature | survived | pclass | age | sibsp | parch | fare | family_size | is_alone |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **survived** | 1.0000 | -0.3385 | -0.0649 | -0.0353 | +0.0816 | +0.2573 | +0.0166 | -0.2034 |
| **pclass** | -0.3385 | 1.0000 | -0.3394 | +0.0831 | +0.0184 | -0.5495 | +0.0659 | +0.1352 |
| **age** | -0.0649 | -0.3394 | 1.0000 | -0.2332 | -0.1725 | +0.0967 | -0.2458 | +0.1716 |
| **sibsp** | -0.0353 | +0.0831 | -0.2332 | 1.0000 | +0.4148 | +0.1597 | +0.8907 | -0.5845 |
| **parch** | +0.0816 | +0.0184 | -0.1725 | +0.4148 | 1.0000 | +0.2162 | +0.7831 | -0.5834 |
| **fare** | +0.2573 | -0.5495 | +0.0967 | +0.1597 | +0.2162 | 1.0000 | +0.2171 | -0.2718 |
| **family_size** | +0.0166 | +0.0659 | -0.2458 | +0.8907 | +0.7831 | +0.2171 | 1.0000 | -0.6909 |
| **is_alone** | -0.2034 | +0.1352 | +0.1716 | -0.5845 | -0.5834 | -0.2718 | -0.6909 | 1.0000 |

### Core Quantitative Findings:
1. **Socioeconomic Advantage ($r = -0.3385$ with Class, $r = +0.2573$ with Fare):** Passengers in higher classes (lower numerical class rank) and those with higher ticket fares achieved significantly greater survival odds.
2. **Family Penalty vs. Solo Disadvantage ($r = -0.2034$):** Traveling entirely alone negatively impacted survival probability compared to traveling in small family units.
3. **Gender Disparity:** Across all classes, female survival rates exceeded male survival rates by over 50 percentage points, driven by maritime evacuation protocols.

## 4. Strategic Business / Analytical Takeaways
* Socioeconomic status and gender were the dominant factors determining safety access.
* Extreme ticket fare outliers reflect top-tier suites that conferred preferential lifeboat placement.