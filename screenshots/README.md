# 📸 Screenshots

All screenshots were captured directly from Orange 3.39 during project execution.

| File | Description |
|---|---|
| `workflow.png` | Complete Orange 3.39 canvas showing all nodes and connections |
| `wordcloud_fake.png` | Word cloud — top 50 tokens from 999 fake news articles |
| `wordcloud_true.png` | Word cloud — top 50 tokens from 999 true news articles |
| `topics_distribution.png` | LDA topic probability sums grouped by class (Fake vs True) |
| `sentiment_distribution.png` | VADER sentiment sums (positive, negative, neutral) by class |
| `predictive_results.png` | Test and Score widget — 10-fold cross-validation across 5 classifiers |

---

## How Screenshots Were Captured

All outputs were generated from the Orange 3.39 workflow using the following configurations:

- **Word Clouds:** Top 50 tokens, Matching Data output from Select Rows (label = Fake / True)
- **Topic Distribution:** Group by (label) → Transpose → Data Table (topic probability sums)
- **Sentiment Distribution:** VADER Sentiment → Group by (label) → Transpose → Data Table
- **Predictive Results:** Test and Score with 10-fold stratified cross-validation, showing average over classes
