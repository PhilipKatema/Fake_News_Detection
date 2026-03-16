# 📈 Results Summary

All findings are from 10-fold stratified cross-validation on labeled articles using Orange 3.39.

\---

## Classification Results

|Model|AUC|Accuracy (CA)|F1|Precision|Recall|MCC|
|-|-|-|-|-|-|-|
|**Random Forest**|**0.924**|**84.8%**|**0.848**|**0.853**|**0.848**|**0.701**|
|Logistic Regression|0.918|83.9%|0.838|0.846|0.839|0.685|
|Neural Network|0.917|84.1%|0.841|0.846|0.841|0.687|
|Naive Bayes|0.914|83.4%|0.834|0.839|0.834|0.673|
|kNN|0.838|76.7%|0.766|0.767|0.767|0.532|
|SVM *(RBF kernel — misconfigured)*|0.635|53.6%|0.434|0.719|0.536|0.216|

**Evaluation:** 10-fold stratified cross-validation | **Metric target:** Average over classes

\---

## Topic Modeling Results (LDA, 5 Topics)

|#|Topic Label|Top Keywords|Fake Sum|True Sum|Dominant Class|
|-|-|-|-|-|-|
|1|Breaking Political News \& Party Conflict|watch, breaking, trump, democrat, fbi, republican, gop|4,036|4,073|Balanced|
|2|Election Integrity \& Legislative Controversy|hillary, clinton, vote, bill, racist, law, security|3,124|3,576|True|
|**3**|**Social Media \& Viral Political Content**|**video, trump, white, liberal, donald, supporter**|**6,933**|**3,594**|**Fake (1.93×)**|
|4|Anti-Establishment \& Identity Politics|obama, trump, president, muslim, anti, police, america|5,316|4,887|Fake|
|5|Media Criticism \& Cultural Commentary|cnn, medium, black, woman, war, say|4,040|5,287|True|

**Model configuration:** LDA · 5 topics · 100 iterations · 10 keywords per topic

\---

## Sentiment Analysis Results (VADER)

|Sentiment|Fake (999 articles)|True (999 articles)|Difference|
|-|-|-|-|
|Positive|1,993.77|1,702.52|+17.1%|
|**Negative**|**3,671.40**|**2,531.47**|**+45.0%**|
|Neutral|17,784.80|17,183.00|+3.5%|

**Key insight:** Fake news shows emotional amplification in both directions — not merely negativity bias. Higher positive AND negative scores indicate language engineered for emotional engagement.

\---

## Word Cloud — Top Distinctive Terms

|Fake News|True News|
|-|-|
|hillary|house|
|video|russia|
|trump|trump|
|obama|north|
|watch|say|
|breaking|republican|
|muslim|korea|
|president|election|
|clinton|senate|
|american|bill|

**Pattern:** Fake news → personality-driven, action/urgency words, identity-coded terms
**Pattern:** True news → institutional vocabulary, geopolitical entities, legislative language

\---

## Interpretation

The four analyses converge on a consistent picture of election-era fake news:

1. **Vocabulary:** Fake news targets individuals and emotions; true news targets institutions and events
2. **Topics:** Fake news is disproportionately anchored in social media amplification mechanics (Topic 3, nearly 2× skew)
3. **Sentiment:** Fake news deploys emotional amplification in both directions as an engagement strategy
4. **Classification:** The patterns are strong enough that even a simple Naive Bayes model achieves AUC 0.914

**Real-world implication:** An AUC of 0.924 (Random Forest) means that given any random pair of one fake and one true article, the model ranks them correctly 92.4% of the time — sufficient for deployment in content moderation prioritization queues.

\---

## Known Limitations

* Dataset covers a narrow time window (Dec 2017 – Mar 2018); generalizability to other periods is untested
* Binary True/False labels do not capture the spectrum of veracity (satire, partial truth, misleading framing)
* SVM results (AUC 0.635) reflect a configuration error (RBF kernel), not a model limitation
* Full-article VADER averaging compresses sentiment variance; sentence-level analysis would be more precise

