## Intro
This report analyze 2 sources of Bias for code smell detection following Acrelli el al's setting in their essay. I use the `feature-envy.arff` dataset and train multiple models to argue there are potential bias in their report from both Metric distribution and correlations perspective.
## 1. Bias from Metrics Distribution - Feature Skewness
### *1.1 Missing Values* *& Class Imbalance handling*

![[Screenshot 2025-09-26 at 4.18.47 PM.png|300]]![[Screenshot 2025-09-26 at 4.30.51 PM.png |300]]

Firstly, I did some quick data exploration upon the features, the Dataset contains 81 columns of features and 1 columns target named `is_feature_envy`. As the graph shown there are 4 features has missing values which are `NMO` , `NIM`, `NOC` , and `WOC`. This lead to In the later dataset preprocess steps, I use simple inputer with median to fill all missing value after train-test split (to prevent data leakage). 

As the bar graph Distribution of is_feature_envy shown above on the right, the dataset is imbalanced in target. there are 83.3 % target is not feature envy, and only 16.7% data is feature envy. This imbalanced dataset is a faithful representation of reality since in real life there is less often to have code smell. `Macro F1` give  equal weight to the minority (Feature Envy) and majority (Non-Feature-Envy) classes regardless of their frequencies. In addition to Macro F1, I use `stratified sampling` technique when doing train-test set split to insure the ratio of two class is consistent in train set and test set.

### *1.2 Features Skewness Identification*
![[Pasted image 20250926170517.png]]The graph above shown the top 4 Skewed features distribution in all 81 features (skew = 15.93), AMW_type (14.50), number_package_visibility_methods (14.20), and AMWNAMM_type (13.90). We can see clearly that all four distributions are heavily concentrated near zero with **l**ong positive tails extending to large values.

This indicated that Feature Skewness is a potential bias in the dataset that may introduced to models and make model underperformed. Specifically, Naive Bayes assume the continuous features follow normal distribution for each class so highly skewess distribution may cause NB to performance worse. Besides, boundary margin models like SVM make have a hard time to find a margin to separate the two target class since most data are concentrated near-zero value.

### *1.3 Experiment Feature Skewness Bias on three models* 

To proof the Feature Skewness is truly a bias from dataset and making models underperformed, I set up the experiment to comparing models result with or without Skewness handling on the same Dataset `feature-envy.arff` and keeps models hyper parameters the same.
#### 1.3.1 Hypothesis
Margin/distance models (Logistic Regression, SVM-RBF) and Naive Bayes will perform worse (lower Macro f1 score) on skewed features and improve after making features more Gaussian-like via `PowerTransformer`.
#### 1.3.2 Experiment Setting
- Dataset: `feature-envy.arff` used with 81 feature, target `is_feature_envy`.
- Data Split: Train set (80%), Test set (20%) , Stratify method.
- Data Preprocessing Pipeline (**controlled Variable**: Skewness handling):
	- Baseline groups  Pipeline: `median imputation` -> `Standard Scaler`
	- Skew handled groups pipeline: `median imputation` -> `PowerTransformer(yeo-johnson)` -> `Standard Scaler`
- Model Setting: 
	- Logistic Regression: `class_weight='balanced'`, `solver='liblinear'`
	- SVM:  `RBF kernel`, ` class_weight='balanced'`,
	- Naive Bayes
#### 1.3.3 Result of Macro-F1 improvement (baseline -> Skew handled)
| Model               | Macro-F1    | ΔF1   | Recall (class 1) | ΔRecall | Accuracy    | ΔAcc  |
| ------------------- | ----------- | ----- | ---------------- | ------- | ----------- | ----- |
| Logistic Regression | 0.67 → 0.72 | +0.05 | 0.64 → 0.82      | +0.18   | 0.77 → 0.79 | +0.02 |
| SVM (RBF)           | 0.65 → 0.69 | +0.04 | 0.64 → 0.79      | +0.15   | 0.76 → 0.77 | +0.01 |
| Naïve Bayes         | 0.64 → 0.66 | +0.02 | 0.50 → 0.57      | +0.07   | 0.77 → 0.77 | +0.00 |
 In the Table above, it is clear that both LR and SVM show consistent Macro-F1 gains and large minority-class recall jumps after skew handling which is a direct evidence of skew-induced modeling bias. Naïve Bayes also improves on Macro-F1 score, which is benefit from coherent with its Gaussian assumption. 
