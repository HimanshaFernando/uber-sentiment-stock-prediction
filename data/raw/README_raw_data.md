# Raw Data

## Purpose of This Directory

The `data/raw/` directory stores the original or initially collected datasets used by the project before the main review-cleaning process is applied.

The project uses two raw data sources:

```text
UBER_REVIEWS.csv
Uber_stock_prices_yahoo.csv
```

The review data represents customer opinions about the Uber application, while the stock dataset contains historical market information for Uber Technologies, Inc.

Raw data should normally be kept unchanged so that the cleaning and analysis process can be reproduced from the original inputs.

## UBER_REVIEWS.csv

### Description

`UBER_REVIEWS.csv` is the original Uber application review dataset used in the project.

The notebook output shows that the dataset contains:

```text
1,455,477 rows
9 columns
```

The original dataset includes reviews from years earlier than the final study period. The project later filters these records to retain only reviews dated from 1 January 2020 to 31 December 2023.

### Columns

| Column | Description |
|---|---|
| `Unnamed: 0` | Original row/index value stored in the source CSV. |
| `review_id` | Unique identifier associated with a review. |
| `pseudo_author_id` | Pseudonymous identifier associated with the review author. |
| `author_name` | Display name of the review author. Many records use a generic value such as `A Google user`. |
| `review_text` | Written text submitted by the customer. This is the main text used for language detection and sentiment analysis. |
| `review_rating` | Numerical rating assigned to the application by the reviewer. |
| `review_likes` | Number of likes or helpful reactions associated with the review. |
| `author_app_version` | Uber application version associated with the reviewer where available. |
| `review_timestamp` | Date and time when the review was submitted. |

### Use in the Project

The raw review dataset is processed in the notebook using the following main steps:

1. Load the CSV using pandas.
2. Check the number of records.
3. Filter `review_timestamp` to the 2020-2023 study period.
4. Save the result to `data/cleaned/uber_reviews_2020_2023.csv`.
5. Detect review language using `langdetect`.
6. Keep reviews detected as English.
7. Convert timestamps to pandas datetime values.
8. Sort the remaining reviews by rating and timestamp.
9. Save the cleaned result as `data/cleaned/sorted_dataset.csv`.

No changes should be made directly to `UBER_REVIEWS.csv` when reproducing the project. Cleaning should be performed through code so the original input remains available.

## Uber_stock_prices_yahoo.csv

### Description

`Uber_stock_prices_yahoo.csv` contains historical Uber stock-price observations used by the project.

The stored CSV contains 1,006 trading-day records covering:

```text
Start date: 2020-01-02
End date:   2023-12-29
Ticker:     UBER
```

The stock dataset contains trading days rather than every calendar day because financial markets are closed on weekends and certain public holidays.

### Columns

The stored file contains the following market fields:

| Column | Description |
|---|---|
| `Date` | Trading date. |
| `Open` | Stock price at the beginning of the trading session. |
| `High` | Highest recorded stock price during the trading session. |
| `Low` | Lowest recorded stock price during the trading session. |
| `Close` | Stock price at the end of the trading session. |
| `Volume` | Number of shares traded during the session. |

The CSV created by different versions of `yfinance` can have slightly different header structures. In the version currently stored in this repository, the first rows contain additional Yahoo Finance ticker/header information. One notebook cell therefore reads the file with:

```python
pd.read_csv(file_path, skiprows=[1, 2])
```

and renames the `Price` heading to `Date` when required.

### Collection in the Notebook

The notebook also contains code that can download the stock data directly using `yfinance`:

```python
import yfinance as yf

uber_stock_data = yf.download(
    "UBER",
    start="2020-01-01",
    end="2023-12-31",
    auto_adjust=False,
    progress=False
)
```

Because `yfinance` uses an exclusive end-date convention in many download operations, the exact final observation returned can depend on the requested end date and library behaviour. The checked CSV stored in this repository currently runs through 29 December 2023.

## Data Integrity

The files in this directory are considered project inputs. To maintain reproducibility:

- Do not manually edit values in the raw CSV files.
- Perform filtering and transformations in the notebook or another documented script.
- Save processed outputs in `data/cleaned/` rather than overwriting the raw files.
- Record any future replacement or updated source data in the project documentation.

## Git LFS

`UBER_REVIEWS.csv` is managed using Git Large File Storage because the complete file is approximately 281 MB.

After cloning the repository, run:

```bash
git lfs install
git lfs pull
```

If Git LFS has not downloaded the data correctly, `UBER_REVIEWS.csv` may contain only text similar to:

```text
version https://git-lfs.github.com/spec/v1
oid sha256:...
size ...
```

That text is a Git LFS pointer and is not the actual review dataset.

`Uber_stock_prices_yahoo.csv` is small enough to be stored normally in the repository.

## Relationship to the Cleaned Data

The data flow is:

```text
UBER_REVIEWS.csv
        |
        | filter to 2020-2023
        v
uber_reviews_2020_2023.csv
        |
        | keep English reviews
        | convert timestamps
        | sort by rating and timestamp
        v
sorted_dataset.csv
```

The two processed review files are stored in `data/cleaned/` and are described in `data/cleaned/README.md`.
