# How to create Bar and Line Plot with Plotly

# Bar Chart
### What they are? and when to use them?
A bar chart presents **categorical** data with rectangular bars with heights (or lengths ) proportional to the values that they represent
Categorical data versus continuous data 

In general variables and data either represent measurement on some continuous scale, or they represent information about some categorical or discrete characteristics
For example, the weight, height, age  would represent continuous variables 
However, a person's gender or occupation, or marital status are categorical or discrete variables
**Using Bar charts, we can visualize categorical data** 

Typically the *x-axis* is the categories and the *y-axis* is the count in each category
However and it's so common that the y-axis can be any **aggregation like (count, sum, average, etc..)**


#Bar Plot With Plotly
I will be using the famous [Iris dataset](https://www.kaggle.com/uciml/iris)

It includes three iris species with 50 samples each as well as some properties about each flower. 

![iris.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631595201871/gOXGzf-x2.png)
In this example, we have the categorical feature which is the class of the iris and many continuous features like the length or the width. We will plot the `class` against the `sepal_length`

```python
import pandas as pd 
import plotly.offline as pyo
import plotly.graph_objs as go

df = pd.read_csv('.iris.csv')

layout = go.Layout(
    title='Iris Sepal Length',
    xaxis=dict(title='Iris Class'),
    yaxis=dict(title='Sepal Length')
)

data = [trace]

layout = go.Layout(title='Iris Sepal Length')

fig = go.Figure(layout=layout, data=data)

pyo.iplot(fig)
``` 

Starting by importing the Plotly library and Pandas. then read the CSV file. The main Idea of Plotly is you can create as many traces as you want then you pass it to the `pyo` to plot it. And this is what I just did. I created a Bar plot with `go.Bar()` and passed the `x` and `y`. the x and y can be any python list or NumPy array or pandas column or index. Make sure you are putting the trace on an array list. As you can have as many of them as you want. Then We are adding the title of the plot by creating a new layout `go.Layout()` then adding an x-axis label and y-axis label.  And then Create the figure and plot it. 
You should see this.




![bar.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631622987635/TKi5sBo1T.png)


# Line Chart

![line.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631623349904/UU33l0YBI.png)

 **I**s a type of chart that displays information as a series of data points connected by straight line segments.

You can think of it as a scatter plot but the points are connected to each other.

## Line Plot With Plotly
```python
import numpy as np 
import plotly.offline as pyo 
import plotly.graph_objs as go 
```
Importing Plotly and NumPy to generate data sequence.

```python
np.random.seed(56)

x = np.linspace(0, 10, 100)

y = np.sin(x) 
```
Adding random seed `np.random.seed()` you can pass any number. This is just to get the same result as me. Use `np.linspace` to generate **normally distributed data.**
We will plot this against its `sin()` thus `y=np.sin(x)`
This is how normally disrupted data looks like.
![normal-distribution-2.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1631623801325/U8FPr9TAn.gif)


```python
trace = go.Scatter(
    x=x,
    y=y,
    mode='lines+markers+text',
    marker = dict(
        color='#818CF8',
    ),
    name='Markers'
)

```

The Idea behind Plotly is that you are creating as many **plots (traces)** as you want and passing it to one **figure**
You might find it weird that we are creating scattered plot `go.Scatter()` but as a said previously the main difference between a Scatter plot and a Line plot is the line that connects all points together. Also on an additional note, it is not only the line that makes a difference. But Also when You should use scattered plot and line plot

**Scatter Plot ** used to see the classification and distribution of the data

**Line Plot** is used to see the continuous patterns that exist on a specific feature. 

Go back to out plot. We are creating a `go.Scatter()` and padding `x` and `y` we just created. The key point is passing the right mode.
If this example we feed a line throght the data **(Line Plot)** So we are passing `mode='lines+markers+text'` also you can pass `mode='lines'`.
But If you want a scattered plot, the mode will be `mode='markders'`

```python
layout = go.Layout(
    # Title of the plot
    title='X VS Sin(X)',
    # Labels of the x and y axes
    xaxis=dict(title='X Values'),
    yaxis=dict(title='Sin(X)'),
    hovermode='closest'
)

fig = go.Figure(data=[trace], layout=layout)

pyo.iplot(fig)
```
Finally, we are adding some layouts to our plot and plot it. 
**Note: **Make sure to pass the `xaxis` and `yaxis` as a python dictioanlry  

If every thing went well, you should see this.

![line.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631624728964/I5zetK4T8.png)


# Example With multiple plots 

```python
import numpy as np 
import plotly.offline as pyo 
import plotly.graph_objs as go 

np.random.seed(56)

x = np.linspace(0, 10, 100)

y = np.sin(x)

trace1 = go.Scatter(
    x=x,
    y=y,
    mode='lines+markers+text',
    marker = dict(
        color='#818CF8',
    ),
    name='Markers'
)

trace2 = go.Scatter(
    x=x,
    y=np.cos(x) - 2,
    mode='lines',
    marker = dict(
        color='#818CF8',
    ),
    name='Lines'
)

layout = go.Layout(
    title='X VS Sin(X)',
    xaxis=dict(title='X Values'),
    yaxis=dict(title='Sin(X)'),
    hovermode='closest'
)

fig = go.Figure(data=[trace1, trace2], layout=layout)

pyo.iplot(fig)
```

![lines.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631624866398/_S6cnUCsA.png)











 