#### 1.3.4 Analysis of PowerTransformer Impact 
![[Pasted image 20250926195250.png]]The graph above shows whats the distribution of the feature `RFC-type` on the left before skewness handling (`PowerTransformer`) and after (on the right). It can clearly see than original data distribution is extremely right-skewed and its break the Naive Bayes assumption of normal distribution. As the opposite, after applied skew handling, the feature now is much more like a normal curve (bell-curve).This is exactly why Naive Bayes is benefit from comparing to its baseline model in better minority class prediction, and better Macro F1 score. 

Naive Bayes's Accuracy remains the same as the its baseline model, I believe this is because both Naive Bayes models used all 81 features and NB is assuming feature independent, so the independence assumption is violated. High correlated feature is  “double-counted,” which can slightly reduce majority-class recall even as minority recall improves. The gain on the small class is offset by a small loss on the large class, leaving accuracy flat, while **Macro-F1** (which weights classes equally) correctly reflects the improvement.

---

![[Pasted image 20250926200718.png]]
The graph above shows the class-conditional distribution of the feature `FDP_method` before (left) and after (right) skewness handling with Yeo–Johnson (`PowerTransformer`). The blue curve is all the feature that is false class which is the majority class, the orange curve is the feature that the target is true in minority class. We can clearly see that after the Transformer applied, the large amount of blue curve near -1 is completely separated from the orange curve. This clearly shows why SVM and logistic Regression has Macro F1 score improvement. PowerTransformer helped reducing skewness and reducing overlap area around the center which may confusing models

#### 1.3.5 Skewness Experiment Summery
The result align with the hypothesis. It clearly shows by eliminating Skewness in the dataset, all three models has gain Macro-f1 Score improvement. This shows that Skewness is truly an bias in the original dataset that need to be tackled.


## 2.Bias from Correlation - High Correlated Feature Pair
Another perspective of bias in the dataset is there area many high correlated Feature pairs. As mentions above, it will have the "double counted" effect if 2 feature are high correlated with each other when feed in to the model. So i make another experiment comparing wether high correlated pair will affect model performance.
## *2.1 High Correlated Feature Pair Identification*
![[Pasted image 20250927001021.png]]
This is the complete Correlation Matrix we can see there are many Red boxes in the graph which shows the 2 feature are highly correlated to each other. Many of them are almost has 1.00 value which indicate very strong positive relationship. If not dropping 1 feature in each pair, both of the feature will be learnt by model causing model and neglect other feature that may actually help.

I set the threshold of highly correlated pair as `absolute Correlation value > 0.5` and found `246 pairs` in all 81 features. ![[Pasted image 20250927002544.png]]
As above graph shown, the 2 most highly correlated pair has absolute corr value of 1. This implies the paired features are either duplicates or strict linear transforms of one another. Such redundancy creates modeling bias since model can effectively double-count the same signal. Perfectly redundant features cause systematic favoritism toward that latent factor.

## *2.2 Experiment High Correlated Pair Bias*

#### 2.2.1 Hypothesis
High Correlated pair will introducing bias to model causing model underperformed.
#### 2.2.2 Experiment setting
 Dataset: `feature-envy.arff` , target `is_feature_envy`.
- **Controlled Variable: Feature Selection**
	- Baseline Group: No Feature Selection, all 81 feature feed into the group. 
	- Correlation-Reduced Group: Select `top 10 Mutual Information` score feature, then among those 10 features, for each pair with `absolute correlation value greater than 0.5`, drop the lower MI score Feature. `Final subset: 3 features`
