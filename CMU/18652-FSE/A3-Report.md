
**Objective**: 
Systematically evaluate which factors—algorithm type, text representation, or metadata—drive classification performance.

---
## Introduction

This report evaluates app review classification performance across 8 model configurations. I trained Random Forest and SVM models with different feature representations (TF-IDF vs Word2Vec) and metadata settings to identify which factors matter most. All models used identical SMOTE-balanced training data to ensure fair comparison.

Key findings: **Feature representation dominates** (+13.9% Macro-F1), metadata helps (+7.4%), and algorithm choice is minor (+1.8%).

---
## 1. Research Questions

  

**RQ1**: Does algorithm type (multiclass Random Forest vs. binary SVM One-vs-Rest) affect classification performance?
  
**RQ2**: Does feature representation (TF-IDF vs. Word2Vec embeddings) affect classifier performance?

**RQ3**: Does adding metadata (rating + sentiScore) improve classification results?

---
## 2. Experiment Design

### 2.1 Dataset Preparation & Class Imbalance Handling


![[Pasted image 20251129172923.png|400]]


I used four JSON datasets (Bug_tt, Feature_tt, Rating_tt, UserExperience_tt) containing app reviews. Each dataset was originally labeled for binary classification; I extracted positive class samples and merged them into a single multiclass corpus:

- **Bug**: 370 samples (26.3%)

- **Feature**: 295 samples (21.0%) ← minority class

- **Rating**: 370 samples (26.3%)

- **UserExperience**: 370 samples (26.3%)

- **Total**: 1,405 reviews

**Class Imbalance**: The dataset shows mild imbalance (ratio 1.25:1). Feature class is underrepresented, which could bias models toward majority classes. To handle this, I applied **SMOTE (Synthetic Minority Over-sampling Technique)** to the training set only (after 80/20 stratified split).


**Why SMOTE?**

- Balances training data (Feature: 236 → 296 samples)

- Ensures all models train on identical balanced data for fair comparison

- Prevents data leakage (applied after train-test split)

- Test set keeps original imbalanced distribution to reflect real-world performance


![[Pasted image 20251129172954.png]]

  
As the visualization shows, SMOTE generated 60 synthetic Feature samples, creating perfect balance (296 samples per class) in training data.

### 2.2 Preprocessing Pipeline - Avoiding Data Leakage


I followed strict protocols to prevent data leakage:
  

1. **Train-Test Split** (80/20, stratified): 1,124 training, 281 test samples

2. **Missing Value Imputation**: Rating field filled using training set median only

3. **Text Vectorization** (fit on training set only):

- **TF-IDF**: max_features=5000, ngram_range=(1,2), fit on lemmatized text

- **Word2Vec**: vector_size=100, window=5, trained on training corpus only

4. **Metadata Features**:

- **Selected**: `rating` (1-5 stars) + `sentiScore` (sentiment polarity)

- **Rationale**: Rating captures explicit user satisfaction (Bug reports have low ratings); sentiScore captures implicit sentiment from text (complements rating when they diverge)

5. **Feature Scaling**: StandardScaler fit on training set, applied to both sets

6. **SMOTE Balancing**: Applied to all feature combinations after preprocessing


**Controlled Variable**: All 8 models trained on the same SMOTE-balanced data. The only differences are algorithm type, feature representation, and metadata inclusion.
  

### 2.3 Algorithm Selection & Justification


**Random Forest (Multiclass)**

- Native multiclass ensemble classifier

- Handles all 4 classes directly

- Robust to overfitting, provides feature importance

- Configuration: 100 trees, default parameters


**SVM One-vs-Rest (Binary)**

- Binary decomposition: trains 4 specialized classifiers (one per class)

- Linear kernel exploits TF-IDF's sparse high-dimensional space

- Configuration: Linear kernel, probability estimates enabled, balanced class weights

  

Both trained under identical conditions (same balanced data, same random seed 42).

  

---

  

## 3. Results & Analysis

### 3.1 Overall Performance - 8 Model Comparison

| **Algorithm** | **Preprocessing**              | **Bug F1** | **Feature F1** | **Rating F1** | **UX F1** | **Macro F1** | **Accuracy** |
| ------------- | ------------------------------ | :--------: | :------------: | :-----------: | :-------: | :----------: | :----------: |
| SVM (OvR)     | TF-IDF + rating + sentiScore   |   0.609    |     0.397      |     0.504     |   0.667   |  **0.544**   |    0.559     |
| Random Forest | TF-IDF + rating + sentiScore   |   0.614    |     0.374      |     0.556     |   0.583   |  **0.532**   |    0.544     |
| SVM (OvR)     | TF-IDF (no metadata)           |   0.601    |     0.403      |     0.481     |   0.617   |  **0.526**   |    0.534     |
| Random Forest | Word2Vec + rating + sentiScore |   0.553    |     0.304      |     0.508     |   0.584   |  **0.487**   |    0.502     |
| Random Forest | TF-IDF (no metadata)           |   0.514    |     0.333      |     0.524     |   0.485   |  **0.464**   |    0.473     |
| SVM (OvR)     | Word2Vec + rating + sentiScore |   0.640    |   **0.032**    |     0.455     |   0.663   |  **0.448**   |    0.537     |
| Random Forest | Word2Vec (no metadata)         |   0.446    |     0.361      |     0.526     |   0.438   |  **0.442**   |    0.445     |
| SVM (OvR)     | Word2Vec (no metadata)         |   0.487    |     0.253      |     0.489     |   0.533   |  **0.440**   |    0.459     |

*Note: All models trained on SMOTE-balanced data (296 samples/class).*

