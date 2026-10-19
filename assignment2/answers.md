# Q1

## What do class 0 and class 1 mean?

- **0** = no diabetes
- **1** = prediabetes or diabetes

Class 1 combines prediabetes and diabetes, so the model cannot tell them apart.

The target is **imbalanced**: only about 14% are class 1. A model that always predicts "no diabetes" would already be about 86% accurate, so accuracy alone is not enough to judge the model. We also need precision and recall.

# Q2

- `BMI` is a real amount, so a higher number means a higher BMI.
- `HighBP` is a yes/no answer, so 0 and 1 are two categories, not amounts.
- `GenHlth` has an order (higher = worse health), but the steps between codes are not measured distances. A 4 is not "twice as bad" as a 2.

So looking only at the numbers in the CSV is not enough. The same kind of number can be a measurement, a category or an ordered group.

# Q3

**1. Is self-rated health related to diabetes?**
- *Evidence:* the `GenHlth` plot. The diabetes share goes from 2.5% (excellent) to 38% (poor).
- *Conclusion:* `GenHlth` has the clearest relationship with the target and looks useful for prediction.
- *We cannot conclude* that poor health causes diabetes. Having diabetes could also make people rate their health as worse.

**2. Are other health conditions related to diabetes?**
- *Evidence:* the binary plot. With high blood pressure the share is 24%, vs 6% without. Heart disease, stroke and walking difficulty show similar gaps.
- *Conclusion:* these conditions often appear together with diabetes in this data.
- *We cannot conclude* which one comes first, or that one causes another.

**3. Does BMI differ between the classes?**
- *Evidence:* the BMI histogram and the table below. The share with diabetes rises from about 6% for BMI under 25 to 34% for BMI over 40.
- *Conclusion:* higher BMI is associated with diabetes.

**4. Does the diabetes share change with age?**
- *Evidence:* the `Age` plot. It rises from about 1% in the youngest group to about 22% at ages 70-74.
- *Conclusion:* age is strongly associated with diabetes.

**5. Which features show little relationship, or a surprising one?**
- *Evidence:* `Sex`, `AnyHealthcare`, `NoDocbcCost` and `Smoker` differ by only 2-4 percentage points. Heavy drinkers have a *lower* diabetes share (6% vs 14%).
- *Conclusion:* these features probably add less to the model.
- *We cannot conclude* that alcohol protects against diabetes. Other differences between the groups, such as age, could explain the pattern.

# Q4

The purpose of the test set is to let us see how the model performs on data it has never seen. If we preprocess the data before splitting, then the test rows have influenced
what kinds of transformations we use, what features to select, and so on. The data has now been "seen" in a subtle way. This is called data leakage. It causes the model to
perform better during testing than it would in production.

# Q5

False positives in this case mean that someone who is not diabetic is marked as being diabetic. A false negative means that a person who is diabetic is marked as not diabetic. For this type of disease classification where it can be verified by a more accurate test later, false positives are generally preferred to false negatives.

Precision is 0.516 with our model, meaning the model was correct in ~52% of positive classifications. In other words, the rate of people who were predicted to have diabetes who are false alarms is 48%.

The recall value with our model using the default threshold is 0.158, meaning that the model missed ~84% of the actual diabetes cases.

The main cause of the problem is the class imbalance combined with the default threshold of 0.5. Only 14% of people have diabetes, so the model rarely gives a probability of more than 0.5. That is also why the accuracy is deceptively high (even if the model predicted negative in 100% of cases, the accuracy would still be 86%).

# Q6

Lowering the threshold to 0.40 flags more people. Recall goes up (0.269) and precision goes down (0.472). In a health setting, missing someone with diabetes (an FN) is usually worse than a false alarm (an FP), so recall is often the more important metric here.

The model, its predicted probabilities, and TEST all stay the same. Only the cut-off that turns a probability into Class O or 1 changes.

False positives went up (1,045 to 5 2,128) and false negatives went down (5,953 to 5,170).

# Q7

## What we learned from the EDA

- The target is imbalanced: only 13.9% of the people are class 1, so accuracy alone is misleading.
- `GenHlth`, `Age`, `BMI`, `HighBP`, `HighChol` and `DiffWalk` show the clearest differences between the classes. For example, the diabetes share rises from 2.5% (excellent health) to 38% (poor health), and from about 6% (BMI below 25) to 34% (BMI above 40).
- `Sex`, `AnyHealthcare`, `NoDocbcCost` and `Smoker` show only small differences.
- Some results are surprising (heavy alcohol use and `CholCheck`) and probably reflect other differences between the groups, not a protective effect.

## What we learned from Logistic Regression

- The accuracy is 0.862, which is almost the same as the 0.861 we would get by always predicting class 0. So the model is not much better than this trivial baseline on accuracy.
- With threshold 0.50 it finds only 16% of the real class 1 (recall 0.158), although about half of its alarms are correct (precision 0.516).
- Lowering the threshold to 0.40 raises recall to 0.269 and lowers precision to 0.472, so it is a trade-off between false negatives and false positives. It is not a free improvement.
- The accuracy hides the weak detection of class 1, so we need the confusion matrix, precision and recall.

## Limitations

1. **Association, not causation.** The EDA and the model show which features go together with diabetes in this data. They do not show that a feature causes diabetes, and we cannot say what would happen if someone changed, for example, their BMI.
2. **Survey data.** The answers are self-reported (for example `GenHlth` and `PhysActivity`) and may be inaccurate. People with diabetes may also answer differently because of the diagnosis, so some features may partly be a consequence of diabetes.
3. **Combined target.** Class 1 mixes prediabetes and diabetes, so the model cannot tell them apart.
4. **Simple model and simple representation.** Logistic Regression is a linear model, and we use the ordered codes as numbers with equal steps. The imbalance is not handled (no class weights), which is one reason for the low recall.
5. **One split and a fixed threshold choice.** We evaluated on a single TEST set and compared only two thresholds, so we do not know how stable the results are. We also did not choose the threshold with the medical costs of the errors in mind.
6. **Duplicate rows were kept**, as the assignment says, so we cannot be sure whether some identical rows are the same person.

# Threshold comparison table

| Threshold | Accuracy | Precision | Recall | F1    | False positives | False negatives | True positives | Predicted class 1 |
|-----------|----------|-----------|--------|-------|-----------------|-----------------|----------------|-------------------|
| 0.5       | 0.862    | 0.516     | 0.158  | 0.242 | 1045            | 5953            | 1116           | 2161              |
| 0.4       | 0.856    | 0.472     | 0.269  | 0.342 | 2128            | 5170            | 1899           | 4027              |

# G1

The work was done done by all of the group members discussing the questions, and finding solutions together.

# G2

We made sure that everyone understood the complete solution by working on most things together.