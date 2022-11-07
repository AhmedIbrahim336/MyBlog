# How to Create Heatmaps with Plotly

# Heatmaps
> WE MOSTLY USE OUR EYES TO MAKE SENSE OF THE WORLD, **NOT COLUMNS OF NUMBERS**

**Heatmaps** allows the visualization of 3 features 
Categorical or continuous features along the x and y-axis
to make up a grid, and then a 3d continuous feature displayed
through color. 

![jet-heatmap.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631627789894/OAognbVWp.png)

**H**eatmaps is a powerful tool. You can get some more insight into the data just by plotting specific features on a heatmap.

Let's consider this example. we had recorded the number of passengers over that travel every month and across the years 
it looks like this 

![flights.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631628288912/A6-jArER7.png)

This is a classic example of when to use heat maps. We have three features `year`, `month`, and `passengers`.
If we plot the `year` aginst `passengers` in a line chart you will get something like this.

![line.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631628567112/6v9ZGvgMy.png)

Besides, it doesn't look so good. We can make it better than that. And the answer is **Heatmaps**.  
To get better understanding let's start by creating your first heatmap with **Plotly**

# Heatmaps with Plotly
The heatmap displays three features so we will pass the `year`, `month`, and `passengers`

```python
import plotly.offline as pyo
import plotly.graph_objs as go
import pandas as pd 

```
The first step is importing pandas and plotly

```python
flights = pd.read_csv('../../Data/flights.csv')
```
Read the data CSV file from the current directory
```python
trace = go.Heatmap(
    x=flights['month'],
    y=flights['year'],
    z=flights['passengers'],
)
``` 
The main idea behind **Plotly** is you can create as many plots (**traces**) as you want and then passing them to **one figure** and finally plotting the figure. 
No we only want to see one plot which is one heatmap.
Starting by `go.Heatmap()` and then passing the `x`, `y`, and `z` 
`x and y` will be the x and y axes
but the `z` column will be the color scale that you can see on the right-hand side of the image.

 **In summary**  we are two features against each other but also adding a third feature. The way we can visualize the change of the third feature across the first and second ones is through color change across the heatmap.
```python
data = [trace]

layout = go.Layout(
    title='Flights',
)

fig = go.Figure(layout=layout, data=data)

pyo.iplot(fig)
```
Finally, we are creating a layout object that has different options. We only add the `title` of the plot. and then creating a figure that requires both the data and the layout we had just created then we plot then using `pyo.iplot(fig)`

You should get something like this.


![heatmap.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631630796178/BWvswUTHd2.png)

 You man consider using different colors. In plotly we call this `colorscale` and there are many color scales you can use. 

In the prev example if we consider padding `colorscale='jet'` when creating the heatmap 

```python
trace = go.Heatmap(
    x=flights['month'],
    y=flights['year'],
    z=flights['passengers'],
    colorscale='Jet'
)
```

![heatmap map with Jet color scale](https://cdn.hashnode.com/res/hashnode/image/upload/v1631630985831/d8J7Qf0My.png)

**A**nother common color scale available on plotly is `Viridis`
```python
trace = go.Heatmap(
    x=flights['month'],
    y=flights['year'],
    z=flights['passengers'],
    colorscale='Viridis'
)
```



![heatmap.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631631131339/7C49lTNgI.png)

You may want to create your own color scales by adding an array of the colors you want 

```python
trace = go.Heatmap(
    x=flights['month'],
    y=flights['year'],
    z=flights['passengers'],
    colorscale= [
    [0, 'rgb(250, 250, 250)'],        #0
    [1./10000, 'rgb(200, 200, 200)'], #10
    [1./1000, 'rgb(150, 150, 150)'],  #100
    [1./100, 'rgb(100, 100, 100)'],   #1000
    [1./10, 'rgb(50, 50, 50)'],       #10000
    [1., 'rgb(0, 0, 0)'],             #100000
    ]
)

```
You should get these gray-black colors.


![heatmap with custom color scale](https://cdn.hashnode.com/res/hashnode/image/upload/v1631631396056/x59ilEszA.png)

# Heatmap with Unequal Block Sizes
The wonderful thing about Plotly heatmaps is they working everywhere even if we have missing values or missing blocks!!!

```python
import plotly.graph_objects as go
import numpy as np

# Build the rectangles as a heatmap
# specify the edges of the heatmap squares
phi = (1 + np.sqrt(5) )/2. # golden ratio
xe = [0, 1, 1+(1/(phi**4)), 1+(1/(phi**3)), phi]
ye = [0, 1/(phi**3), 1/phi**3+1/phi**4, 1/(phi**2), 1]

z = [ [13,3,3,5, 12, 15],
      [13,2,1,5, 0, 11],
      [13,10,11,12, 22, 9],
      [13,8,8,8, 10, 2]
    ]

fig = go.Figure(data=go.Heatmap(
          x = np.sort(xe),
          y = np.sort(ye),
          z = z,
          type = 'heatmap',
          colorscale = 'Viridis'))

# Add spiral line plot

def spiral(th):
    a = 1.120529
    b = 0.306349
    r = a*np.exp(-b*th)
    return (r*np.cos(th), r*np.sin(th))

theta = np.linspace(-np.pi/13,4*np.pi,1000); # angle
(x,y) = spiral(theta)

fig.add_trace(go.Scatter(x= -x+x[0], y= y-y[0],
     line =dict(color='white',width=3)))

axis_template = dict(range = [0,1.6], autorange = False,
             showgrid = False, zeroline = False,
             linecolor = 'black', showticklabels = False,
             ticks = '' )

fig.update_layout(margin = dict(t=200,r=200,b=200,l=200),
    xaxis = axis_template,
    yaxis = axis_template,
    showlegend = False,
    width = 700, height = 700,
    autosize = False )

fig.show()
```

![unequal-blocks.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631632029369/EaVADPfrZ.png)




 