# Complete guide to handling categorical features with scikit-learn

# General Insights 
Most machine learning algorithms don't understand any text data like cities, jobs, or student grades. They only understand numbers. In most cases, we have to convert these *text-based features* into numbers. We call this ** category encoding**. 

A typical data scientist spends 70 – 80% of his time cleaning and preparing the data. And converting categorical data is an unavoidable activity. It not only elevates the model quality but also helps in better feature engineering. Now the question is, how do we proceed? Which categorical data encoding method should we use?

# Types Of Categorical Features 
- **Nominal** -- These are variables that are not related to each other in any order such as color (black, blue, green). 
- **Ordinal** --   These are variables where a certain order can be found between them such as student grades (A, B, C, D, Fail).

# Explore the iris dataset
In this tutorial, I will be using the  [Iris dataset](https://www.kaggle.com/uciml/iris) . You can find it available on  [Kaggle.com](https://www.kaggle.com/)

It includes three iris species with 50 samples each as well as some properties about each flower.  This means it has 150 rows. Our goal is to encode these `classes` into number

```python 
import pandas as pd 

df = pd.read_csv('./datasets/iris.csv')

df.head()

```

You should see this.  We are interested in the `Species` column we want to convert it into numbers. 

![Screen Shot 2021-09-20 at 3.10.45 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632143451425/s6L9hGg5r.png)


```python
[in] : df['Species'].unique()
[out]: array(['Iris-setosa', 'Iris-versicolor', 'Iris-virginica'], dtype=object)
```

# Map each type with the panda's map function 

```python
mapping = {
    'Iris-setosa': 1,
    'Iris-versicolor': 2,
    'Iris-virginica': 3
}

df['Species'] = df['Species'].map(mapping)
```
Above we are assigning each category a number and mapping it to the right category using the `map` function available in pandas.  


```python
[in] :df['Species'].value_counts()
[out]: 
3    50
2    50
1    50
Name: Species, dtype: int64
```




![encoding.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632146403833/atvjlcYov.png)
# Label Encoding

You can find what we just did already available in `sklearn` as `LabelEncoder`

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()

encoder.fit(df['Species'])

df['Species'] = encoder.transform(df['Species'])
```

Above, We are importing the `LabelEncoder` from the `preprocessing` module. Next initializing it and fitting the `Species` column into it. Finally, we are using the encoder to transform the `Species` columns and then update the data frame 


**Checking**


```python
[in] :df['Species'].value_counts()
[out]: 
3    50
2    50
1    50
Name: Species, dtype: int64
```

It is working!!!.
But we have a downside to this approach. What if in some situations the algorithm will be affected by the value of the number. For example, the algorithm may prepare `Iris-Virginia` with value `3` compared to  `Iris-setosa` with value one.  Of course, we have a solution for this which Is ... *One hot encoding*

# One Hot Encoding
A way to represent categorical data as binary (0,1). 

```python
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder()

encoder.fit(df['Species'].values.reshape(-1,1))

encoder.transform(df['Species'].values.reshape(-1,1)).toarray()
```

```python
# ----------------- output ------------------
array([[1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       [1., 0., 0.],
       ..................
```
As you see we have an array in each `row` only on sell have `one` that why we call it `OneHotEncoder`. By doing this the algorithm will have no bios toward specific categories.  

Let's see it as a Pandas's `DataFrame`

```python
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder()

encoder.fit(df['Species'].values.reshape(-1,1))

encoded = encoder.transform(df['Species'].values.reshape(-1,1)).toarray()

encoded_df = pd.DataFrame(encoded, columns=df['Species'].unique())

encoded_df.head()
```
Above we are importing `OneHotEncoder` from the 'preprocessing' module. Next, initializing it. When we `fit` the model we need to pass the column we need to fit as a 2d array. A quick way to do this in NumPy is to reshape it with `-1,1` dimensional. This will create an array with two dimensional but the second one will be `0` so it is one dimensional.  the same applying to the `transform`. The result of the `transform` and nd array according to two your number of categories. In the iris example, we have 3 categories so we are expected 3d array of zeros and ones. Next, we are creating a data frame out of it. 

||	Iris-setosa|	Iris-versicolor|	Iris-virginica|
|:--:|:---:| ----:|----:|
|0|	1.0|	0.0|	0.0|
|1	|1.0	|0.0	|0.0|
|2	|1.0	|0.0	|0.0|
|3	|1.0	|0.0	|0.0|
|4	|1.0	|0.0	|0.0|




could you think of one reason why just label encoding is not sufficient to provide to the model for training? Why do you need one hot encoding?

## What do you think?



















