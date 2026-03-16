# 🔧 Workflow

## About the Orange Workflow

The complete analysis was built as a visual workflow in **Orange 3.39** using the **Text Mining add-on**. No Python scripting was used — every step is a configurable node on the Orange canvas.

\---

## Prerequisites

1. **Download Orange 3.39**
https://orangedatamining.com/download/
2. **Install the Text Mining add-on**
Orange → Options → Add-ons → Search "Text" → Install → Restart Orange
3. **Prepare the dataset**
Follow the steps in [`../data/README.md`](../data/README.md) to create `fake\\\_news\\\_combined.csv`

\---

## Workflow Overview

The canvas contains **18 nodes** organized into one shared backbone and four analytical branches:

```
BACKBONE
────────
CSV File Import → Select Columns (Set Role) → Corpus → Preprocess Text
                                                              │
          ┌───────────────────────────────────────────────────┤
          │                                                   │
BRANCH A  │  BRANCH B            BRANCH C       BRANCH D     │
Word Cloud│  Sentiment Analysis  Topic Modeling  Bag of Words │
──────────│  ─────────────────  ─────────────  ────────────  │
Select    │  Sentiment Analysis  Topic Modeling  Bag of Words  │
Rows(Fake)│  → Group by         → Edit Domain  → Test and    │
→ Word    │  → Transpose        → Group by       Score ←──── │
  Cloud   │  → Data Table       → Transpose    ↑ ↑ ↑ ↑ ↑
Select    │                     → Data Table   NB LR RF NN SVM
Rows(True)│                                    → Confusion
→ Word    │                                      Matrix
  Cloud   │
```

\---

## Node-by-Node Configuration

For detailed settings on every node — including exact values for each dropdown, checkbox, and numeric field — refer to:

📄 [**`../docs/workflow\\\_guide.docx`**](../docs/workflow_guide.docx)

This guide covers every node in the order they should be built, including:

* All Preprocess Text settings (tokenization pattern, normalization method, frequency thresholds)
* Corpus field assignments (which column is text, title, class variable)
* Bag of Words TF-IDF configuration
* Classifier hyperparameters
* Test and Score cross-validation setup
* A 13-item completion checklist
* A troubleshooting quick-reference table

\---

## Recreating the Workflow from Scratch

If you want to rebuild the workflow node by node:

1. Open Orange 3.39
2. Open a blank canvas (File → New)
3. Follow the workflow guide in `docs/workflow\\\_guide.docx` from Section 2 onward
4. Save your workflow as `FAKE\\\_NEWS\\\_DETECTION.ows` (File → Save)

### Node Panel Locations (Orange 3.39)

|Node|Panel Location|
|-|-|
|CSV File Import|Data|
|Corpus|Text Mining|
|Preprocess Text|Text Mining|
|Select Rows|Data|
|Word Cloud|Text Mining|
|Sentiment Analysis|Text Mining|
|Topic Modeling|Text Mining|
|Bag of Words|Text Mining|
|Edit Domain|Data|
|Group by|Data|
|Transpose|Data|
|Data Table|Data|
|Naive Bayes|Model|
|Logistic Regression|Model|
|Random Forest|Model|
|Neural Network|Model|
|kNN|Model|
|SVM|Model|
|Test and Score|Evaluate|
|Confusion Matrix|Evaluate|

\---

## Important Configuration Notes

> \\\*\\\*SVM Kernel:\\\*\\\* Orange defaults to RBF kernel. For text classification with TF-IDF data, always change this to \\\*\\\*Linear kernel\\\*\\\*. The RBF kernel is computationally inappropriate for sparse high-dimensional text vectors and will produce near-random results (as seen in this project — AUC 0.635).

> \\\*\\\*WordNet Lemmatizer:\\\*\\\* First use will prompt an NLTK data download. Click Download and ensure you have an internet connection. If download fails, substitute Porter Stemmer.

> \\\*\\\*Bag of Words output:\\\*\\\* Connect via the \\\*\\\*Embeddings\\\*\\\* output channel, not the Data output. Classifiers and Test and Score require Embeddings for text-based classification.

