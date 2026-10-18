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

False positives in this case mean that someone who is not diabetic is marked as being diabetic. A false negative means that a person who is diabetic is marked as not diabetic. For disease classificatoin, false positives are generally preferred to false negatives.

Precision is 0.516 with our model, meaning the model was correct in ~52% of positive classifications. In other words, the rate of false positives is 48%.

The recall value with our model using the default threshold is 0.158, meaning that the model missed ~84% of the actual diabetes cases.

The main cause of the problem is the class imbalance combined with the default threshold of 0.5. Only 14% of people have diabetes, so the model rarely gives a probability of more than 0.5. That is also why the accuracy is deceptively high (even if the model predicted negative in 100% of cases, the accuracy would still be 86%).

# Q6

Changing the threshold from 0.5 to 0.4 noticeably improved the results.

# Q7