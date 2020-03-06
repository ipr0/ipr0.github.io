### Simple API server using Flask microframework

```shell
python3 -m venv flask
cd flask
source bin/activate
pip install flask
```

`cake_server.py`
```python
from flask import Flask, json

cakes = {  
  "1": {
    "name": "Cheesecake",
    "ingredients": ["cheese", "flour", "sugar"],
  },
  "2": {
    "name": "Cherry Pie",
    "ingredients": ["cherries", "flour", "sugar"],
  },
} 
 

api = Flask(__name__)

@api.route('/cake/<id>',methods=['GET'])
def get_cake(id):
  return json.dumps(cakes[id])

if __name__ == "__main__":
  api.run()

```

```shell
python3 server.py
 * Serving Flask app "cake_server" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
127.0.0.1 - - [05/Mar/2020 18:58:14] "GET / HTTP/1.1" 404 -
127.0.0.1 - - [05/Mar/2020 18:58:27] "GET /cake HTTP/1.1" 404 -
127.0.0.1 - - [05/Mar/2020 18:58:31] "GET /cake/1 HTTP/1.1" 200 -
```
