# Main Challenges of Machine Learning

![What Is Machine Learning](https://images.unsplash.com/photo-1550432163-9cb326104944?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1600&q=80)

# What Is Machine Learning?
Machine Learning is the science (and art) of programming computers so they can learn from data.

> [Machine Learning is the] field of study that gives computers the ability to learn without being explicitly programmed.
—Arthur Samuel, 1959

For example, If we want to build  ** a spam filter ** using traditional programming. We have to write complex code and thousands and thousands of checking and comparing different conditions. After all of this, we might have a functional program But does it adapt to the new changes in the system? As you might guess of course spammers will try to change their way of writing!!!!. Got the point... we need to handle it in a different way. 

What if the machine can recognize different patterns in the data (emails) and **take decisions based on this**. *Here where Machine Learning Shines*.



Machine Learning can help humans learn. ML algorithms can be inspected to see what they have learned (although for some algorithms this can be tricky). For instance, once the spam filter has been trained on enough spam, it can easily be inspected to reveal the list of words and combinations of words that it believes are the best predictors of spam. Sometimes this will reveal unsuspected correlations or new trends, and thereby lead to a better understanding of the problem.

Applying ML techniques to dig into large amounts of data can help discover patterns that were not immediately apparent. This is called *data mining*.



# Main Challenges of Machine Learning 

![Main Challenges of Machine Learning ](https://images.unsplash.com/photo-1501526029524-a8ea952b15be?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1600&q=80)


In short, the two things that can go wrong  are “bad algorithm” and “bad data.” Let’s start with examples of bad data.

## Insufficient Quantity of Training Data

As humans, we don't need so much data to learn. For a toddler, if he spent a couple of days with a dog. he can easily identify any dog on the plant!!. In machine learning, we don't there yet 😎 🚀 .

it takes a lot of data for most Machine Learning algorithms to work properly. Even for very simple problems you typically need thousands of examples, and for complex problems such as **image** or **speech recognition**, you may need millions of examples (unless you can reuse parts of an existing model).




## Nonrepresentative Training Data

Data should match reality. For example, if we have a cancer dataset and we want to classify it as *benign* or *malignant* . If the dataset has 80% of its case as malignant. You will have a model that will classify nearly every case as malignant because according to your *unbalanced dataset* The model will found itself right in most cases if he said *malignant*.  

It is crucial to use a training set that is representative of the cases you want to generalize to. This is often harder than it sounds: if the sample is too small, you will have sampling noise (i.e., nonrepresentative data as a result of chance), but even very large samples can be nonrepresentative if the sampling method is flawed. This is called sampling bias.



## Poor-Quality Data 
Obviously, if your training data is full of errors, outliers, and noise (e.g., due to poor-quality measurements), it will make it harder for the system to detect the underlying patterns, so your system is less likely to perform well. It is often well worth the effort to spend time cleaning up your training data. 
The truth is, most data scientists spend a significant part of their time doing just that. 
For example:
- If some instances are clearly outliers, it may help to simply discard them or try to fix the errors manually.

- If some instances are missing a few features (e.g., 5% of your customers did not specify their age), you must decide whether you want to ignore this attribute altogether, ignore these instances, fill in the missing values (e.g., with the median age), or train one model with the feature and one model without it, and so on.

## Irrelevant Features 

Your system will only be capable of learning if the training data has relevant features and not so much noise. A critical part of the success of a machine learning project is coming up with a good set of features. This process is called *feature engineering*. 

- Feature selection: selecting the most useful features to train on among existing features.
- Feature extraction: combine existing features to get a more powerful one (dimensionality reduction algorithms can help 🙌).



![overfitting](https://images.unsplash.com/photo-1558021212-51b6ecfa0db9?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1600&q=80)

 ## Overfitting the Training Data

Say you are visiting a foreign country and the taxi driver rips you off. You might be tempted to say that all taxi drivers in that country are thieves. What you just did is a normal part of human psychology. Machine learning is no exception they can fall into the same trap if we are not careful. In machine learning we called this *overfitting*


Overfitting happens when the model is too complex relative to the amount and noisiness of the training data. The possible solutions are:

- To simplify the model by selecting one with fewer parameters (e.g., a linear model rather than a high-degree polynomial model), by reducing the number of attributes in the training data or by constraining the model

- To gather more training data

- To reduce the noise in the training data (e.g., fix data errors and remove outliers)


![underfitting](https://images.unsplash.com/photo-1489976908522-aabacf277f49?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1600&q=80)

# Underfitting the Training Data
As you might guess, underfitting is the opposite of overfitting: it occurs when your model is too simple to learn the underlying structure of the data. For example, a linear model of life satisfaction is prone to underfit; reality is just more complex than the model, so its predictions are bound to be inaccurate, even on the training examples.

The main options to fix this problem are:
-  Selecting a more powerful model, with more parameters
-  Feeding better features to the learning algorithm (feature engineering)
-  Reducing the constraints on the model (e.g., reducing the regularization hyper‐ parameter)

## What do you think 💭 ?



