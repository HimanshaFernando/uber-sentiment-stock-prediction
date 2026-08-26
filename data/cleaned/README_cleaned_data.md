# Cleaned Data

## Overview

The `data/cleaned/` folder contains the processed datasets and saved analysis results used in the project.

### `uber_reviews_2020_2023.csv`

Uber reviews filtered from the original dataset to the period from 1 January 2020 to 31 December 2023.

### `sorted_dataset.csv`

The further cleaned review dataset. Non-English reviews are removed, timestamps are converted to datetime format, and the remaining records are sorted before sentiment analysis.

### `CrudeOil_prices_with_sentiment.csv`

Historical crude-oil futures data for `CL=F` with a 1-to-10 score calculated from the relative closing-price level. It is used as a comparison experiment in the project.

### `lag_analysis_results.csv`

Saved lag-analysis results containing Pearson and Spearman correlations and p-values for different lag periods.

## Source

The review-based cleaned files are created from:

```text
data/raw/UBER_REVIEWS.csv
```

The crude-oil file is generated from Yahoo Finance data using `yfinance`.
