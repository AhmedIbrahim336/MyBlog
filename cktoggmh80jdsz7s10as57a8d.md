# Building Frontend Websites For Data Analysis with Dash For Python Developers

# Introduction to Dash
Downloaded 600,000 times per month, Dash is the original low-code framework for rapidly building data apps in Python, R, Julia, and F#.

Written on top of Plotly.js and React.js, Dash is ideal for building and deploying data apps with customized user interfaces in pure Python, R, Julia, or F#. It's particularly suited for anyone who works with **data**.

Through a couple of simple patterns, Dash abstracts away all of the technologies and protocols that are required to build a **full-stack web app** with interactive data visualization.

**Dash** is simple enough that you can bind a user interface to your Python, R, Julia, or F# code in less than 10 minutes.

Dash apps are rendered in the web browser. You can deploy your apps to **VMs** or **Kubernetes clusters** and then share them through URLs. Since Dash apps are viewed in the web browser, Dash is inherently cross-platform and mobile ready.

## Installation
In your terminal install `dash`
```bash
pip install dash
```
You also need pandas as peer dependencies
```bash
pip install pandas 
```

## Importing `Dash` and running the app

```python
#------ basic.py---------
import dash 
from dash import dcc, html
```
`dcc` (dash core components ) has prebuild and functioning components like a dropdown, Input, range, slider, ...etc


`html` has the normal HTML like `h1`, `p`, `img`, ...etc

```python
#------ basic.py---------
app = dash.Dash()

app.layout = html.Div('Hello, Dash!')

app.run_server()
```
Starting by creating the main app with `dash.Dash()`. and adding `layout`. You can't start your app without including the layout.
Now we only include one heading with text `Hello, Dash!`.

Running the application using `app.run_server()`

Then run this python file 
```bash

 python basic-example.py 
Dash is running on http://127.0.0.1:8050/

 * Serving Flask app "basic-example" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:8050/ (Press CTRL+C to quit)
127.0.0.1 - - [17/Sep/2021 14:52:12] "GET /_reload-hash HTTP/1.1" 200 -
127.0.0.1 - - [17/Sep/2021 14:52:12] "GET /_dash-dependencies HTTP/1.1" 200 -
```
Now you have your server running on port `8050`. You can open this link here `http://127.0.0.1:8050/` and you can see this 


![hello-dash.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631883282408/GtfoB1EXgQ.png)

Awesome right!
Let's make some refractory to make the code looks better.
If now update something in the `basic.py` file the server will not reload. You will find it difficult to keep close and open the server when you are updating the file. We call this **Hot Reloading**
Let add it to our app.
```python
if __name__ == '__main__':
    app.run_server(debug=True, threaded=True)
```
Now we are padding two options to the `run_server` function. Both of them are required to be able to have hot reloading

if you notice I added a new line of code 
```python
if __name__ == '__main__':
```
This is to make sure we are running this file `basic.py` directly as in bigger projects you can have this file working independently or part of your project 

## Adding HTML Elements
```python
# ----------- basic.py -------------
app.layout = html.Div(children=[
    html.H1('Hello, Dash!'),
    html.P(children='This is normal text'),
    html.Ul(children=[
        html.Li('First'),
        html.Li('Second'),
        html.Li('Third'),
        html.Li('Forth'),
    ])
])
```
We will replace the previous `layout` with this new one. You can imagine HTML as a tree and you can have as many branches as you want. In code, it looks like nesting. In this example, we have a `div` with three ** children** or branches. We have headings, Paragraphs, and Lists.
Notice we have an Unordered list `Ul` that have its children as `Li`
If You run the project you will get this.  


![html.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631884220717/J7kZONWvt.png)

# Styling HTML Elements
No one ever will accept to use this website in this current state. And this moves us to our next point which is **Stying**. You can Style each component by padding `style` property as python dictionary with various styles you want to apply color, background, font-size. You can think of it as you are typing normal CSS with slite variation. For example, you will not type `font-size` instead you will type `fontSize`. The same applies to every style you will ever encounter. 

`background-color` -> backgroundColor

`list-style` -> listStyle

So we can edit the previous `layout` to be like this one 
```python
app.layout = html.Div(children=[
    html.H1('Hello, Dash!', style= {
        'color': '#EFF6FF',
        'backgroundColor': '#1E3A8A',
        'padding': '10px 32px'
    }),
    html.P(children='This is normal text', style={
        'padding': 23,
        'fontSize': '32px'
    }),
    html.Ul(children=[
        html.Li('First'),
        html.Li('Second'),
        html.Li('Third'),
        html.Li('Forth'),
    ], style={
        'display': 'flex',
        'alignItems': 'center',
        'justifyContent': 'space-between',
        'listStyle': 'none'
    })
])
```

And now we have this.


![style.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631884971994/8qgL-nAVn.png)


In most cases, you will not build every component thing from scratch because Dash has its own library that did this for you. Let take a look...

## Dash Core Components 

