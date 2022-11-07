# Building Interactive dashboard with Plotly

# Plotly
In recent years, the interface and style of Matplotlib has begun to show their age. Newer tools like ggplot and ggvis in the R language, along with web visualization toolkits based on D3js and HTML5 canvas often make Matplotlib feel chunky and old fashioned. Still, Still, I’m of the opinion that we cannot ignore Matplotlib’s strength as a well-tested, cross-platform graphics engine
On of the best web based tools out their of course is **Plotly**. By small amount of effort you will be able to build **interactive plots and dashboards**

Now we know why you should care about Plotly let's get you first start with you first plot. But First let's setup the environment. I will be using normal python scripts. Feel free to use Jupyter notebook. you will get the same result. 

### Installing Plotly 
You can install plolty by simply opening you terminal and typing the following command 
```terminal
conda install plotly
```

# Your First Scatter Plot
```python
# --------------- file: plotly.py -------------- 
import plotly.offline as pyo 
import plotly.graph_objs as go 

np.random.seed(42)

x = np.random.randint(1,101, 100)
y = np.random.randint(1,101, 100)

# It have to be a list
data =[go.Scatter(
    x=x, 
    y=y, 
    mode='markers',
    text='X, Y',
    marker=dict(
        color='#60A5FA',
        size=12,
        symbol = 'diamond'
    )
)]

# Not a list
layout = go.Layout(
    title='X VS Y',
    xaxis = dict(title='MY X AXIS'),
    yaxis= dict(title='MY Y AXIS'),
    hovermode='closest',
)

fig = go.Figure(data=data, layout=layout)

pyo.plot(fig,filename='scatter.html')

```
After you installed the package if you run the prev example you should see a new tab on your default browser that has this.

![scatter.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631516015594/mnEwk87YH.png)
Feel free to play with it. it is interactive. You can **zoom in or out**. take **screenshots**. Interactive **hovering effects**  

## In-Depth explanation
First, we started by importing the library. You can think about it as a common convention to import it like this
The `go` interface includes all types of charts that we can create.
The `pyo` is the actual class the generate the plot
You can think about it as you create the plot with `go` and once you are done you can pass it to `pyo`  
```python
import plotly.offline as pyo 
import plotly.graph_objs as go 
```
### Get the data 
As you just get started let's generate random data using `numpy`. 
`np.random.randint(1,101,100)` => Getting 100 random integers between 1, 100
`np.random.seed(42)` => This simple to get the same result I will get. Feel free to change it from `42` to any number you wish or not setting it at all  
```python
np.random.seed(42)
x = np.random.randint(1,101, 100)
y = np.random.randint(1,101, 100)
```

## Building the trace 
I need your imagination at this point. We call the whole plot a `figure`. This is the final product that you are passing to the Plotly and gets the figure and each figure can have many `plots` as you want and we call it `trace` you can simply have one single figure with as many traces as you want. If you got this, let's create your first `trace`
```python
trace = go.Scatter(
    x=x, 
    y=y, 
    mode='markers',
    text='X, Y',
    marker=dict(
        color='#60A5FA',
        size=12,
        symbol = 'diamond'
    )
)

data = [trace]
```
We are at the core now. You can create a plot simply by calling `go`  with the type of plot you want to create. In this specific example, we will make a scatter plot. It accepts `x` and `y` with various other interesting options.

- `mode`: 
`mode='markers'` => You will have a plot with `points` and the points scattered over the plot 

 `mode='lines'` => It is the same as `markers` but all the points will be connected with each other like a `line chart`.


![lines.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631535923464/H8IaVavCp.png)

Not so pretty I know. We will go into this later.
- `text`: it is the text that will be displayed when you hover over a point on the plot
- `marker`: it is a python dictionary and it includes all the various styles for the plot points. `color` can be any color (red, green, hex, rgb, reba)

You will find the `color` and `size` very useful as you will see in the future examples that we can colorize markers based their categories and the same for the size a quick example

![bubble.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631536272693/WI5pnu7OH.png)
we are ploting `feature 1` against `feature 2` but also changes the colors based on `feature 3` and sizes base on `feature 4`. You will fine this quite handy. I will dedicate a specific blog post about it. As it is very useful in a lot of situations.  

### Adding Layouts 
In the Layout class we can specify different options like the `title` of the plot and name of the `x-axis` and the `y-axis`
```python
layout = go.Layout(
    title='X VS Y',
    xaxis = dict(title='MY X AXIS'),
    yaxis= dict(title='MY Y AXIS'),
    hovermode='closest',
    # hovermode='y'
    # hovermode='x'
    # hovermode="x unified"
)
```
**Note:** the `xaxis` and `yaxis` must be a Python dictionary with the propriety title you can't do this 
```python
layout = go.Layout(
    xaxis = 'This is the X-axis'
    yaxis= 'This is the Y-axis,
)
```

Also, you can pass a direct python dictionary instead of using the `dict` function. **It is the same**
```python
layout = go.Layout(
    xaxis = {'title':'MY X AXIS'},
    yaxis= {'title':'MY Y AXIS'},
)
```
Now on the `hovermode` it is about what you want to see when you have over the plot points it have different options but the default one is `hovermode='closest'` which means show the data related to the nearest point

Another option you might be interested in is `hovermode`
```python
hovermode='y' # Show only the `Y` value of the point when I hover over it
hovermode='x'  # The same but for the `X` value
hovermode="x unified" # Show vertical line across the x axis 
hovermode="y unified" # The same but for the y axis
hovermode=False # Cancel the hover mode
```
### Creating Figure
Then you create a new figure and pass it the layout and data you just created. 
```python
fig = go.Figure(data=data, layout=layout)
```
### Finally See You Figure
You have two options here 
If are typing a normal python script you have two options 
1. Saving the plot into an HTML file by using `pyo.plot()` and passing the filename. If you don't pass any name the default one will be `temp-plot.html`

```python
pyo.plot(fig,filename='scatter.html')
```
2. Let's say you are exploring the data and don't want all files or simply you will not share it with anyone. You will find the other option `pyo.iplot()` quite useful as it will open a new server and just showing you the HTML page without saving it.

```python
pyo.iplot(fig)
```
I am sure that If you are using **jupyter notebook**, you will want to print it in the same notebook you can use the interactive option that we just used `iplot()`. `i` for interactive 
```python
![plotly.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631538151336/OeTGZ5uSL.png)
pyo.iplot(fig)
```
You can still save it in a file from the jupyter notebook 
```python
pyo.plot(fig,filename='scatter.html')
```





![scatter.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631537849683/TdoiGeIkn.png)









