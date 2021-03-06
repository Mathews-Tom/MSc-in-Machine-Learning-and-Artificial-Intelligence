# Finding the Optimal Threshold

In the last segment, you saw that the ROC curve essentially shows you a trade-off between sensitivity and specificity. But how do you find the optimal threshold in order to get decent accuracy, sensitivity and specificity? Let's hear what Rahim has to say.

**VIDEO**

So, first Rahim calculated the values of accuracy, sensitivity and specificity at different cut-off values and stored them in a dataframe using the code provided below.

```python
# Now let's calculate accuracy sensitivity and specificity for various probability cutoffs.

cutoff_df = pd.DataFrame( columns = ['prob','accuracy','sensi','speci'])
from sklearn.metrics import confusion_matrix

# TP = confusion[1,1] # true positive

# TN = confusion[0,0] # true negatives

# FP = confusion[0,1] # false positives

# FN = confusion[1,0] # false negatives

num = [0.0,0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9]
for i in num:
    cm1 = metrics.confusion_matrix(y_train_pred_final.Churn, y_train_pred_final[i])
    total1=sum(sum(cm1))
    accuracy = (cm1[0,0]+cm1[1,1])/total1

    speci = cm1[0,0]/(cm1[0,0]+cm1[0,1])
    sensi = cm1[1,1]/(cm1[1,0]+cm1[1,1])
    cutoff_df.loc[i] =[ i ,accuracy,sensi,speci]

print(cutoff_df)
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

The key takeaways from this code are the accuracy, sensitivity and specificity values that have been calculated using the appropriate elements in the confusion matrix. The code outputted the dataframe given below.

![Sensitivity-Specificity Trade-off](https://i.ibb.co/rG6rQnv/Sensitivity-Specificity-Trade-Off.png)

As you can see, when the probability thresholds are very low, sensitivity is very high and specificity is very low. Similarly, for larger probability thresholds, the sensitivity values are very low but the specificity values are very high. And at about 0.3, the three metrics seem to be almost equal with decent values. Therefore, we choose 0.3 as the optimal cut-off point. The graph given below also showcases that at about 0.3, the three metrics intersect.

![Accuracy, Sensitivity, and Specificity tradeoff](https://i.ibb.co/tKrZ9jF/Accuracy-Sensitivity-and-Specificity-Trade-Off.png)

As you can see, at about a threshold of 0.3, the curves of accuracy, sensitivity and specificity intersect, and they all take a value of around 77-78%.

Now, as Rahim mentioned, you could have chosen any other cut-off point based on which of these metrics you want to be high. If you wanted to capture the 'Churns' better, you could let go of a little accuracy and choose an even lower cut-off, and if you wanted to capture more non churns you can choose higher cut-off. It is completely dependent on the situation that you are in. In this case, we just chose the 'optimal' cut-off point to give you a fair idea of how the threshold should be chosen.

#### Choosing the Optimal Cut-off

Qn: Suppose you created a dataframe to find out the optimal cut-off point for a model that you built. The dataframe looks like the following.

| **Threshold** | **Probability** | **Accuracy** | **Sensitivity** | **Specificity** |
| ------------- | --------------- | ------------ | --------------- | --------------- |
| 0.0           | 0.0             | 0.21         | 1.00            | 0.00            |
| 0.1           | 0.1             | 0.39         | 0.96            | 0.22            |
| 0.2           | 0.2             | 0.56         | 0.88            | 0.49            |
| 0.3           | 0.3             | 0.59         | 0.81            | 0.53            |
| 0.4           | 0.4             | 0.62         | 0.78            | 0.63            |
| 0.5           | 0.5             | 0.74         | 0.73            | 0.74            |
| 0.6           | 0.6             | 0.81         | 0.64            | 0.79            |
| 0.7           | 0.7             | 0.78         | 0.42            | 0.83            |
| 0.8           | 0.8             | 0.63         | 0.21            | 0.92            |
| 0.9           | 0.9             | 0.56         | 0.03            | 0.98            |

Based on this table, what will the approximate value of the optimal cut-off be?

- 0.4

- 0.5

- 0.6

- 0.7

Ans: B. *The optimal cut-off point exists where the values of accuracy, sensitivity and specificity are fairly decent and almost equal. At a cut-off of 0.5, the metric values are 0.74, 0.73 and 0.74, respectively. This is the optimal value of the threshold that you can have.*

#### Choosing a Model Evaluation Metric

Qn: As you learnt, there is usually a trade-off between various model evaluation metrics, and you cannot maximise all of them simultaneously. For example, if you increase sensitivity (% of correctly predicted churns), the specificity (% of correctly predicted non-churns) will reduce. 

Let's say that you are building a telecom churn prediction model with the business objective of your company to implement an aggressive customer retention campaign to retain the 'high churn-risk' customers. This is because a competitor has launched extremely low-cost mobile plans, and you want to avoid churn as much as possible by incentivising the customers. Assume that budget is not a constraint.

Which of the following metrics should you choose the maximise?

- Accuracy

- Sensitivity

- Specificity

Ans: B. *High sensitivity implies that your model will correctly identify almost all customers who are likely to churn. It will do that by overestimating the churn likelihood, i.e., it will misclassify some non-churns as churns, but that is the trade-off that you need to choose rather than the opposite case (in which case you may lose some low churn-risk customers to the competition).*

## Coming up

In the next segment, you will perform an exercise using the concepts that you have just learnt.