- Data Split: Train set (80%), Test set (20%) , Stratify method.
- **Models Setting**: 
	- Logistic Regression (balanced, `liblinear`), 
	- SVM (RBF, balanced), 
	- Naive Bayes.
#### 2.2.3 Results (Baseline -> MI+ corr pair drop)

| Model               | Macro-F1    | ΔF1   | Recall (class 1) | ΔRecall | Accuracy    | ΔAcc  |
| ------------------- | ----------- | ----- | ---------------- | ------- | ----------- | ----- |
| Logistic Regression | 0.67 → 0.73 | +0.06 | 0.64 → 0.79      | +0.15   | 0.77 → 0.81 | +0.04 |
| SVM (RBF)           | 0.65 → 0.73 | +0.08 | 0.64 → 0.93      | +0.29   | 0.76 → 0.80 | +0.04 |
| Naïve Bayes         | 0.64 → 0.70 | +0.06 | 0.50 → 0.61      | +0.11   | 0.77 → 0.82 | +0.05 |
The results in Table 2.2.3 demonstrate that combining Mutual Information feature selection with a correlation check (threshold 0.5) yields consistent improvements across all three models.

Logistic Regression gains +0.06 Macro-F1 and +0.15 minority recall, showing that the removal of redundant features rebalanced coefficients toward more discriminative signals for the rare “smell” class. Accuracy also rose modestly from 0.77 → 0.81.
 
SVM (RBF) benefits the most, with Macro-F1 rising from 0.65 → 0.73 and minority recall nearly doubling (0.64 → 0.93). This suggests that correlated features in the baseline space had been distorting the margin; once reduced to orthogonal, high-MI metrics, the decision boundary became much more effective at capturing minority cases.

Naïve Bayes also improves, Macro-F1 climbs from 0.64 → 0.70 and recall increases by +0.11. Aggressively pruning correlated features reduced some of the bias introduced by redundant signals, allowing it to better approximate its underlying distributional assumptions.

#### 2.2.4 Analysis of Correlation Bias Impact 
For Logistic Regression, the results show that applying MI reduction and correlation filtering led to clear improvements over the baseline. Macro-F1 and recall for class 1 both increased. By eliminating these correlated pairs, Logistic Regression placed its separating hyperplane more effectively, capturing more positive cases without reducing overall accuracy

For Naïve Bayes, correlation filtering also improved results compared to the baseline. Both Macro-F1 and recall for class 1 increased, showing that removing redundant variables mitigated the double-counting problem caused by correlated features.

For SVM with RBF kernel, the correlation-reduced model outperformed its baseline by achieving higher Macro-F1 and notably stronger recall for class 1. By filtering out correlated features, the surface became less distorted, allowing the SVM to capture more true positives and raise recall for class 1 while keeping accuracy stable.

Since the reduced dataset has only 3 feature, I draw this  3D visualization (see below) to see the actual decision plane of the svm. (also see live 3d visualization on Colab notebook)
![[newplot (3).png|550]]

#### 2.2.5 Correlation Experiment Summery

The result proof the hypothesis. The experiments show that highly correlated features bias the models by double-counting signals, reducing sensitivity to minority cases. After applying MI reduction with correlation filtering, all three models improved over their baselines. Logistic Regression and Naïve Bayes gained in Macro-F1 and recall, while SVM showed the strongest improvement.

## 3. Conclusion
![[Pasted image 20250927221911.png|600]]
Our experiments show that dataset biases from skewed distributions and correlated features significantly distort model behavior. Skewness bias made Naïve Bayes violate its Gaussian assumption and compressed decision margins for SVM and Logistic Regression; after PowerTransformer, all models gained Macro-F1 and recall, especially on the minority “feature envy” class. Correlation bias led models to double-count redundant signals, reducing sensitivity to class 1. After MI + correlation filtering, all models improved further, with SVM showing the strongest recall jump (0.64 → 0.93) and a much cleaner decision surface