# Custom Analytics Dashboards With Dash and Python

Dash is a python framework created by Plotly for creating interactive web applications. ... With Dash, you don't have to learn HTML, CSS, and Javascript in order to create interactive dashboards, you only need **python**. Dash is open source and the applications build using this framework are viewed on the web browser.


![dash-website.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1631973566680/8NbhTgTkR.gif)

In today's tutorial, we will create a simple web app that displays some analytics about the iris dataset. 

# Requirements 

I recommend following along using normal python scripts. You can download the  [iris dataset](https://www.kaggle.com/uciml/iris)  from  [kaggle.com](https://www.kaggle.com/).

Let's import the required packages and building the UI components.

## Importing libraries
```python
# -------------------- analytic.py-------------------
import dash 
from dash import dcc, html
from dash.dependencies import Output, Input
from dash.html.Div import Div
import plotly.offline as pyo
import plotly.graph_objs as go
import pandas as pd 
```
# Read and explore the data.

```python
df = pd.read_csv('./iris.csv')
```
Above, We are using pandas `read_csv` function to read the iris data set from the same directory. 
if you plot the data frame, you should see this.


![iris.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631974016008/0mfdjIYZb.png)

It includes three iris species with 50 samples each as well as some properties about each flower. One flower species is linearly separable from the other two, but the other two are not linearly separable from each other.

The columns are:

- Id
- SepalLengthCm
- SepalWidthCm
- PetalLengthCm
- PetalWidthCm
- Species

We want to create a scatter plot with each combination we have 5 columns so we have 5^2 plots possible. As you might guess already this can be difficult to visualize all of these relations. Luckily Dash made this easy.

# Create the Graph 
```python
def gen_graph(x, y, size=12, color='green', mode='markers'):

    # Build basic graph
    trace =  go.Scatter(
        x=df[x],
        y=df[y],
        mode=mode,
        marker = dict(
            size=size,
            color=color
        )
    )

    data = [trace]

    layout = go.Layout(
        title='{} VS {}'.format(x, y),
        xaxis=dict(title=x),
        yaxis=dict(title=y)
    )

    return go.Figure(layout=layout, data=data)
```
Each time we are selecting a new option from the dropdown we will be rendering a new graph so it makes sense to have a function that can generate this graph for you each time.

To have a dynamic graph you need to make everything customizable. And this is what I just did. The `x` and `y` will be padded from the dropdown. 

The main idea behind Plotly is that you can create as many plots (**traces**) as you want and then passing all of them into one figure and then plot it.

In this particular example, we are creating only one trace. It will be a scattered plot. It takes the `x` and `y` axis. These will be columns from the iris data set we will be using the dropdown to say which column should be passed.
The mode can be `markers` or `text` or `lines`. We will make a dropdown to control this a will. The same concept applies to the `colors` and the `size`.


# Creating the dash app

```python
app = dash.Dash()
```

# Generate options for the dropdowns 

```python
def gen_options():
    options = []
    for column in df.columns:
         options.append({'label': column, 'value': column})
    return options
```

Above, we are creating an `options` array at which every column in the iris dataset will be an option on the dropdown. 
 

![dropdown.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631975085817/51l7FXy8T.png)


# Building UI components
 
```python
x_axis_dropdown = dcc.Dropdown(
    options=gen_options(),
    value= gen_options()[0]['value'],
    id='x-axis'
)

y_axis_dropdown = dcc.Dropdown(
    options=gen_options(),
    value= gen_options()[1]['value'],
    style={'margin': '5px 0px'},
    id='y-axis'
)

color_slider = dcc.Slider(
    min=0,
    max=9,
    marks={i: 'Size of {}'.format(i) for i in range(10)},
    value=5,
    id='size-input'
)


colors_dropDown = dcc.Dropdown(
    options=[
        {'label': 'Gray', 'value': '#1F2937'},
        {'label': 'Red', 'value': '#F87171'},
        {'label': 'Yellow', 'value': '#FBBF24'},
        {'label': 'Green', 'value': '#34D399'},
        {'label': 'Blue', 'value': '#3B82F6'},   
    ],
    value= '#FBBF24',
    style={'margin': '5px 0px', 'width': '50%'},
    id='colors-dropdown'
)

mode_dropDown = dcc.Dropdown(
    options=[
        {'label': 'Markers', 'value': 'markers'},
        {'label': 'Text', 'value': 'text'},
        {'label': 'Lines', 'value': 'lines'},   
    ],
    value= 'markers',
    style={'margin': '5px 0px', 'width': '50%'},
    id='mode-dropdown'
)
```

We are creating a dropdown for the x-axis, y-axis, mode, and colors.  They all have the same concept. The `options` param should be an array of dictionaries in which `label` will be the text that will be displayed and `value`  is the actual value we will be using in side the application.   `id` is important as we will use it later to get the data from each component.
 
# Creating the layout 
In this example, we will not have a very complicated layout. will list all the components one after another.

```python
app.layout = html.Div([
    html.H1('Iris Dataset'),
    x_axis_dropdown,
    y_axis_dropdown,
    colors_dropDown,
    mode_dropDown,
    html.Div( children= color_slider, style=dict(margin='10px 0px', display = 'block')),
    dcc.Graph(figure=default_graph, id='graph')
])
```

Notice How are integrating `Plotly` with `Dash`. By passing the figure to `dcc.Graph()`. Of course, it needs an id as will update the graph once the dropdown or the slider change.

# Contol the state

```python
@app.callback(
    Output('graph', 'figure'),
    [
        Input('x-axis', 'value'), 
        Input('y-axis', 'value'),
        Input('size-input', 'value'),
        Input('colors-dropdown', 'value'),
        Input('mode-dropdown', 'value')
    ]
)
def update_graph(x, y, size, color, mode):
    if not color:
        color = '#93C5FD'
    return gen_graph(x=x, y=y, size=size, color=color, mode=mode)

```

All that we are created was just the UI. There is no logic in it. To add logic we need to update the state. 
For example, if an input has a value of `5` we call this the *state of the input* by changing it we are changing the state. 

In `Dash` we are updating the state using `callbacks`. The callback accepts output and a list of all the inputs. and this function will be called whenever the input change. `Output(id, property)` We want to update the graph so ware making it as an output and passing its id to the `Output`. But we need to be more specific about which part of the graph will are interesting in!!. Luckily we are intersted in updating the hole `figure`. So the `Ouput` takes the id of the component you want to update and the `property` you want to update it.

The same for the `Input`. You will be passing the id of the component you want to get the state from it. and the actual property you are interested in. We are interested in the `value` property for all of these fields.

**Notice** each item in the input array will be passed a parameter to the function `update_graph` that is why I have 5 inputs and 5 parameters.

the `update_graph` using the `gen_graph` function to generate a new graph using these new values. Notice that the `get_graph` function returns a new `figure` and we are returning it. Dash will use the return of this function to update the `output`.

# Run the app
```python
if __name__ == '__main__':
    app.run_server(debug=True, threaded=True)
```

We are running the app using `app.run_server()`.
I am using simple check here this means that we will be running the app only if I am running the file `analytics.py` directly. 

Also, I am passing `debug=True` and `threaded=True` to enable Live reloading


![dash-website.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1631976327212/yJNV0rb88.gif)




# What do you think?






















 





















