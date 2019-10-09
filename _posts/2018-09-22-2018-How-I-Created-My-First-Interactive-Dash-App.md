---
layout: post
title: How I Created my First Plotly-Dash App
author: Jess Chace
cover_image: plotly-dash.png
cover_alt: Plotly-Dash
categories: Python
description: How I Created my First Plotly-Dash App
tags:
- Python
- Plotly-Dash
excerpt_separator: <!--more-->
published: False
---

*How I created my first Plotly-Dash app.*

<!--more-->

Was plotting in Matplotlib for this project I was just working on

Have used Plotly in the past

Dash allows me to achieve front end usability without knowing any JavaScript or React.  

decided to give Dash a go because of the number of interactive features such as drop-down menus, click boxes, radio buttons, and sliders.

### Creating the App

To get started, here are the imports I used for the app.  The dash core components (dcc) are the features of the graph.  The dash html components (html) are the web aspect of the app.  

```python
import os
import dash
import dash_core_components as dcc
import dash_html_components as html
import plotly.graph_objs as go
import pandas as pd
```

Next, I needed to read my data using a familiar Python command.  Feel free to pull this .csv from my GitHub profile since the dataframe is set up for this particular app.  

```python
# Read in the data

districts = pd.read_csv("https://github.com/thedatasleuth/New-York-Congressional-Districts/blob/master/districts.csv?raw=True")

df = districts
```

there are two main parts to a Dash app: creating the figure and then adding the callbacks.  you can start creating the figure by adding .html Div.  you can also add a title like this.  

```python
# Get a list of all the years

years = districts['Year'].unique()

app.layout = html.Div([
    html.H2("Voter Registration by Count"),
    html.Div(
        [
            dcc.Dropdown(
                id="Year",
                options=[{
                    'label': i,
                    'value': i
                } for i in years],
                value='All Years'),
        ],
        style={'width': '25%',
               'display': 'inline-block'}),
    dcc.Graph(id='funnel-graph'),
])
```


The callbacks are what facilitate the interactive features.  Explain what each line is doing.

```python
@app.callback(
    dash.dependencies.Output('funnel-graph', 'figure'),
    [dash.dependencies.Input('Year', 'value')])

def update_graph(Year):
    if Year == "All Years":
        df_plot = df.copy()
    else:
        df_plot = df[df['Year'] == Year]

    trace1 = go.Bar(x=df_plot ['DISTRICT'],
                    y=df_plot [('DEM')],
                    name='Democratic Party',
                    marker=dict(color='rgb(3,67,223)'))
    trace2 = go.Bar(x=df_plot ['DISTRICT'],
                    y=df_plot [('REP')],
                    name='Republican Party',
                    marker=dict(color='rgb(229,0,0)'))
    trace3 = go.Bar(x=df_plot ['DISTRICT'],
                    y=df_plot [('CON')],
                    name='Conservative Party',
                    marker=dict(color='rgb(132,0,0)'))
    trace4 = go.Bar(x=df_plot ['DISTRICT'],
                    y=df_plot [('WOR')],
                    name='Working Families Party',
                    marker=dict(color='rgb(149,208,252)'))
    trace5 = go.Bar(x=df_plot ['DISTRICT'],
                    y=df_plot [('IND')],
                    name='Independent Party',
                    marker=dict(color='rgb(126,30,156)'))
    trace6 = go.Bar(x=df_plot ['DISTRICT'],
                    y=df_plot [('GRE')],
                    name='Green Party',
                    marker=dict(color='rgb(21,176,26)'))
    trace7 = go.Bar(x=df_plot ['DISTRICT'],
                    y=df_plot [('WEP')],
                    name="Women's Equality Party",
                    marker=dict(color='rgb(255,129,192)'))
    trace8 = go.Bar(x=df_plot ['DISTRICT'],
                    y=df_plot [('REF')],
                    name='Reform Party',
                    marker=dict(color='rgb(206,162,253)'))
    trace9 = go.Bar(x=df_plot ['DISTRICT'],
                    y=df_plot [('OTH')],
                    name='Other',
                    marker=dict(color='rgb(249,115,6)'))
    trace10 = go.Bar(x=df_plot ['DISTRICT'],
                     y=df_plot [('BLANK')],
                     name='Blank',
                     marker=dict(color='rgb(101,55,0)'))

    return {
        'data': [trace1, trace2, trace3, trace4, trace5,
                     trace6, trace7, trace8, trace9, trace10],
        'layout':
        go.Layout(
            title='{}'.format(Year),
            barmode='group',
            xaxis = dict(
                title='Districts'))
    }
```


This is the final part.  If I want to serve up the app locally, I use the following code to specify the host and port:

```python
if __name__ == '__main__':
    app.server.run(debug=True)
```

### Deploying the App

Was nervous about this part.  We spent a little bit of time on Flask but honestly it went in one ear and out the other.  There were some initial steps that I had to take into order to deploy the app to Heroku.  

Talk about steps to set up account.

Talk about setting up a virtual environment

Talk about pip installing everything again in the virtual environment

Talk about what gunicorn is.

Talk about creating a new Heroku app.  Heroku requires lower-case-letters-that-are-separated-by-dashes-and-not-underscores.

Then talk about the five files that I created in my new Heroku folder and the code that went into each file.  Talk about the requirements.txt file.  Maybe even talk about the SO questions I posted.  

.gitignore

```
venv
*.pyc
.DS_Store
.env
```

app.py

THe app

Procfile

```
web: gunicorn app:server
```

requirements.txt

```
Flask==1.0.2
gunicorn==19.9.0
dash==0.26.5
dash-core-components==0.29.0
dash-html-components==0.12.0
dash-renderer==0.13.2
plotly==3.2.1
pandas==0.23.1
pandas-datareader==0.6.0
```

runtime.txt

```
python-3.6.6
```

Because I was still in the folder I created for the Heroku app, it was then just a matter of git add, git commit, and git push like this:

```
git add .
git commit -m "Initial push"
git push heroku master
```

And that's it!  Head on over to the heroku url to view your new Dash app.