You can import prebuild components like a dropdown or a slider or Input or textarea or checkboxes, ... etc
You can reference the hole list on the main [ dash documentation](https://dash.plotly.com/dash-core-components) 

It is very similar to what we just did. We will just replace the HTML part with the components one.
Create new file and name it `core.py`
```python
# ------------------core.py--------------
import dash
from dash import html, dcc

app = dash.Dash(__name__)
app.layout = html.Div([
    dcc.Dropdown(
        id='demo-dropdown',
        options=[
            {'label': 'New York City', 'value': 'NYC'},
            {'label': 'Montreal', 'value': 'MTL'},
            {'label': 'San Francisco', 'value': 'SF'}
        ],
        value='NYC'
    ),
])


if __name__ == '__main__':
    app.run_server(debug=True, threaded=True)
```

We now creating a dropdown from `dcc` that we imported from `dash`. Each compoent has it's own properties you can pass but the most common ones are the `id` and `value` that is shared between many components. 

Now run `python core.py`

You should get this simple dropdown


![dropdown.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631885701169/RmHoaqQvsH.png)


Now let's add a **slider** by editing the layout and add this code blew 
```python
app.layout = html.Div([
    dcc.Dropdown(
        id='demo-dropdown',
        options=[
            {'label': 'New York City', 'value': 'NYC'},
            {'label': 'Montreal', 'value': 'MTL'},
            {'label': 'San Francisco', 'value': 'SF'}
        ],
        value='NYC'
    ),
   html.Div([
    dcc.Slider(
        min=0,
        max=9,
        marks={i: 'Label {}'.format(i) for i in range(10)},
        value=5,
    )
   ], style={
       'margin': '40px 0px'
   })
])

```
Notice I added a new `Div` as wrapper for the `Slider`
The slider tasks `min` and `max` values. Also a default value `value=5`, A marks or labels for every point on the slider. 
You will understand this better when you run the app and play around with it.


![slider.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631886053205/TrrOFl6Ir.png)

Besides the old dropdown, you can see a slider with a default value equal to 5 and a label for each point on the slider.

**In the future**, we will be able to get the value from the slider and make sum logic on it.

# Add Plotly to Dash
If you have worked with Plotly before you can find it make sense to make an integration between Plotly and Dash.

Create new file and call it `plotly-dash.py`
1. Import libraries 
```python
import dash
from dash import dcc, html
import plotly.offline as pyo
import plotly.graph_objects as go 
import numpy as np
```
2. Generate random data using NumPy
```python
np.random.seed(42)
x = np.linspace(0, 10, 100)
y = np.sin(x)
```
3. Building scatter plot
```python
trace = go.Scatter(
    x=x,
    y=y,
    mode='markers',
    marker={
        'size': 12,
        'color': 'white',
        'symbol': 'pentagon',

    }
)
data = [trace]
```
4. Add Layout to the figure
     ```python
         layout = go.Layout(
         title='X VS Sin(X)',
         xaxis= dict(title='X'),
         yaxis=dict(title='Sin(X)'),
         paper_bgcolor='#D1FAE5',
         plot_bgcolor='#6EE7B7'
        )

       fig = go.Figure(data=data, layout=layout)
       ```
5. Create the dash app and run the server

```python
app = dash.Dash()

app.layout = html.Div(children=[
    html.H1('Hello, Dash!!', style={
        'color': '#34D399'
    }),
    dcc.Graph(id='scatterplot',figure=fig)
])


if __name__ == '__main__':
     app.run_server(debug=True, threaded=True)
```
Notice we are passing the `figure` to the `dcc.Graph` 

Run the server to see what we have

![plotly-dash.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631886517787/JUVPMhizk.png)


# Callbacks and State Management

You might think that we need a way to interact with these dropdowns or input. This is what will do now. It is simple logic. Which it getting the value from and input and display it.

Create a new file and call it `callbacks.py`
- Importing the libraries and init the app
```python
import dash
from dash import dcc, html
from dash.dependencies import Input, Output


app = dash.Dash()
```
We have two new imports here. We use `Input` to get an element and `Output` to update an element 

-. Create the UI components 
```python
app.layout = html.Div([
    dcc.Input(
        id='input', 
        value='', 
        placeholder='Type what you want to see..',
        style={
            'padding': '5px 3px',
            'width': '90%',
        }
    ),
    html.P(id='text',children='')
])
```
Notice we have an `input` and a `P`. The most important thing is each element should have a unique `id`. We will use it later to get the element or update it.  

- Get the Input value and update the text
```python
@app.callback(
    Output('text', 'children'),
    [Input('input', 'value')]
)
def update_text(value):
    return 'You typed ... {}'.format(value)
```
The way we are controlling the state in Dash is by using Callbacks. 
the call back take an output and a list of inputs 
The Output and Input takes two parameters the first is the id of the element you want to grab and the second one is the property you want
`Output(id, property)`
each input in the inputs list will be passed to the `update_text` function. If you have 4 inputs you will have 4 params. the return of this function is the output of the callback. This means we will display the value of the input on the text output 

- Run the server 
```python
if __name__ == '__main__':
      app.run_server(debug=True, threaded=True)
```

5. See in action 
![key-to-learn.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631887648868/JBRw3IwSH.png)

# Extra
## Create Search App With Dash

```python
import dash 
from dash import dcc,html
from dash.dependencies import Input, Output

data = [
    'jone',
    'jane',
    'lorem',
    'text'
]

app = dash.Dash()

search = dcc.Input(
    placeholder='Enter a value...',
    type='text',
    value='',
    id='search'
)

all_items = []
for item in data:
    all_items.append(html.Li(children=item))

items_list = html.Ul(
    children=all_items,
    id='items_list'
)

app.layout = html.Div([
    html.H1('Key To Learn'),
    search,
    items_list
])

@app.callback(
    Output('items_list', 'children'),
    Input('search', 'value'),
)
def update_list(value):
    filtered_children = []
    for item in data:
        if item.startswith(value) or not value:
            filtered_children.append(html.Li(children=item))
    return filtered_children

if __name__ == '__main__':
    app.run_server(debug=True, threaded=True)
```




![search.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631888668127/MoUPQv3tta.png)

 








