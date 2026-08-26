# A Big Data Analysis Approach to Predict Stock Price Changes through Customer Sentiment

## MSc Data Science Final Project

This repository contains the implementation and datasets used for an MSc Data Science final project investigating the relationship between customer sentiment in Uber application reviews and Uber stock-market behaviour.

The project mainly uses:

- Uber customer review data.
- Uber historical stock-price data for ticker `UBER`.
- VADER and TextBlob sentiment analysis.
- Pearson and Spearman correlation analysis.
- Lag-analysis results.
- Linear Regression, Support Vector Regression, and Gradient Boosting Regression.
- A crude-oil price-based comparison dataset used as an additional reference experiment.

The main study period is from 2020 to 2023.

## Repository Structure

```text
uber-sentiment-stock-prediction-main/
|
|-- README.md
|-- LICENSE
|-- .gitattributes
|-- gitignore
|
|-- codes/
|   |-- uber_sentiment_stock_prediction.ipynb
|
|-- data/
|   |-- raw/
|   |   |-- README.md
|   |   |-- UBER_REVIEWS.csv
|   |   |-- Uber_stock_prices_yahoo.csv
|   |
|   |-- cleaned/
|       |-- README.md
|       |-- uber_reviews_2020_2023.csv
|       |-- sorted_dataset.csv
|       |-- CrudeOil_prices_with_sentiment.csv
|       |-- lag_analysis_results.csv
|
|-- images/
    |-- Average Stock Price Trend for UBER.png
    |-- Smoothed Average Stock Price Trend for UBER.png
    |-- Flow chart.png
```

## Project Workflow

The notebook `codes/uber_sentiment_stock_prediction.ipynb` contains the main analysis workflow.

1. Load the original Uber review dataset.
2. Filter reviews to the period from 1 January 2020 to 31 December 2023.
3. Detect review language using `langdetect` and retain English reviews.
4. Convert review timestamps to datetime format and sort the cleaned records.
5. Obtain Uber historical stock data using Yahoo Finance through `yfinance`.
6. Explore the average Uber stock-price trend and a 30-day moving average.
7. Perform customer-review sentiment analysis using VADER and TextBlob.
8. Compare sentiment with review ratings and calculate correlation where required.
9. Apply regression models to the aggregated sentiment data.
10. Use MAE and MSE to compare regression-model errors.
11. Generate a crude-oil price-derived scoring dataset as a comparison experiment.
12. Store lag-correlation results in `lag_analysis_results.csv`.

## Data Files

### Raw Data

`data/raw/UBER_REVIEWS.csv`

Contains the original Uber application reviews used as the main customer-sentiment source.

`data/raw/Uber_stock_prices_yahoo.csv`

Contains historical Uber stock-market data for the 2020-2023 study period.

### Cleaned and Analysis Data

`data/cleaned/uber_reviews_2020_2023.csv`

Contains Uber reviews filtered to the 2020-2023 project period.

`data/cleaned/sorted_dataset.csv`

Contains the further processed review data after English-language filtering, datetime conversion, and sorting. This file is used for sentiment analysis.

`data/cleaned/CrudeOil_prices_with_sentiment.csv`

Contains crude-oil futures data for ticker `CL=F` together with a price-derived score from 1 to 10. The score is calculated from each closing price relative to the minimum and maximum closing prices in the selected period. This is a comparison/reference experiment and should not be interpreted as natural-language customer sentiment.

`data/cleaned/lag_analysis_results.csv`

Stores saved Pearson and Spearman correlation results for multiple lag periods. The file includes lag days, correlation coefficients, and p-values.

More detailed dataset information is available in:

- `data/raw/README.md`
- `data/cleaned/README.md`

## Sentiment Analysis

### VADER

The project uses NLTK's `SentimentIntensityAnalyzer`, which implements VADER sentiment analysis. VADER produces a compound sentiment value between `-1` and `1` for each review.

The current notebook maps the VADER compound value using:

```python
mapped_score = (scores['compound'] + 1) * 4
```

This formula produces values between 0 and 8. The notebook currently stores the result in a column named `Sentiment_1_to_10`, so this naming should be considered when interpreting the output.

### TextBlob

TextBlob is also used in parts of the notebook as an alternative sentiment method. TextBlob returns a polarity value that is transformed to a positive numerical scale for comparison with review-rating behaviour.

## Correlation and Lag Analysis

Pearson and Spearman correlation methods are used to investigate relationships between variables.

- Pearson correlation measures the strength of a linear relationship.
- Spearman correlation measures whether two variables follow a similar monotonic ranking pattern and does not require the relationship to be strictly linear.

The saved file `lag_analysis_results.csv` contains results for lag periods of 0, 1, 2, 3, 5, 7, 14, and 30 days. Each row records Pearson and Spearman correlation values and their corresponding p-values.

## Regression Models

The notebook currently implements three regression algorithms.

### Linear Regression

