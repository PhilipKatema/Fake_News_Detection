# 📁 Data

## Dataset: True and Fake News Sample

This project uses a balanced binary-labeled corpus of 1,998 political news articles sourced from Kaggle.

### Download

The full dataset is publicly available at:
**https://www.kaggle.com/datasets/clmentbisaillon/fake-and-real-news-dataset**

Download both files:

* `True.csv` — 21,417 true news articles (Reuters)
* `Fake.csv` — 23,481 fake news articles

This project uses a 999-article sample from each file (1,998 total).

\---

## Preparation Steps

The two CSV files must be combined before importing into Orange. Follow these steps:

1. Open `True.csv` in Excel or any spreadsheet tool
2. Add a new column named `label`
3. Set all rows in this column to the value `True`
4. Repeat for `Fake.csv`, setting all rows to `Fake`
5. Copy all data rows from `Fake.csv` and paste below the True rows (one header row only)
6. Save the merged file as `fake\\\_news\\\_combined.csv`

The final file should have **1,998 rows** and **5 columns**.

\---

## Schema

|Column|Type|Description|
|-|-|-|
|`title`|String|Headline of the news article|
|`text`|String|Full body text — primary feature for all analysis|
|`subject`|String|Topic category (True = `politicsNews`, Fake = `News`)|
|`date`|String|Publication date (mostly Dec 2017 – Mar 2018)|
|`label`|Categorical|**Class variable** — `True` or `Fake` (added during preparation)|

\---

## Sample Data

The file `sample\\\_combined.csv` in this folder contains 20 rows (10 True, 10 Fake) showing the exact format required. Use it to verify your Orange import configuration before loading the full dataset.

\---

## Coverage

* **Time period:** 2016–2018 U.S. election cycle
* **True news source:** Reuters (institutional political journalism)
* **Fake news source:** Sites flagged by PolitiFact and similar fact-checking organizations
* **Balance:** Perfectly balanced — 999 True, 999 Fake

\---

## Important Notes

* The `label` column must be set as the **Target (class) variable** in Orange's CSV File Import widget
* All other columns should be set as **Feature (String)**
* The `text` column is the only one used as text input in the Corpus widget
* Do not include a row index column — Orange will create one automatically

\---

## Citation

Bisaillon, C. (2020). *Fake and Real News Dataset*. Kaggle.
https://www.kaggle.com/datasets/clmentbisaillon/fake-and-real-news-dataset

