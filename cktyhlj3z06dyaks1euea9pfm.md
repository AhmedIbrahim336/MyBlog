# Implement Linear Regression Algorithm From Scratch

It has never been easy to build machine learning models. You can reach 98% accuracy with the default parameters in Scikit-Learn. 

To complete this 2% you need a deep understanding of how these algorithms work behind the Scenes. This 2% can have a huge impact on the end result. For example, if you are building a self-driving car. even 9.95% is not enough we need more than that. 

In this tutorial, we will implement Linear Regression from scratch. 

Alert 🚨: We have some match here 👀. it is not that much 😊. 


## Importing Libraries 
```python
import numpy as np 
import matplotlib.pyplot as plt
import seaborn as sns 

%matplotlib inline 
```

Above we will be using Numpy for linear algebra. Also, we will need Matplotlib and seaborn for data visualization. 

## Create fake data 

```python
xs = np.array([1,2,3,4,5,6], dtype=np.float64)
ys = np.array([5,4,6,5,6,7], dtype=np.float64)
```
For now, we are using these data but later we will try our model on the iris dataset 😃.

## Visualize the data

```python

sns.set_theme()
sns.scatterplot(x=xs,y=ys)
plt.xlabel('X')
plt.ylabel('Y')
```


![scatter.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632491783955/ADej_cquI.png)


You will notice that we have some positive correlation between X and Y. Make this in your mind this will help us when we are going forward. 

## Line Equation 

Linear registration is based on founding the beast line that can fit the data like this line here. 

![Linear Regression](https://cdn.hashnode.com/res/hashnode/image/upload/v1632492002585/zoZb4m_lp.png)

If we can find the best line that fits with data perfectly (Like in the image), We can start making predictions

Remember in high school. The only way to draw this line is to have the *slope* of the line and the *y-intercept*. All of this according to this equation **`Y = m X + b`**

`m` :  the slope of the line `

`b`: y-intercept


## Calculating the slope 

To calculate the slope using the X and Y. We will use the equation below. 

```python
def get_slope(xs, ys):
    m = ((mean(xs) * mean(ys)) - mean(xs * ys)) / (np.power(mean(xs), 2) - mean(np.power(xs, 2)))
    return m

m = get_slope(xs, ys)

m
```

```python
#--------- ouput-----------
0.428
```

Now we have the slope of the line. What about the y-intercept. Luckly we can calculate it using the slope we just found. 

```python
b = np.mean(ys) - (m * np.mean(xs))
b
```

Now we have all that we need `m(slope)` `y-intercept`. let's create the equation. `Y= mX + b` According to this equation if we have feature X we can predict Y. Let's translate this as python code. 

```python
def predict(xs):
    return (m * xs) + b
```

## Visualize the predictions line 

```python
sns.lineplot(x=xs, y=regression_line, label='Regression Line')
sns.scatterplot(x=xs, y=ys)
plt.xlabel('X')
plt.ylabel('Y')
plt.legend()
```

![Visualize Linear Regression](https://cdn.hashnode.com/res/hashnode/image/upload/v1632493051291/_jjOC6rUe.png)

We will use this regression line to make our predictions. 

## Make predictions 
Will try to predict the output for three new values.

```python
new_xs = np.array([10, 8, 11])
predictions = predict(new_xs)

sns.lineplot(x=xs, y=regression_line, label='Regression Line')
sns.scatterplot(x=xs, y=ys, color='blue', label='Traning')
sns.scatterplot(x=new_xs, y=predictions, color='g', label='Predictions')
```


![scatter.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632493446970/uGnG2LWzA.png)

The green line represents our predictions. 



## Try our model on the iris dataset

You can find the  [iris dataset](https://www.kaggle.com/uciml/iris)  available on  [Kaggle.com](https://www.kaggle.com/) 


### Load and split the data

```python
from sklearn.datasets import load_iris

iris = load_iris()

X = iris['data'][:,1]
y = iris['target']
```

### Split into training and testing data

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
```

### Building the `LinearRegression` class

```python
class LinearRegiresion:
    def fit(self , xs, ys):
        m = ((mean(xs) * mean(ys)) - mean(xs * ys)) / (np.power(mean(xs), 2) - mean(np.power(xs, 2)))
        b = np.mean(ys) - (m * np.mean(xs))

        self.m = m 
        self.b = b 
        return self
    def predict(self, xs):
        return (self.m * xs) + self.b
```


### Train the model 

```python
lin_reg = LinearRegiresion()

lin_reg.fit(X_train, y_train)
```

### Evaluate the model with mean squared error
```python
from sklearn.metrics import mean_squared_error

y_pred = lin_reg.predict(X_test)
mean_squared_error(y_test, y_pred)

```

```python
#------------ ouput ----------
0.5353
```

So our model got `0.53`. It is like random guessing. Anyway, it was a great chance to look at what is really going on behind the scene.

let's compare it to Scikit-Learn 

```python
from sklearn.linear_model import LinearRegression

linear = LinearRegression()

linear.fit(X_train.reshape(-1,1), y_train)

y_pred = linear.predict(X_test.reshape(-1, 1))

mean_squared_error(y_test, y_pred)
```
```python
#------- ouput -------
0.515
```

Sklearn model got `0.51`. Our Model is performing better on this dataset 😎. 

# What do you think?  


