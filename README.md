# FLASK + BOOTSTRAP FAST

#### STEP 1: Make the thing

```console
mkdir your_project
cd your_project
python3 -m venv venv
source venv/bin/activate
pip3 install flask
git init
heroku login
pip3 install gunicorn
pip3 freeze > requirements.txt
touch Procfile
```

#### STEP 2: Add this to Procfile

```
web: gunicorn app:server
```

#### STEP 3a: Add base.html (with bootstrap header to check if working)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
        integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <title>Document</title>
    {% block head %}{% endblock %}
</head>

<body>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand" href="#">Navbar</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavDropdown"
            aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavDropdown">
            <ul class="navbar-nav">
                <li class="nav-item active">
                    <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Features</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Pricing</a>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownMenuLink" role="button"
                        data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                        Dropdown link
                    </a>
                    <div class="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
                        <a class="dropdown-item" href="#">Action</a>
                        <a class="dropdown-item" href="#">Another action</a>
                        <a class="dropdown-item" href="#">Something else here</a>
                    </div>
                </li>
            </ul>
        </div>
    </nav>

    {% block body %}{% endblock %}
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"
        integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo"
        crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js"
        integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1"
        crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"
        integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM"
        crossorigin="anonymous"></script>
</body>

</html>

```
#### STEP 3b: Add index.html (with more bootstrap checking)

```html
{% extends 'base.html' %}

{% block head %}

{% endblock %}

{% block body %}
<h1>What is this? Todos for cats?! </h1>
{{ nums[1] }}
<button type="button" class="btn btn-outline-primary">Primary</button>
<button type="button" class="btn btn-outline-secondary">Secondary</button>
<button type="button" class="btn btn-outline-success">Success</button>
<button type="button" class="btn btn-outline-danger">Danger</button>
<button type="button" class="btn btn-outline-warning">Warning</button>
<button type="button" class="btn btn-outline-info">Info</button>
<button type="button" class="btn btn-outline-light">Light</button>
<button type="button" class="btn btn-outline-dark">Dark</button>
{% endblock %}
```

#### STEP 3c: Add fake cat.py file to confirm externals are working

```python
def question():
    return ['Pippin', 'Uli', 'MSKC']
```

#### STEP 3d: Add actual app.py

```python
from cats import question
import importlib
from flask import Flask, render_template, url_for
import dash
import dash_core_components as dcc
import dash_html_components as html

server = Flask(__name__)


nums = [1, 2, 3, 4, 5]

cats = question()


@server.route('/')
def index():
    return render_template('index.html', nums=cats)


app = dash.Dash(
    __name__,
    server=server,
    routes_pathname_prefix='/dash/'
)

# app.layout = html.Div("My Dash app")

app.layout = html.Div([
    html.H2('PICK A CITY, ANY CITY!'),
    dcc.Dropdown(
        id='dropdown',
        options=[{'label': i, 'value': i} for i in ['LA', 'NYC', 'MTL']],
        value='LA'
    ),
    html.Div(id='display-value')
])


@app.callback(dash.dependencies.Output('display-value', 'children'),
              [dash.dependencies.Input('dropdown', 'value')])
def display_value(value):
    return 'You have selected "{}"'.format(value)


if __name__ == '__main__':
    app.run_server(debug=True)

```

#### STEP 4: PUSH IT LIVEEEE

```
heroku create awesome_dashapp_name
git add .
git commit -m "WHEEE FLASK"
git push heroku master
heroku open
pip3 install dash==1.8.0
pip3 freeze > requirements.txt
git status
touch .gitignore
```

#### STEP 5: Add a gitignore: 
(should probably do this before pushing live but again, lil stition)

```
venv
history
*.pyc
.DS_Store
.env
```

---