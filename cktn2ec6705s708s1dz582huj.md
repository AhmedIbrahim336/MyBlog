# How to create Histograms and Distribution plots with Plotly?

# Histogram
A histogram displays an accurate representation of the overall distribution of a continuous feature.

To create a histogram, we divide the entire range of values of the continuous feature into series of intervals.

This series of intervals is known as **"bins"**.

We then count the number of occurrences per bin.

We can change the bin size to get either more or fewer details.
 
Le's view some examples...

## Hist Plot
#### Requirements
- I will be using the  [Abalone](https://www.kaggle.com/rodolfomendes/abalone-dataset)  data set. You can find it available on kaggle.com. Feel free to download it so you can follow along.

```python
import pandas as pd 
import plotly.offline as pyo 
import plotly.graph_objs as go
```

The first step is importing both Pandas and Plotly. the `graph_objs` has all various types of graphs **(figures, plots)** you want to create. The graph you will create from `go` you will pass it to `pyo` to plot it as an **html** file or as an **image**. 

```python
df = pd.read_csv('./abalone.csv')

trace = go.Histogram(x=df['length'], xbins=dict(start=0, end=1, size=0.02))

data = [trace]
```
We are reading the Abalone data set using Pandas. Make sure you have the data set in the same directory. 

Plotly works because you can create as many plots **(we call them traces)** as you want and then pass it to one figure then pass this figure to Plotly to display it.

I am creating a new histogram using `go.Histogram` and passing the `abalone.length` to the x-axis for the plot. also notice that we are passing another interesting option which is the `xbins`

**xbins: **
You can decide which slice of data you want to display using the `start` and `end` options. For example, the `length` in the data set has a range from `0` to `1` if you want to see only the range from `0` to `0.5` you can specify the `start` as `0` and the end as `0.5`.

**Note: ** Make sure you are creating the `data` as a Python list as I said you can create as many **traces** as you want. Now we are creating only one.

```python
layout = go.Layout(
    title='Abalone Length',
    xaxis=dict(title='Length')
)

fig = go.Figure(layout=layout, data=data)

pyo.iplot(fig)
```   
Now we need to add a title and a label to the plot. You can do this by creating a new layout `go.Layout` various layout options.
 
**`title`:** The Plot Title

**`xaxis`: ** Python dictionary has all the options you want to add to the axis. we only specify the `title` for the x-axis

Note: Note you have to pass a Python dictionary to the `xaxis` option.


![hist.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631803507975/kfhaAqgxi.png)



# Distribution Plot 

Distribution Plots, or Distplots, typically layer **three plots** on top of one another. The first is a **histogram**, where each data point is placed inside a bin of similar values. The second is **rug plot** - marks are placed along the x-axis for every data point, which lets you see the distribution of values inside each bin. 

Lastly, Distribution plots often include a **" kernel density estimate"** or **KDE** line that tries to describe the shape of the distribution.

A **rug plot** is a plot of data for a single quantitative variable, displayed as marks along an axis.

**kernel density estimation (KDE)** is a non-parametric way to estimate the probability density function of a random variable.

Let get your first example...

## Dist Plot With Plotly

#### Requirement
- I will be using the  [Iris](https://www.kaggle.com/uciml/iris)  data set. You can find it available on kaggle.com. Feel free to download it so that you can follow along.


```python
import plotly.offline as pyo
import plotly.figure_factory as ff
import pandas as pd
```
The first step is to import both pandas and Plotly. Notice we are this time importing the `figure_factory` it is the same as `graph_objs` but can do more complex plots like Dist plot.


```python
iris = pd.read_csv('./iris.csv')

iris.head()
```
Our data set looks like this.
![iris.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631804184154/NlI8LC4xJ.png)

I will create a **Distribution plot** that has three **Hist Plots**

```python
hist_data = []
classes = iris['class'].unique()
for iris_class in classes:
   sepal_length =  iris[iris['class'] == iris_class]['sepal_length']
   hist_data.append(sepal_length)
```
We have three classes available in the **Iris** data set (Iris-setosa
, Iris-versicolor,Iris-virginica). We will create a hist plot for every one of these. 

```python
fig = ff.create_distplot(hist_data=hist_data, group_labels=classes, bin_size=[0.5,0.5,0.5,0.5])
pyo.iplot(fig)
```

From the `ff` we are creating a Dist plot using `ff.create_distplot` and passing the `hist_data` we had just created. Also, we need a label for each hist plot so are passing the classes as labels. As we saw with the Hitplots, we are passing the `bin_size`. the `bin_size` should be an array in which every item corresponds to a specific hist plot 


![dist.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631804668172/upQeKHpIe.png)
