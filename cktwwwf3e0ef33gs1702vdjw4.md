# Evaluating a machine learning model.


So you've built a machine learning model and trained it on some data...now what?. In this post, I'll discuss how to evaluate your model.

# The train/test split

The most important thing you can do to properly evaluate your model is to not train the model on the entire dataset. A typical train/test split would be to use 80% of the data for training and 30% of the data for testing. 

## Metrics
1. Accuracy Score
2. Precision/recall
3. Classification Report
4. F1 Score
5. Confusion Matrix
6. Mean Squared Error

# Accuracy

**Accuracy** is defined as the percentage of correct predictions for the test data. It can be calculated easily by dividing the number of correct predictions by the number of total predictions.

```python
from sklearn.ensemble import RandomForestClassifier

rad_reg = RandomForestClassifier()

rad_reg.fit(X_train, y_train)

rad_reg.score(X_test, y_test)
```
Above, We are training a `RandomForestClassifier` on the training data. Next, we are calculating the accuracy of the model using `.score()` method and passing the test data. The output should be a number between `0`  and `1`. `0` means our model is performing poorly. `1` our model is perfect.

```python
#----output----
0.8720059523809525
``` 
 
Another way to do it is to use the `accuracy_score` function from the `metrics` module.
 
```python
from sklearn.metrics import accuracy_score 

y_pred = rad_reg.predict(X_test)

accuracy_score(y_test, y_pred)
```
Above, We are importing the `accuracy_score` from `metrics`. Making some predictions. Next, Passing these predictions to the `accuracy_socre`. It will compare the **actual result with the predicted one**

# Classification metrics 
When performing classification predictions, there are four types of outcomes that could occur.

- **True positives** are when you predict an observation belongs to a class and it actually does belong to that class.
- **True negatives** are when you predict an observation does not belong to a class and it actually does not belong to that class.
- **False positives** occur when you predict an observation belongs to a class when in reality it does not.
- **False negatives** occur when you predict an observation does not belong to a class when in fact it does.

Based on this we can compare the **predictions** versus the **true labels**

![classification-report.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632398516098/ICH-SE48O.png)
 
The dimentians of the classification metrics will grow as your features grow for example if you have `three` labels you will have `3x3` table

```python
from sklearn.metrics import confusion_matrix 

confusion_matrix(y_pred, y_test)
```
Above we are importing the `confusion_metrix` from `sklearn`. Note it is the same as classification metrics. 
```python
#------output-------------
array([[12,  0,  0],
       [ 0, 10,  0],
       [ 0,  2,  6]])
```
It is 3x3 array. Can we do better...? of course. lest's display this array in a `pandas` data frame. and visualize it using `matplotlib`.

```python
from sklearn.metrics import confusion_matrix 

classification = confusion_matrix(y_pred, y_test)

classification_df = pd.DataFrame(classification,columns=df['class'].unique(), index=df['class'].unique())

sns.heatmap(classification_df)
```


![heatmap.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632399025468/js2h9NrPB.png)

# Precison/ Recall
**Precision** is defined as the fraction of relevant examples (true positives) among all of the examples which were predicted to belong in a certain class.

**Recall** is defined as the fraction of examples that were predicted to belong to a class with respect to all of the examples that truly belong in the class.

```python
from sklearn.metrics import precision_score, recall_score

precision_score(y_test, y_pred, average='micro'), recall_score(y_test, y_pred, average='micro')
```

We are following the same steps. Importing then passing the predictions and the actual results. Thanks to Scikit-Learn for their awesome API. 

```python
# ------------ouput--------------------
(0.9333333333333333, 0.9333333333333333)
```



# F1 Score

F1 Score is needed when you want to seek a balance between Precision and Recall. Right…so what is the difference between F1 Score and Accuracy then? We have previously seen that accuracy can be largely contributed by a large number of True Negatives which in most business circumstances, we do not focus on much whereas False Negative and False Positive usually has business costs (tangible & intangible) thus F1 Score might be a better measure to use if we need to seek a balance between Precision and Recall AND there is an uneven class distribution (a large number of Actual Negatives).


**Note:**  1 in F1 Score doesn't mean something. it just how we write it.



```python
from sklearn.metrics import f1_score

f1_score(y_test, y_pred )
```
```python
# -------- output --------
0.9333333333333333
```

# Classification Report 
A common way to combine all of these concepts into one metrics is to use `classificatioin_report` offered by sci-kit-learn.

```python
from sklearn.metrics import classification_report 

print(classification_report(y_test, y_pred))
```

Notice that you need to print the result else you will get unreadable test. 

```python
             precision    recall  f1-score   support

           0       1.00      1.00      1.00        12
           1       1.00      0.83      0.91        12
           2       0.75      1.00      0.86         6

    accuracy                           0.93        30
   macro avg       0.92      0.94      0.92        30
weighted avg       0.95      0.93      0.94        30
```



Above you have the precision, recall, f1 score, and accuracy for each class you have in the dataset.

# Mean Squared Error 

Mean squared error is simply defined as the average of squared differences between the predicted output and the true output. Squared error is commonly used because it is agnostic to whether the prediction was too high or too low, it just reports that the prediction was incorrect.


# What do you think?
