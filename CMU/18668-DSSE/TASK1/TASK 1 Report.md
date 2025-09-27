## Dataset Quick Explore 

![[Screenshot 2025-09-20 at 2.10.34 AM.png]]
The Given dataset is imbalanced, majority class is `Not Bankrupt`, minority class is `Bankrupt`.
Since It's a extremely imbalanced class, and I believe in this binary classification, determine a company is bankrupt or not should be equally important. So I will more focus on `Macro Avg F1-Score` to compare model performance in below analysis.
## Comparison models & Analysis 
---
![[Screenshot 2025-09-20 at 1.43.20 AM.png]]
### Decision Tree
#### *Model Setting*
For All the Decision Tree , we are using the default hyperparameters from scikit-learn. The `random_state` is set for reproducibility.
#### *Observation*
In the table above, the `DT(MI)`achieved the highest `Macro F1 score` which is `0.66` the `DT(Base)` has `0.63`, and `DT(PCA)` has `0.6`. While all Decision tree has a train Accuracy of 1.0 means all of them are overfitting, the DT(MI) is the more generalized one among the three due to a higher test Accuracy 0.9545. The DT(MI) also perform better on the minority class which is bankrupt companies with 0.34. This indicate that the feature selection method (MI) does helps the Decision Tree to classify more bankrupt case. While DT(PCA) has both the worst Test accuracy (0.94), worst macro Avg f1-score (0.23).
#### *Explanation*
The DT(base) is trained on all 95 features, and since it reach reach Training accuracy of 1.0 which means the DT(baseline) has classify everything in the train set correctly, but when testing , it cannot generalize well on data entries that it never seen before. This is the main reason why the test accuracy is lower than train accuracy. Besides, the 95 features almost surely have many features is noise but DT(base) also learn that since its reach 1.00 train acc. Those noise will let the tree create many wrong separation nodes.

For DT(MI), it has the best performance (highest macro f1) among all decision trees, this is because by selecting the top 10 features with the highest Mutual Information scores, we are providing the Decision Tree with a subset of features that are most relevant and informative for predicting bankruptcy. This let the DT(MI) to have a shallower tree, which means better generalizations. Comparing to Baseline model which is letting DT itself to select what feature is important to separate, which can lead to the Decision Tree falsely chosen some separate node optimal local but not optimal overall. Some features might appear to be good discriminators locally within a small subset of the data, even if they are not truly relevant to the overall classification problem or are just capturing noise.

For DT(PCA),it's the worst performance across the three. This is due to DT is favor on clear axis-aligned feature splits to separate classes. PCA reduced 95 dimensions to 10 components, which the minority class separating logic is been hidden during the reductions. For instance, if the original feature `Debt ratio`can do a good job separating bankrupts, a tree node can have a clean, explainable logic with `Debt ratio > 0.5` means bankrupt. But if we utilizing PCA, the deb ratio feature is mix with other features. This makes the DT hard to find a separable logic.

# Naive Bayes
#### Model Setting
For all NB models, i use the `GaussianNB` from scikit-learn with default setting.
#### *Observation*
The `NB(Base)` achieved a Macro F1 score  `0.25` with test accuracy of about `0.29`. The `NB(MI)` improved to a Macro F1 of `0.64` with slightly highest test accuracy `0.946`.The `NB(PCA)` performed with macro F1 `0.58` and test accuracy `0.945`. All Naive Bayes models struggled in minority (bankrupt) recall, but `NB(MI)` was able to capture more bankrupt cases compared to the others.
#### *Explanation*
Since all Navie Bayes are assuming features are independent to each other, the `NB(Base)` is the worst performance among the three makes sense. In all 95 features, there is almost no way each feature is independent to each other. So when calculations the given possibility on each class, there is high chance to introduce more error while during all the mutiplcations which finally leading to poor train/test accuracy.

`NB(MI)`has the best macro f1 score among the three and comparing to the baseline model, the MI feature selection dramatically help the performance. This is because only 10 features are fed in the model instead of 95 which has way less possibility that the 10 feature is not independent to each other.

`NB(PCA)` is way better than `NB(base)` but not as good as the NB(MI) since the PCA process is still let the noise feature introduced in the components when doing reducing dimension. This leads to less macro f1 score comparing to `NB(MI)`.


# SVM

#### _Model Setting_
For all SVM models, we are using scikit-learn’s `SVC` with the default RBF kernel. The `random_state` is fixed for reproducibility. the `class_weight = 'balanced'` to handle the class imbalance.

#### _Observation_
The `SVM(Base)` achieved a Macro F1 of `0.62` with test accuracy `0.89`. The `SVM(MI)`  has  a Macro F1 `0.59` with test accuracy `0.86`. The `SVM(PCA)` has a Macro F1 `0.60` with test accuracy `0.87`.  All SVM performance almost the same.
#### *Explanation*
SVM is goal to find an optimal separating hyperplane with maximum margin, and the RBF kernel allows it to capture nonlinear decision boundaries. This flexibility makes SVM relatively robust to noisy or redundant features, which explains why all three variants perform similarly.

The `SVM(Base) `has the best result, this is due to both MI or PCA methods make eliminate some useful features. SVM prefer higher dimensions data. For instance, if a dataset cannot use rbf to draw a clear separation line in 3 dimension, it is not possible for SVM to distinguish them in lower dimension. This explains why `SVM(Base)` achieved the best Macro F1. This dataset is easier to separate in high dimension space.