## Implementation of Flask

- Install flask in current environment

```bash
pip install flask
```

- Import required environment

```python
from flask import Flask, request,render_template
from flask import g, Response
```
- Creating welcome page which renders landing page "index.html"

```python
'''Welcome Page'''
@app.route("/")
def welcome():
    return render_template('index.html')
```

- Sending data to a route address: 

```python
@app.route("/boot_ask_one")
def boot():
    
    '''prepare some data called INFO'''
                
    return render_template("boot_ask_one.html", info = INFO) 
    
```














