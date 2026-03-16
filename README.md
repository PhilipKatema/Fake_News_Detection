# 🗞️ Fake News Detection — 2016–2018 U.S. Election Cycle

> A complete text mining pipeline analyzing 44,867 labeled political news articles using topic modeling, sentiment analysis, word frequency visualization, and supervised machine learning classification.
> Built entirely in \*\*Orange 3.39\*\* (no code required).

\---

## 📊 Key Results

|Model|AUC|Accuracy|F1|
|-|-|-|-|
|**Random Forest** ⭐|**0.924**|**84.8%**|**0.848**|
|Logistic Regression|0.918|83.9%|0.838|
|Neural Network|0.917|84.1%|0.841|
|Naive Bayes|0.914|83.4%|0.834|
|kNN|0.838|76.7%|0.766|
|SVM *(misconfigured — see lessons learned)*|0.635|53.6%|0.434|

**Fake news carried 45% more negative sentiment** than true news, and scored nearly **2× higher** on the "Social Media \& Viral Political Content" topic — the computational signature of election-era misinformation.

\---

## 🗂️ Repository Structure

```
fake-news-detection/
│
├── README.md                        ← You are here
│
├── data/
│   ├── README.md                    ← Dataset description \& download instructions
│   └── sample\_combined.csv          ← 20-row sample showing expected format
│
├── docs/
│   ├── business\_justification.docx  ← Problem statement, tool \& dataset rationale
│   ├── project\_report.docx          ← Full project report (all 6 required sections)
│   └── workflow\_guide.docx          ← Node-by-node Orange 3.39 configuration guide
│
├── screenshots/
│   ├── workflow.png                  ← Complete Orange canvas
│   ├── wordcloud\_fake.png            ← Top vocabulary — Fake news
│   ├── wordcloud\_true.png            ← Top vocabulary — True news
│   ├── topics\_distribution.png       ← LDA topic sums by class
│   ├── sentiment\_distribution.png    ← VADER scores by class
│   └── predictive\_results.png        ← Test and Score classifier comparison
│
├── workflow/
│   └── README.md                    ← How to open/recreate the Orange workflow
│
└── results/
    └── summary.md                   ← All findings at a glance
```

\---

## 🧠 Project Overview

The 2016 U.S. presidential election brought fake news into mainstream political consciousness. Fabricated stories spread six times faster than real news on social media, reached millions of voters, and were designed to exploit emotional triggers rather than inform. This project investigates whether text mining techniques — applied to a labeled corpus of election-era news — can identify the linguistic, topical, and emotional patterns that make misinformation detectable.

### Business Questions Addressed

* Can a machine learning model trained on labeled news data detect fake news with meaningful accuracy?
* What vocabulary, sentiment, and topical fingerprints distinguish misinformation from credible journalism?
* How can these findings be applied to social media moderation, fact-checking, and election integrity platforms?

\---

## 📁 The Data