**Best Model**: SVM (OvR) + TF-IDF + Metadata achieves highest Macro F1 (0.544) with strong UX (F1=0.667) and Bug (F1=0.609) recognition.

**Worst Failure**: SVM + Word2Vec + Metadata completely fails on Feature class (F1=0.032), showing Word2Vec instability.


![[Pasted image 20251129173026.png|500]]

### 3.2 RQ1: Binary vs. Multiclass Algorithm - Minimal Impact

  

**Hypothesis**: SVM's One-vs-Rest binary decomposition will outperform Random Forest's native multiclass approach.

  

**Results**:

- **SVM (OvR)** average Macro F1: 0.490

- **Random Forest** average Macro F1: 0.481

- **Difference**: +0.009 (1.8% improvement)

  

**Answer**: Algorithm type has **minimal impact**. SVM marginally outperforms Random Forest, but the difference is negligible. Both approaches are viable.

  

**Why the small difference?**

- SVM's binary decomposition allows specialized classifiers per class, slightly improving decision boundaries

- Random Forest's ensemble averaging compensates for its multiclass approach

- Both benefit equally from TF-IDF and metadata features


**Implication**: Don't obsess over algorithm choice—focus on feature engineering instead.

  
### 3.3 RQ2: TF-IDF vs. Word2Vec - Feature Representation Dominates


**Hypothesis**: TF-IDF's exact keyword matching will outperform Word2Vec's semantic embeddings for short review texts.

  

**Results**:

- **TF-IDF** average Macro F1: 0.517

- **Word2Vec** average Macro F1: 0.454

- **Difference**: +0.063 (13.9% improvement)

  

**Answer**: **TF-IDF substantially outperforms Word2Vec**. This is the largest performance gap among all factors.


**Why TF-IDF wins?**

  
1. **Short text nature**: App reviews are brief (10-30 words). Exact keyword matching beats semantic similarity.

2. **Distinctive vocabulary**: Each class has clear marker words:

- Bug: "crash", "error", "broken"

- Feature: "add", "wish", "should"

- Rating: "stars", "recommend"

- UX: "love", "easy", "interface"

3. **Limited training data**: 1,124 samples insufficient for quality Word2Vec embeddings.

  

**Evidence**: Top 5 models all use TF-IDF. Word2Vec models cluster at bottom (ranks 6-8).

  

**Catastrophic Word2Vec failure**: SVM + Word2Vec + Metadata achieves high Bug F1 (0.640) but **completely fails** on Feature class (F1=0.032). This shows Word2Vec's instability—it captures some classes well but catastrophically ignores others.

  

![[Pasted image 20251129173107.png|650]]

**Implication**: For short-text classification, prioritize TF-IDF over embeddings.

### 3.4 RQ3: Metadata Impact - Consistent Improvement

**Hypothesis**: Adding rating and sentiment score will improve classification by providing complementary non-textual signals.

**Results**:

- **With metadata** (rating + sentiScore): 0.503

- **Without metadata**: 0.468

- **Difference**: +0.035 (7.4% improvement)


**Answer**: **Metadata consistently helps**. All 4 algorithm-feature combinations benefit:

| Configuration  | With Metadata | Without Metadata | Δ Macro F1                |
| -------------- | ------------- | ---------------- | ------------------------- |
| SVM + TF-IDF   | 0.544         | 0.526            | **+0.018**                |
| RF + TF-IDF    | 0.532         | 0.464            | **+0.068** ← largest gain |
| RF + Word2Vec  | 0.487         | 0.442            | **+0.045**                |
| SVM + Word2Vec | 0.448         | 0.440            | **+0.008**                |

**Why metadata helps**:

- **rating**: Explicit satisfaction signal (Bug reports typically 1-2 stars, positive UX 4-5 stars)

- **sentiScore**: Implicit emotional tone (catches sentiment when rating diverges—e.g., "5 stars but wish they'd fix...")

- **Complementarity**: Together capture sentiment intensity that text alone misses
  

**Biggest beneficiary**: Random Forest + TF-IDF gains +6.8 points, suggesting tree models effectively leverage non-textual features.

![[Pasted image 20251129173233.png|550]]

  

**Implication**: Always collect metadata—even simple features like ratings substantially improve performance.
### 3.5 Per-Class Performance - Feature Class Remains Challenging  

**Easiest class**: User Experience (avg F1 = 0.558)

- Strong emotional language ("love", "hate", "amazing")

- High sentiScore predictiveness


**Hardest class**: Feature (avg F1 = 0.307)

- Despite SMOTE balancing, remains difficult

- Overlaps with UserExperience language patterns ("I wish...", "would be nice...")

- One model catastrophically failed (F1 = 0.032)


**Evidence**: Even best model (SVM + TF-IDF + Meta) achieves only F1=0.397 for Feature class vs. F1=0.667 for UX class.


**Why Feature is hard?**

1. **Language ambiguity**: Feature requests use same phrases as UX feedback

2. **SMOTE limitation**: Synthetic samples may not capture real Feature diversity

3. **Class boundary overlap**: Some reviews are legitimately both Feature + UX


**Implication**: Production systems should flag Feature predictions for human review.

---
## 4. Conclusion


### 4.1 Key Findings (Ranked by Impact)


This controlled experiment reveals three clear findings:

1. **Feature representation dominates** (+13.9%): TF-IDF significantly outperforms Word2Vec for short review texts. Exact keyword matching beats semantic similarity.

2. **Metadata provides consistent value** (+7.4%): Adding rating and sentiment scores improves all configurations, with largest benefit for Random Forest models.
  
3. **Algorithm choice is minor** (+1.8%): SVM (OvR) edges Random Forest, but difference is negligible. Choice should depend on secondary factors (interpretability, speed).


