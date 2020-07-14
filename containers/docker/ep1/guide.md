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

```
FROM python:3.7.3
```

Next we install required modules using the RUN instruction.

```
RUN pip3 install Flask
```

We need to EXPOSE the port we wish to use for our server.

```
EXPOSE 5000
```

Next we copy all the files for our web server into a project directory called web_serv.

```
WORKDIR /web_serv
COPY . .
```

Finally we give the CMD instruction to run the server.

```
CMD ["python3", "app.py"]
```

Complete Dockerfile

```
FROM python:3.7.3
RUN pip install Flask
EXPOSE 5000
WORKDIR /web_serv
COPY . .
CMD ["python", "app.py"]
```

#### Part 3. Building container image
To build a container image from a Dockerfile, we can use the builder provided with the docker program. See below for the command to use from 
the terminal to build.

```bash
docker build -t . web_server:1.0
```

**-t** Name and optionally a tag in the 'name:tag' format
**.** Dockerfile in current directory
**web_server** Name of image
**1.0** Tag

If we run the image list command we can find it stored in our local docker registry.

```bash
docker image ls
```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
web_server          1.0                 c7c37e472d31        2 weeks ago         1.22MB

#### Part 4. Launching the container
Running the container is easy just execute

```bash
docker run web_server:1.0
```

Check the status of the container

```bash
docker ps
```

CONTAINER ID        IMAGE                                   COMMAND             CREATED             STATUS              PORTS                    NAMES
71eec442b631        web_server:1.0                          "python app.py"     5 seconds ago       Up 4 seconds        0.0.0.0:5000->5000/tcp   keen_hamilton