* **Dataset:** True and Fake News Sample
* **Source:** [Kaggle — Fake and Real News Dataset](https://www.kaggle.com/datasets/clmentbisaillon/fake-and-real-news-dataset) (Clément Bisaillon)
* **Size:** 44,867 articles — 21,417 True, 23,450 Fake (perfectly balanced)
* **Period:** 2016–2018 U.S. election cycle
* **Fields:** `title`, `text`, `subject`, `date`, `label`

See [`data/README.md`](https://github.com/PhilipKatema/Fake_News_Detection/blob/main/data/README.md) for full dataset description and preparation steps.

\---

## 🔧 Tool

**Orange 3.39** with the **Text Mining add-on**

All analysis was performed using Orange's visual workflow canvas — no scripting required. The workflow is fully reproducible by following the [`docs/workflow\_guide.docx`](https://github.com/PhilipKatema/Fake_News_Detection/blob/main/docs/workflow_guide.docx).

\---

## 🔬 Methodology

### Pipeline Architecture

```
CSV Import → Corpus → Preprocess Text ──┬──▶ Word Cloud (Fake / True)
                                        ├──▶ Sentiment Analysis → Box Plot
                                        ├──▶ Topic Modeling (LDA, 5 topics)
                                        └──▶ Bag of Words (TF-IDF) → Test and Score
                                                   ↑ Naive Bayes
                                                   ↑ Logistic Regression
                                                   ↑ Random Forest
                                                   ↑ Neural Network
                                                   ↑ SVM / kNN
```

### Preprocessing

|Step|Configuration|
|-|-|
|Transformation|Lowercase, Remove URLs|
|Tokenization|Regexp: `\[a-zA-Z]+`|
|Normalization|WordNet Lemmatizer|
|Filtering|English stopwords, Document frequency: 0.001–0.80, Min length: 3|

### Vectorization

TF-IDF (sublinear TF, smooth IDF) via Orange's Bag of Words widget.

### Evaluation

10-fold stratified cross-validation. Metrics: AUC, Classification Accuracy, F1, Precision, Recall, MCC.

\---

## 📈 Findings

### 1 · Vocabulary Fingerprints

|Class|Dominant Terms|
|-|-|
|**Fake News**|hillary, video, trump, obama, watch, breaking, muslim, tweet|
|**True News**|house, russia, korea, north, senate, republican, election, law|

Fake news is **personality-driven**. True news is **institution-driven**. Both cover the same political period through fundamentally different lenses.

!\[Word Clouds](screenshots/wordcloud\_fake.png)

### 2 · Topic Modeling

5 LDA topics identified. Standout finding:

|Topic|Fake Sum|True Sum|Ratio|
|-|-|-|-|
|🔴 Social Media \& Viral Political Content|**6,933**|3,594|**1.93×**|
|Anti-Establishment \& Identity Politics|5,316|4,887|1.09×|
|Breaking Political News \& Party Conflict|4,036|4,073|\~1.0×|
|Media Criticism \& Cultural Commentary|4,040|**5,287**|0.76×|
|Election Integrity \& Legislative Controversy|3,124|3,576|0.87×|

Election-era fake news was built around packaging viral social media content as news rather than reporting primary-source events.

!\[Topics Distribution](screenshots/topics\_distribution.png)

### 3 · Sentiment Analysis (VADER)

|Score|Fake|True|Difference|
|-|-|-|-|
|Positive|1,994|1,703|+17%|
|**Negative**|**3,671**|**2,531**|**+45%**|
|Neutral|17,785|17,183|+3%|

Fake news shows emotional amplification in **both** directions — not simply negativity bias. Designed to provoke, not inform.

!\[Sentiment Distribution](screenshots/sentiment\_distribution.png)

### 4 · Predictive Classification

4 of 5 classifiers exceeded AUC 0.91, confirming that vocabulary patterns alone are sufficient for automated detection at meaningful real-world reliability.

!\[Predictive Results](screenshots/predictive\_results.png)

\---

## ⚠️ Lessons Learned

**What went well:**

* 4 of 5 classifiers exceeded AUC 0.91 with consistent performance
* Topic 3 finding was clear, interpretable, and election-contextually meaningful
* Orange's visual workflow made the pipeline fully transparent and reproducible

**What did NOT go well:**

* SVM produced near-random results (AUC 0.635) due to Orange's **default RBF kernel** — wrong for sparse TF-IDF data. A **linear kernel** is required for text classification. This is a one-setting fix.
* Noise tokens (`ha`, `wa`, `u`) survived preprocessing — a custom stopword list was needed
* Full-article VADER averaging compressed sentiment variance, making Box Plot visualizations flatter than expected

**What I'd do differently:**

* Set SVM kernel to Linear before running (single dropdown change, likely +10% AUC)
* Build a project-specific stopword list for political noise tokens
* Perform sentence-level sentiment analysis for more granular emotional profiling
* Add ROC Analysis widget for multi-classifier curve comparison
* Expand dataset to 2020 and 2022 election cycles to test temporal generalizability

\---

## 📄 Documentation

|File|Description|
|-|-|
|[`docs/project\_report.docx`](https://github.com/PhilipKatema/Fake_News_Detection/blob/main/docs/workflow_guide.docx)|Full academic report — all findings, methodology, and lessons learned|
|[`docs/workflow\_guide.docx`]([docs/workflow_guide.docx](https://github.com/PhilipKatema/Fake_News_Detection/blob/main/docs/workflow_guide.docx))|Step-by-step Orange 3.39 node configuration guide (fully replicable)|

\---

## 🚀 How to Replicate

1. Download the dataset from [Kaggle](https://www.kaggle.com/datasets/clmentbisaillon/fake-and-real-news-dataset)
2. Add a `label` column: `True` for all rows in True.csv, `Fake` for all rows in Fake.csv
3. Merge into `fake\_news\_combined.csv`
4. Install [Orange 3.39](https://orangedatamining.com/) and the Text Mining add-on
5. Follow [`docs/workflow\_guide.docx`](https://github.com/PhilipKatema/Fake_News_Detection/blob/main/docs/workflow_guide.docx) to build the workflow node by node

See [`workflow/README.md`](https://github.com/PhilipKatema/Fake_News_Detection/blob/main/workflow/README.md) for detailed setup instructions.

\---

## 📚 References

* Hutto, C.J. \& Gilbert, E. (2014). VADER: A Parsimonious Rule-based Model for Sentiment Analysis of Social Media Text. *ICWSM*.
* Bisaillon, C. (2020). Fake and Real News Dataset. *Kaggle*.

\---

*Data Mining Course Project · Spring 2026 · Data Analytics in Business Program*

