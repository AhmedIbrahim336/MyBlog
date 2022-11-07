# How to make Box Plots in Python with Plotly.


![Box Plot example](https://cdn.hashnode.com/res/hashnode/image/upload/v1631543554700/Sn6Pbb0hhe.png)
Box plot is a way to visualize the relationship between *categorical* features (gender, obesity degrees ) and *continuous* features (age, weights)
But why it is important. The answer to this question can simply return to how it is actually designed. But before dig into its components lets introduce you to a statistics term *Quartile*

**Quartile** In statistics, a quartile is a type of quantile that divides the number of data points into *four parts*, or quarters, of more-or-less equal size. The data must be ordered from smallest to largest to compute quartiles. And we have three mean quartiles as follow:

**Q1:** is the **middle** number between the *minimum* and median of the data set. Also, you will find that *25%* of the data is blowing this point

**Q2:**: is the **median** of the data set. Thus **50%** of the data lies blew this point

**Q3:** is the **middle** value between the median and the *maximum* value of the data set.

**IQR(Interquartile Range):** is the length of the filled box Q3 - Q1


![box-plot-with-data.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631544501698/oO3HIrpKH.png)
Let's explain this in simple terms as you see in the previous image.
We have a data set at which the min value starts from `0` and the max at `50`. You can see simply the min and max points at the top and the bottom of the chart
At the middle you have the *median* value of this dataset.
The space between `Q1` and the *lower fence* represents *25%* between the min value and the median. So it makes so much sense to have the median on top of it and the min value blow it.  **`Q3`** is the same as `Q1` But now it is between the max value and the median. The middlebox is a difference between **`Q3`**  and  **`Q1`**. We also call it **IQR (Interquartile Range)**. the word `Inter` simply means between. and `quartile` means between Q3 and Q1

In statistics, the upper and lower fences represent the cut-off values for upper and lower outliers in a dataset. They are calculated as:

Lower fence = Q1 – (1.5*IQR)
Upper fence = Q3 + (1.5*IQR)


## When to use Box plots?
Box plots should go to your mind when you are comparing categorical features with continuous ones like (age vs weight).

# Basic Example With Plotly

```python
import plotly.offline as pyo
import plotly.graph_objs as go 

y = [1,14,14,15,16,18,18,19,19,20,20,23,24,26,27,27,28,29,33,54]

data = [
    go.Box(y=y),
]

pyo.iplot(data)

```

**S**taring by opening your jupyter notebook and Importing the plotly library 

On the `graph_objs` class we are creating a `Box` blot and passing only the `y` and plot it using the `iplot` method available at `pyo` class. As simple as that!!!

You should get something like this.
Notice when you hover it you can see various data points like the median, Q1, Q3, min, max values.
![box-plot-with-data.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631545509346/4-DKVqVa0.png)

We can make this more better by passing more options to the `Box` plot
For example, you can pass `poxpoints=all` you will be able to see the the points as well beside the box

```python
import plotly.offline as pyo
import plotly.graph_objs as go 

y = [1,14,14,15,16,18,18,19,19,20,20,23,24,26,27,27,28,29,33,54]

data = [
    go.Box(y=y, boxpoints='all')
]

pyo.iplot(data)

```
Other options for `boxpoints` are
- `outliers`: show only the min and the max points
- `False`: show nothing


![points-box-plot.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631545745854/XMzAwunRd.png)

you can also specify where you want to appear relative to the box (on the right or the left) by passing `pointpos=1` or you can replace `1` by positive or negative. As positive values will make the points appear at the right and the opposite for the negative ones. `pointpos=0` will display the points on top of the box  
```python
import plotly.offline as pyo
import plotly.graph_objs as go 

y = [1,14,14,15,16,18,18,19,19,20,20,23,24,26,27,27,28,29,33,54]

data = [
    go.Box(y=y, boxpoints='all', pointpos=0)
]

pyo.iplot(data)

```


![points-on-top-of-the-plot.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631546188002/-CT4sluOf.png)


# Real World Example With  [Abalone dataset](https://www.kaggle.com/rodolfomendes/abalone-dataset) 


In this example, I will be working with the **Abalone dataset**. You can find it available on [kaggle.com](kaggle.com)
 
```python
import plotly.offline as pyo
import plotly.graph_objs as go 
import pandas as pd 

df = pd.read_csv('./abalone.csv')

df.head() 
```
I downloaded the dataset and put it in same place as my jupyer notebook. 
Starting by importing plotly and pandas.
Then read the CSV file using the `read_csv` function available with pandas.
The data set will look like this.

![dataset.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631546541521/GQM-3ENe_.png)


We only interesting in the `rangs` columns as each abalone have multiple ranges and each one is different from the other ones. So we need to see the distribution of this feature between all abalones.

```python
data = [
    go.Box(
        y=df['rings'],
        boxpoints=False,
        name='Rings'
        
    )
]

pyo.iplot(data)
```
Passing the `rings` column to the `Box` plot.


![abalone-rings.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631546765127/joCfvHlkF.png)