Linear Regression is used as a simple baseline model. It estimates a straight-line relationship between the input variable and the target value. It is easy to interpret and provides a useful reference when comparing more flexible models.

### Support Vector Regression

Support Vector Regression, or SVR, is a regression version of the Support Vector Machine approach. The notebook uses an RBF kernel, allowing the model to represent non-linear patterns rather than being restricted to a straight-line relationship.

### Gradient Boosting Regression

Gradient Boosting Regression is an ensemble method that builds a sequence of decision-tree-based models. Each new model attempts to reduce errors made by the previous models, allowing the final model to represent more complex relationships.

### Current Model Use

In the current notebook, these three models are applied to yearly aggregated sentiment values and are also used in the crude-oil comparison experiment. They should therefore be described as regression experiments implemented in the project rather than as direct daily Uber stock-return prediction models unless additional stock-return modelling code is added.

## Model Evaluation

The regression models are compared using two error measures.

### Mean Absolute Error

Mean Absolute Error, or MAE, calculates the average absolute difference between predicted and observed values. Lower MAE indicates smaller average prediction error.

### Mean Squared Error

Mean Squared Error, or MSE, calculates the average squared difference between predicted and observed values. Because larger errors are squared, MSE penalises large prediction errors more strongly than MAE.

Lower MAE and MSE values indicate better predictive performance when the models are evaluated on the same target and test data.

## Crude Oil Comparison Experiment

The notebook downloads historical crude-oil futures data using ticker `CL=F` for the 2020-2023 period.

A score between 1 and 10 is assigned according to the position of each closing price within the historical price range:

```text
1 = closing prices near the minimum of the period
10 = closing prices near the maximum of the period
```

A 60-day moving average is also used to smooth this score for visualisation. Linear Regression, SVR, and Gradient Boosting Regression are then applied to yearly aggregated crude-oil scores as an additional comparison experiment.

This score is derived from market prices and is not customer or text sentiment.

## Technical Requirements

### Python

Python 3.10 or later is recommended.

### Main Libraries

```text
pandas
numpy
matplotlib
scipy
nltk
langdetect
textblob
yfinance
scikit-learn
jupyter
```

Install the main dependencies using:

```bash
pip install pandas numpy matplotlib scipy nltk langdetect textblob yfinance scikit-learn jupyter
```

## Git Large File Storage

The large Uber review datasets are managed using Git Large File Storage because they exceed normal GitHub file-size limits.

The Git LFS files include:

```text
data/raw/UBER_REVIEWS.csv
data/cleaned/uber_reviews_2020_2023.csv
data/cleaned/sorted_dataset.csv
```

After cloning the repository, run:

```bash
git lfs install
git lfs pull
```

If Git LFS is not pulled correctly, these CSV files may contain only a small pointer file rather than the complete dataset.

## How to Run the Project

### 1. Clone the Repository

```bash
git clone https://github.com/HimanshaFernando/uber-sentiment-stock-predictio
cd uber-sentiment-stock-prediction-main
```

### 2. Download Git LFS Data

```bash
git lfs install
git lfs pull
```

### 3. Create and Activate a Virtual Environment

Windows:

```bash
python -m venv .venv
.venv\Scripts\activate
```

macOS or Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 4. Install Required Libraries

```bash
pip install pandas numpy matplotlib scipy nltk langdetect textblob yfinance scikit-learn jupyter
```

### 5. Start Jupyter Notebook

```bash
jupyter notebook
```

Open:

```text
codes/uber_sentiment_stock_prediction.ipynb
```

## File-Path Note

The notebook currently uses filename-only paths such as:

```python
pd.read_csv("UBER_REVIEWS.csv")
```

The repository stores the files inside `data/raw/` and `data/cleaned/`. If the notebook is run from the `codes/` directory, the paths should be adjusted to the actual repository locations, for example:

```python
../data/raw/UBER_REVIEWS.csv
../data/raw/Uber_stock_prices_yahoo.csv
../data/cleaned/uber_reviews_2020_2023.csv
../data/cleaned/sorted_dataset.csv
../data/cleaned/CrudeOil_prices_with_sentiment.csv
../data/cleaned/lag_analysis_results.csv
```

Generated cleaned files should also be saved into `../data/cleaned/` rather than the notebook directory.

## NLTK Setup

The notebook downloads the VADER lexicon using:

```python
nltk.download('vader_lexicon')
```

An internet connection may be required the first time this resource is downloaded.

## Reproducibility

To reproduce the project correctly:

1. Download the complete Git LFS files.
2. Install all required Python libraries.
3. Use the 2020-2023 study period consistently.
4. Use the correct repository-relative file paths.
5. Download the NLTK VADER lexicon when required.
6. Run notebook cells in the intended order because later stages depend on data and variables created earlier.
7. Keep raw datasets unchanged and save processed outputs in `data/cleaned/`.

## Academic Use

This repository forms part of an MSc Data Science final project. The code, datasets, analytical outputs, model results, and documentation should be interpreted together with the accompanying dissertation.
