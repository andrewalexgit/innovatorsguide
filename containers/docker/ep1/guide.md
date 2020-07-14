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

The code (app.py):

```python
from flask import Flask, request

app = Flask(__name__)

@app.route("/show_text", methods=["POST"])
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

We can test out our server by running it locally

```bash
python3 app.py
```

Testing the show_text endpoint with cURL

```bash
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"text":"Hello world!"}' \
  http://localhost:5000/show_text
```

#### Part 2. Dockerfile
Dockerfile's contain instructions that execute asynchronously. Generally, every instruction builds a new layer of the Linux container image. To get started,
we will choose a pre-built base image to use for our web service. When pulling base images, we use the FROM instruction.
