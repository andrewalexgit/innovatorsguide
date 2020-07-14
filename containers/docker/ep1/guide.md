# Container series - Docker chapter

#### Episode 1: Creating a simple web service container

We will create a Flask based web service, create a Dockerfile, build a Linux container image from docker, and run a container using the docker engine.

#### Part 1. Developing our Flask web service

Assuming you already have fundamental python knowledge, we will dive directly into setting up a minimal web service with the Flask module. To build our web 
service, we install the module using pip and then write our code.

**Note I'm using pip3 which is specific to my environment - yours might just be pip. **

```bash
pip3 install Flask
```

The code:

```python
from flask import Flask, request

app = Flask(__name__)

@app.route("/show_text", methods=["POST", "GET"])
def show_text():
  """Displays text sent via. POST request..."""
  
  data = request.get_json()
  return data["text"]
  
@app.route("/")
def index():
  """Index route..."""
  
  return "Simple web service container"
  
if __name__ == "__main__":
  app.run(host="0.0.0.0", port=5000)

````
