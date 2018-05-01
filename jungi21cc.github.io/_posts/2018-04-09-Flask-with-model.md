---
layout: post
title: Flask with machine learning
tags: [Computer Science]

---

### Flask with machine learning

### 1. *flask*

```
$ touch dss.py
$ mkdir models
$ mkdir static
$ mkdir templates
$ cd templates
$ touch index.html
$ cd ..
```

#### - flask with dss.py

```python
from flask import Flask, render_template, request, jsonify
import pickle

app = Flask(__name__)
app.config.update(
    TEMPLATE_AUTO_RELOAD = True,

)

# load model
models = {}
def init():
    with open('./models/classification.plk', 'rb') as f:
        models['classification'] = pickle.load(f)

init()

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/predict/', methods = ['POST'])
def predict():
    classification_list = ['정치', '경제', '사회', '생활/문화', '세계', 'IT/과학']
    model = model['classification']

    sentence = request.values.get('sentence')

    predict_data = model.predict_proba([sentence])[0]
    result = {'status':200, 'result':list(predict_data), 'category':classification_list}
    return jsonify(result)


if __name__ == '__main__':
    app.run()

#run
#$ python3 dss.py
```

#### - index.html

```html
at ATOM
html ==> enter

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>my title</title>

  </head>
  <body>
  test body

  </body>
</html>


```

#### - templates via CSS

bootstrap - documentation - alert, buttons

highcharts - highcart demo - pie chart - html 3 scripts




***

*Reference*
